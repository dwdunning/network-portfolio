---
layout: post
title: "Packet Tracer Lab 29 – VLSM Design and Implementation Practice"
date: 2026-08-07
categories: [Networking, Cisco, Packet Tracer, Module-16-Network Systems]
tags: [networking, cisco, packet tracer, vlsm, subnetting, ipv4, cidr, routing]
---

## Overview

In this lab, I designed and implemented a Variable Length Subnet Masking (VLSM) addressing scheme for a network containing four LANs and a point-to-point WAN connection.

The assigned network was `10.11.48.0/24`. Each LAN required a different number of host addresses, so using the same subnet mask everywhere would waste address space. VLSM allowed me to divide the `/24` network into appropriately sized subnets based on the actual host requirements.

The lab required me to:

- Determine how many subnets were needed
- Select an appropriate subnet mask for each network
- Allocate the VLSM subnets from largest to smallest
- Document the network, usable host, and broadcast addresses
- Assign addresses to routers, switches, and hosts
- Configure the required interfaces
- Verify end-to-end connectivity

![Lab 29 Network Topology]({{ '/assets/images/lab29/topology.png' | relative_url }})

## Part 1 - Examine the Network Requirements

The starting network was:

`10.11.48.0/24`

The topology had the following requirements:

| Network | Required Hosts |
|---|---:|
| ASW-1 LAN | 14 |
| ASW-2 LAN | 30 |
| ASW-3 LAN | 6 |
| ASW-4 LAN | 60 |
| Building1-Building2 WAN | 2 |

Because there are four LANs and one WAN connection, the topology requires a total of **5 subnets**.

### Determine the Required Subnet Masks

For each network, I selected the smallest subnet capable of supporting the required number of usable host addresses.

| Network | Hosts Needed | CIDR | Subnet Mask | Usable Hosts |
|---|---:|---:|---|---:|
| ASW-1 LAN | 14 | `/28` | `255.255.255.240` | 14 |
| ASW-2 LAN | 30 | `/27` | `255.255.255.224` | 30 |
| ASW-3 LAN | 6 | `/29` | `255.255.255.248` | 6 |
| ASW-4 LAN | 60 | `/26` | `255.255.255.192` | 62 |
| Building1-Building2 WAN | 2 | `/30` | `255.255.255.252` | 2 |

The ASW-4 LAN required the largest subnet because it needed 60 host addresses. A `/26` provides 64 total addresses, with 62 available for hosts after excluding the network and broadcast addresses.

The point-to-point WAN required only two usable addresses, making a `/30` appropriate.

## Part 2 - Design the VLSM Addressing Scheme

With VLSM, I allocated the address space beginning with the largest network and continued toward the smallest.

The allocation order was:

1. ASW-4 LAN - 60 hosts
2. ASW-2 LAN - 30 hosts
3. ASW-1 LAN - 14 hosts
4. ASW-3 LAN - 6 hosts
5. Building1-Building2 WAN - 2 hosts

### VLSM Subnet Table

Starting at `10.11.48.0`, the resulting VLSM addressing scheme was:

| Subnet Description | Hosts Needed | Network Address/CIDR | First Usable Host | Broadcast Address |
|---|---:|---|---|---|
| ASW-4 LAN | 60 | `10.11.48.0/26` | `10.11.48.1` | `10.11.48.63` |
| ASW-2 LAN | 30 | `10.11.48.64/27` | `10.11.48.65` | `10.11.48.95` |
| ASW-1 LAN | 14 | `10.11.48.96/28` | `10.11.48.97` | `10.11.48.111` |
| ASW-3 LAN | 6 | `10.11.48.112/29` | `10.11.48.113` | `10.11.48.119` |
| Building1-Building2 WAN | 2 | `10.11.48.120/30` | `10.11.48.121` | `10.11.48.123` |

The usable ranges for each subnet were therefore:

| Subnet | Usable Address Range |
|---|---|
| ASW-4 | `10.11.48.1 - 10.11.48.62` |
| ASW-2 | `10.11.48.65 - 10.11.48.94` |
| ASW-1 | `10.11.48.97 - 10.11.48.110` |
| ASW-3 | `10.11.48.113 - 10.11.48.118` |
| WAN | `10.11.48.121 - 10.11.48.122` |

This allocation uses only the address space required by each network rather than dividing the entire `/24` into equal-sized subnets.

## Addressing Scheme

The lab specified how addresses should be assigned within each subnet:

- Building1 receives the first usable address on its LANs.
- Building1 receives the first usable address on the WAN.
- Building2 receives the first usable address on its LANs.
- Building2 receives the last usable address on the WAN.
- Switches receive the second usable address in their LAN.
- Hosts receive the last usable address in their LAN.

Using those rules produced the following addressing table.

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| Building1 | G0/0 | `10.11.48.97` | `255.255.255.240` | N/A |
| Building1 | G0/1 | `10.11.48.65` | `255.255.255.224` | N/A |
| Building1 | S0/0/0 | `10.11.48.121` | `255.255.255.252` | N/A |
| Building2 | G0/0 | `10.11.48.113` | `255.255.255.248` | N/A |
| Building2 | G0/1 | `10.11.48.1` | `255.255.255.192` | N/A |
| Building2 | S0/0/0 | `10.11.48.122` | `255.255.255.252` | N/A |
| ASW-1 | VLAN 1 | `10.11.48.98` | `255.255.255.240` | `10.11.48.97` |
| ASW-2 | VLAN 1 | `10.11.48.66` | `255.255.255.224` | `10.11.48.65` |
| ASW-3 | VLAN 1 | `10.11.48.114` | `255.255.255.248` | `10.11.48.113` |
| ASW-4 | VLAN 1 | `10.11.48.2` | `255.255.255.192` | `10.11.48.1` |
| Host-A | NIC | `10.11.48.110` | `255.255.255.240` | `10.11.48.97` |
| Host-B | NIC | `10.11.48.94` | `255.255.255.224` | `10.11.48.65` |
| Host-C | NIC | `10.11.48.118` | `255.255.255.248` | `10.11.48.113` |
| Host-D | NIC | `10.11.48.62` | `255.255.255.192` | `10.11.48.1` |

## Part 3 - Configure the Devices

Most of the network was already configured in the Packet Tracer activity. I was responsible for completing three areas:

1. Building1 LAN interfaces
2. ASW-3 management addressing
3. Host-D addressing

### Configure Building1 G0/0

The ASW-1 LAN uses:

`10.11.48.96/28`

Building1 receives the first usable address, `10.11.48.97`.

```text
Building1> enable
Building1# configure terminal
Building1(config)# interface gigabitethernet0/0
Building1(config-if)# ip address 10.11.48.97 255.255.255.240
Building1(config-if)# no shutdown
