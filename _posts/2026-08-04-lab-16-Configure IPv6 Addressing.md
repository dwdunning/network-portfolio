---
layout: post
title: "Packet Tracer Lab 16 – Configure IPv6 Addressing"
date: 2026-08-04
categories: [Networking, Cisco, Packet Tracer, Module-13-Network-Addressing]
tags: [networking, cisco, packet tracer, ipv6, addressing, router, subnetting]
---

## Overview

In this lab, I configured IPv6 addressing throughout a small enterprise network. The activity included enabling IPv6 routing on a Cisco router, assigning global unicast and link-local addresses to router interfaces, configuring IPv6 addresses on servers and client PCs, and verifying end-to-end connectivity.

## Objectives

- Enable IPv6 routing on a Cisco router.
- Configure IPv6 global unicast and link-local addresses.
- Configure IPv6 addressing on servers and client PCs.
- Verify IPv6 connectivity using a web browser and ping.
- Save and validate the router configuration.

## Network Topology

![Lab topology.]({{ site.baseurl }}/assets/images/lab16/topology.png)

The network consists of:

- One Cisco router (R1)
- Two LANs connected through Gigabit Ethernet interfaces
- Two switches
- Two servers
- Four client PCs
- One ISP connection over a serial interface

Each LAN uses its own `/64` IPv6 network.

| Network | Prefix |
|---------|--------|
| Sales/Billing/Accounting | `2001:db8:1:1::/64` |
| Design/Engineering/CAD | `2001:db8:1:2::/64` |
| ISP Link | `2001:db8:1:a001::/64` |

## Router Configuration

The first task was enabling IPv6 packet forwarding on R1.

```text
ipv6 unicast-routing
```

Each interface then received:

- A global unicast IPv6 address
- A manually assigned link-local address (`fe80::1`)
- Administrative activation with `no shutdown`

Configured interfaces:

| Interface | IPv6 Address |
|-----------|--------------|
| GigabitEthernet0/0 | `2001:db8:1:1::1/64` |
| GigabitEthernet0/1 | `2001:db8:1:2::1/64` |
| Serial0/0/0 | `2001:db8:1:a001::2/64` |

The router configuration was verified before being saved.

![Router IPv6 configuration verification.]({{ site.baseurl }}/assets/images/lab16/router.png)

## Server Configuration

Both servers were configured with static IPv6 addresses.

### Accounting Server

| Setting | Value |
|---------|-------|
| IPv6 Address | `2001:db8:1:1::4/64` |
| Default Gateway | `fe80::1` |

![Accounting server IPv6 configuration.]({{ site.baseurl }}/assets/images/lab16/accounting.png)

### CAD Server

| Setting | Value |
|---------|-------|
| IPv6 Address | `2001:db8:1:2::4/64` |
| Default Gateway | `fe80::1` |

![CAD server IPv6 configuration.]({{ site.baseurl }}/assets/images/lab16/CAD.png)

## Client Configuration

Each workstation received a static IPv6 address within its subnet.

| Device | IPv6 Address |
|--------|--------------|
| Sales | `2001:db8:1:1::2/64` |
| Billing | `2001:db8:1:1::3/64` |
| Design | `2001:db8:1:2::2/64` |
| Engineering | `2001:db8:1:2::3/64` |

All clients used the router's link-local address (`fe80::1`) as their default gateway.

## Connectivity Testing

Connectivity was verified by:

- Accessing the Accounting web server
- Accessing the CAD web server
- Pinging the ISP IPv6 address
- Reviewing Packet Tracer assessment results

These tests confirmed successful routing between both IPv6 LANs and the ISP network.

## Results

Packet Tracer reported a perfect score.

![Packet Tracer assessment results.]({{ site.baseurl }}/assets/images/lab16/results.png)

- Score: **100/100**
- Assessment Items: **32/32**
- IPv6 Address Configuration: **30/30**
- Routing: **1/1**
- Other: **1/1**

## What I Learned

This lab reinforced several important IPv6 concepts:

- IPv6 routing must be enabled with `ipv6 unicast-routing`.
- Interfaces require both a global unicast address and a link-local address.
- Link-local addresses can serve as the default gateway for hosts.
- IPv6 addressing follows a hierarchical structure using `/64` networks.
- Verifying configurations with `show ipv6 interface brief` helps identify addressing mistakes before testing connectivity.
- Saving the configuration ensures IPv6 settings persist after a reboot.

## Conclusion

This lab demonstrated a complete IPv6 deployment on a small routed network. After configuring the router, servers, and client devices, all systems successfully communicated using IPv6, and Packet Tracer verified the implementation with a perfect assessment score.
