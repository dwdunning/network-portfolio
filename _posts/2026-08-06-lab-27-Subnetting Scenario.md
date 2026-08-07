---
layout: post
title: "Packet Tracer Lab 27 – Subnetting Scenario"
date: 2026-08-06
categories: [Networking, Cisco, Packet Tracer, Module-16-Routing]
tags: [networking, cisco, packet tracer, subnetting, ipv4, cidr, routing]
---

## Overview

This lab focused on designing and implementing an IPv4 subnetting scheme using the network **192.168.100.0/24**. The objective was to divide the network into multiple subnets capable of supporting at least 25 hosts each, assign the resulting addresses to routers, switches, and PCs, and verify end-to-end connectivity using the existing EIGRP routing configuration.

## Objectives

- Design an IPv4 subnetting scheme.
- Calculate subnet masks and host ranges.
- Assign IP addresses to routers, switches, and PCs.
- Configure router interfaces.
- Configure switch management interfaces.
- Configure a PC with static addressing.
- Verify connectivity throughout the network.

## Network Topology

![Packet Tracer subnetting topology.]({{ site.baseurl }}/assets/images/lab27/topology.png)

## Step 1 - Determine the Required Number of Subnets

The topology contains four LANs and one WAN link.

| Network | Purpose |
|----------|---------|
| R1 G0/0 LAN | PC1 and S1 |
| R1 G0/1 LAN | PC2 and S2 |
| R2 G0/0 LAN | PC3 and S3 |
| R2 G0/1 LAN | PC4 and S4 |
| Serial WAN | R1 ↔ R2 |

**Total required subnets:** **5**

## Step 2 - Borrow Host Bits

Starting network:

```text
192.168.100.0/24
```

To create at least five subnets:

| Borrowed Bits | Number of Subnets |
|--------------:|------------------:|
| 2 | 4 ❌ |
| 3 | 8 ✅ |

Therefore, **3 host bits** were borrowed.

New prefix:

```text
/27
```

## Step 3 - Calculate Hosts Per Subnet

A `/27` network leaves five host bits.

```
2^5 = 32 addresses
32 - 2 = 30 usable hosts
```

Each subnet supports **30 usable hosts**, satisfying the requirement for at least 25 hosts.

## Binary Subnet Calculation

| Subnet | Decimal | Binary |
|-------:|--------:|:-------|
| 0 | 0 | 00000000 |
| 1 | 32 | 00100000 |
| 2 | 64 | 01000000 |
| 3 | 96 | 01100000 |
| 4 | 128 | 10000000 |

## New Subnet Mask

Binary:

| Octet 1 | Octet 2 | Octet 3 | Octet 4 |
|----------|----------|----------|----------|
| 11111111 | 11111111 | 11111111 | 11100000 |

Decimal:

```text
255.255.255.224
```

CIDR notation:

```text
/27
```

## Complete Subnet Table

| Subnet | Network Address | First Host | Last Host | Broadcast |
|--------:|-----------------|------------|-----------|------------|
| 0 | 192.168.100.0 | 192.168.100.1 | 192.168.100.30 | 192.168.100.31 |
| 1 | 192.168.100.32 | 192.168.100.33 | 192.168.100.62 | 192.168.100.63 |
| 2 | 192.168.100.64 | 192.168.100.65 | 192.168.100.94 | 192.168.100.95 |
| 3 | 192.168.100.96 | 192.168.100.97 | 192.168.100.126 | 192.168.100.127 |
| 4 | 192.168.100.128 | 192.168.100.129 | 192.168.100.158 | 192.168.100.159 |
| 5 | 192.168.100.160 | 192.168.100.161 | 192.168.100.190 | 192.168.100.191 |
| 6 | 192.168.100.192 | 192.168.100.193 | 192.168.100.222 | 192.168.100.223 |
| 7 | 192.168.100.224 | 192.168.100.225 | 192.168.100.254 | 192.168.100.255 |

## Subnet Assignments

| Network | Assigned Subnet |
|----------|-----------------|
| R1 G0/0 LAN | Subnet 0 |
| R1 G0/1 LAN | Subnet 1 |
| R2 G0/0 LAN | Subnet 2 |
| R2 G0/1 LAN | Subnet 3 |
| R1 ↔ R2 WAN | Subnet 4 |

## Addressing Plan

| Device | Interface | IP Address | Mask | Default Gateway |
|--------|-----------|------------|------|-----------------|
| R1 | G0/0 | 192.168.100.1 | 255.255.255.224 | N/A |
| R1 | G0/1 | 192.168.100.33 | 255.255.255.224 | N/A |
| R1 | S0/0/0 | 192.168.100.129 | 255.255.255.224 | N/A |
| R2 | G0/0 | 192.168.100.65 | 255.255.255.224 | N/A |
| R2 | G0/1 | 192.168.100.97 | 255.255.255.224 | N/A |
| R2 | S0/0/0 | 192.168.100.158 | 255.255.255.224 | N/A |
| S1 | VLAN 1 | 192.168.100.2 | 255.255.255.224 | 192.168.100.1 |
| S2 | VLAN 1 | 192.168.100.34 | 255.255.255.224 | 192.168.100.33 |
| S3 | VLAN 1 | 192.168.100.66 | 255.255.255.224 | 192.168.100.65 |
| S4 | VLAN 1 | 192.168.100.98 | 255.255.255.224 | 192.168.100.97 |
| PC1 | NIC | 192.168.100.30 | 255.255.255.224 | 192.168.100.1 |
| PC2 | NIC | 192.168.100.62 | 255.255.255.224 | 192.168.100.33 |
| PC3 | NIC | 192.168.100.94 | 255.255.255.224 | 192.168.100.65 |
| PC4 | NIC | 192.168.100.126 | 255.255.255.224 | 192.168.100.97 |

## Configuring R1

The lab required configuring only the LAN interfaces on R1.

```text
enable
configure terminal

interface g0/0
 ip address 192.168.100.1 255.255.255.224
 no shutdown

interface g0/1
 ip address 192.168.100.33 255.255.255.224
 no shutdown

interface s0/0/0
 ip address 192.168.100.129 255.255.255.224
 clock rate 64000
 no shutdown
```

## Configuring S3

```text
enable
configure terminal

interface vlan 1
 ip address 192.168.100.66 255.255.255.224
 no shutdown

ip default-gateway 192.168.100.65
```

## Configuring PC4

| Setting | Value |
|----------|-------|
| IP Address | 192.168.100.126 |
| Subnet Mask | 255.255.255.224 |
| Default Gateway | 192.168.100.97 |

## Connectivity Verification

Connectivity testing verified both local and remote communication.

Successful tests included:

- PC4 → Default Gateway (192.168.100.97)
- PC4 → R1 G0/0 (192.168.100.1)

These successful pings confirmed that:

- Local subnet addressing was correct.
- Default gateways were configured correctly.
- EIGRP routing between R1 and R2 was functioning properly.
- End-to-end connectivity existed across multiple subnets.

## Assessment Results

The completed activity received a perfect score.

![Packet Tracer assessment results showing a score of 30/30.]({{ site.baseurl }}/assets/images/lab27/results.png)

- **Score:** 30/30
- **Items Completed:** 13/13
- **IPv4 Host Address Calculation:** 11/11
- **IPv4 Subnet Mask Calculation:** 11/11
- **Device Interface Configuration:** 3/3
- **Default Gateway Configuration:** 5/5

## What I Learned

This lab reinforced the relationship between subnet calculations and practical device configuration. By borrowing three host bits from a `/24` network, I created eight `/27` subnets that each supported 30 usable hosts while meeting the design requirements. I also practiced assigning addresses systematically, configuring router and switch interfaces, setting default gateways, and verifying that routing protocols could successfully forward traffic between multiple subnets.
