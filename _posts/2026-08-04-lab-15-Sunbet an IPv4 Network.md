---
layout: post
title: "Packet Tracer Lab 15 – Subnet an IPv4 Network"
date: 2026-08-04
categories: [Networking, Cisco, Packet Tracer, Module-13-IP-Addressing]
tags: [networking, cisco, packet tracer, ipv4, subnetting, routing, switching]
---

## Overview

In this lab, I designed and implemented an IPv4 subnetting scheme for a customer network using the `192.168.0.0/24` address space. The objective was to create multiple equal-sized subnets that satisfied the current host requirements while allowing room for future expansion. After determining the appropriate subnet mask, I configured the router, switches, and PCs before verifying end-to-end connectivity.

## Objectives

- Design an IPv4 subnetting scheme.
- Configure router interfaces.
- Configure switch management interfaces.
- Configure PC addressing.
- Verify network connectivity with ping.

## Network Topology

![Completed network topology.]({{ site.baseurl }}/assets/images/lab15/topology.png)

## Designing the Subnet Scheme

### Requirements

- LAN-A requires **50 hosts**
- LAN-B requires **40 hosts**
- At least **2 additional subnets** for future expansion
- All subnets must use the **same subnet mask**

### Selecting the Subnet Mask

| Prefix  | Subnets | Usable Hosts |
| :------ | ------: | -----------: |
| /25     |       2 |          126 |
| **/26** |   **4** |       **62** |
| /27     |       8 |           30 |

A **/26 (255.255.255.192)** subnet mask satisfies both requirements:

- Four subnets
- Sixty-two usable hosts per subnet

### Subnets Created

| Subnet           | Host Range                   | Broadcast    |
| :--------------- | :--------------------------  | :----------- |
| 192.168.0.0/26   | 192.168.0.1 – 192.168.0.62   | 192.168.0.63 |
| 192.168.0.64/26  | 192.168.0.65 – 192.168.0.126 | 192.168.0.127|
| 192.168.0.128/26 | 192.168.0.129 – 192.168.0.190| 192.168.0.191|
| 192.168.0.192/26 | 192.168.0.193 – 192.168.0.254| 192.168.0.255|

## Addressing Plan

| Device         | Interface | Address         |
| :------------- | :-------- | :-------------- |
| CustomerRouter | G0/0      | 192.168.0.1/26  |
| CustomerRouter | G0/1      | 192.168.0.65/26 |
| LAN-A Switch   | VLAN 1    | 192.168.0.2/26  |
| LAN-B Switch   | VLAN 1    | 192.168.0.66/26 |
| PC-A           | NIC       | 192.168.0.62/26 |
| PC-B           | NIC       | 192.168.0.126/26|

## Router Configuration

The customer router was configured with:

- Hostname
- Enable secret
- Console password
- IPv4 addresses on both LAN interfaces
- Saved startup configuration

![CustomerRouter CLI configuration.]({{ site.baseurl }}/assets/images/lab15/router.png)

## Switch Configuration

Each switch received a management IP address on VLAN 1 along with the correct default gateway.

### LAN-A Switch

![LAN-A switch configuration.]({{ site.baseurl }}/assets/images/lab15/vlan1.png)

### LAN-B Switch

![LAN-B switch configuration.]({{ site.baseurl }}/assets/images/lab15/vlan1b.png)

## PC Configuration

### PC-A

![PC-A IPv4 configuration.]({{ site.baseurl }}/assets/images/lab15/PC-A.png)

### PC-B

![PC-B IPv4 configuration.]({{ site.baseurl }}/assets/images/lab15/PC-B.png)

## Connectivity Testing

### PC-A to Default Gateway

The gateway responded successfully.

![PC-A pinging its default gateway.]({{ site.baseurl }}/assets/images/lab15/ping-A.png)

### PC-B to Default Gateway

The gateway also responded successfully.

![PC-B pinging its default gateway.]({{ site.baseurl }}/assets/images/lab15/ping-B.png)

### PC-A to PC-B

Communication between both LANs succeeded through the router.

One interesting observation was the **TTL value of 127**, indicating that the packets traversed a router before reaching the destination.

![PC-A successfully pinging PC-B.]({{ site.baseurl }}/assets/images/lab15/ping-AtoB.png)

## Assessment Results

Packet Tracer reported a perfect score.

- **23 / 23 items completed**
- **100% correct**

![Packet Tracer assessment results.]({{ site.baseurl }}/assets/images/lab15/results.png)

## What I Learned

This lab reinforced the relationship between subnet masks, available hosts, and the number of subnets created. Although a `/25` subnet would have supported enough hosts, it produced only two subnets. Borrowing one additional bit to create a `/26` network generated four subnets while still providing 62 usable host addresses in each subnet.

I also became more comfortable assigning management IP addresses to switches, configuring router interfaces, and verifying connectivity between multiple LANs using ICMP. Watching the TTL decrease after crossing the router helped reinforce how packets move between different networks.
