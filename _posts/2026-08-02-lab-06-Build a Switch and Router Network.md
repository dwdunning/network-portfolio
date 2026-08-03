---
layout: post
title: "Packet Tracer Lab 06 – Build a Switch and Router Network"
date: 2026-08-02
categories: [Networking, Cisco, Packet Tracer]
tags: [networking, cisco, packet tracer, routing, switching, ssh]
categories: [module-11]
---

## Overview

In this lab, I built a small routed network consisting of a router, a switch, and two PCs. The objectives were to configure IP addressing, establish connectivity between two separate networks, configure secure device access, and enable remote management using SSH.

## Objectives

- Connect the network using the correct Ethernet cables.
- Configure static IP addresses on both PCs.
- Configure the router interfaces and basic security.
- Configure the switch management interface (VLAN 1).
- Verify end-to-end connectivity.
- Configure SSH for secure remote management.
- Verify the completed configuration.

## Network Topology

The completed topology consisted of:

- One Cisco ISR4321 router (R1)
- One Cisco switch (S1)
- Two PCs (PC-A and PC-B)

![Completed network topology.]({{ site.baseurl }}/assets/images/lab06/topology.png)

## Router Configuration

The router was configured with:

- Hostname
- Enable secret
- Console password
- Password encryption
- MOTD banner
- IP addresses on both Gigabit Ethernet interfaces
- SSH configuration
- RSA keys
- Local SSH user
- Startup configuration saved to NVRAM

After configuration, both router interfaces were operational.

![Router interface status.]({{ site.baseurl }}/assets/images/lab06/ip-brief.png)

The `show ip interface brief` command confirmed:

- GigabitEthernet0/0/0 — **192.168.0.1**
- GigabitEthernet0/0/1 — **192.168.1.1**
- Both interfaces were **up/up**, confirming Layer 1 and Layer 2 connectivity.

## Switch Configuration

The switch was configured through the console connection with:

- Hostname
- Enable secret
- Console password
- Password encryption
- MOTD banner
- VLAN 1 management interface
- Default gateway
- Saved startup configuration

## Troubleshooting

This lab required more troubleshooting than the previous Packet Tracer activities.

Initially, the router interfaces appeared as:

```text
Status: up
Protocol: down
```

This indicated that the router interfaces were enabled but were not detecting an active Ethernet connection.

Using Packet Tracer's **Assessment Items**, I discovered that although the correct cable type had been used, the cables were connected to the wrong router interfaces.

The correct connections were:

- **R1 GigabitEthernet0/0/0 → PC-B FastEthernet0**
- **R1 GigabitEthernet0/0/1 → S1 GigabitEthernet0/1**

After correcting the cabling, both interfaces transitioned to **up/up**, connectivity was restored, and the remainder of the lab completed successfully.

This reinforced an important troubleshooting lesson: if an interface reports **up/down**, verify the physical connections before assuming the IP configuration is incorrect.

## Results

The completed lab received a perfect score.

![Packet Tracer assessment results.]({{ site.baseurl }}/assets/images/lab06/results.png)

- **Score:** 40/40
- **IP Configuration:** 13/13
- **Other Configuration:** 15/15
- **Physical Configuration:** 12/12

## Skills Practiced

- Cisco IOS CLI
- Static IPv4 addressing
- Router interface configuration
- Switch management configuration
- VLAN 1 management interface
- Console access
- Password security
- SSH configuration
- RSA key generation
- Network troubleshooting
- Physical layer verification
- Configuration verification using `show` commands

## Reflection

This was one of the most valuable Packet Tracer labs so far because it combined router configuration, switch configuration, physical cabling, and troubleshooting into a single activity. The largest challenge was diagnosing why the router interfaces remained **up/down** despite correct IP addressing. Working through the assessment results helped isolate the issue to incorrect physical connections rather than configuration errors, reinforcing the importance of verifying Layer 1 and Layer 2 before troubleshooting higher network layers
