---
layout: post
title: "Packet Tracer Lab 07 - Connect a Wired and Wireless LAN"
date: 2026-08-02
categories: [Cisco, Packet Tracer, Networking]
tags: [packet tracer, cisco, networking, lan, wan, cabling, network+, ccna]
categories: [module-11]
---

## Overview

In this lab, I connected a small network consisting of enterprise and home networking equipment using the appropriate cable types for each interface. Unlike previous labs that focused on Cisco IOS configuration, this activity emphasized selecting the correct physical media based on the interfaces being connected.

The completed topology included routers, a switch, a cable modem, a wireless router, wired and wireless hosts, and a simulated cloud representing the ISP connection.

## Objectives

- Connect devices using the appropriate cable types.
- Verify network connectivity between devices.
- Access Router0 through the console port.
- Verify interface status using Cisco IOS.
- Explore the Packet Tracer physical topology.

---

## Building the Network

The network required several different connection types:

- Copper straight-through Ethernet
- Copper crossover Ethernet
- Serial WAN
- Coaxial
- Console
- Fiber optic

One of the more interesting aspects of this lab was determining the correct cable by examining the interface rather than assuming a cable type based solely on the devices being connected. For example, Router1's FastEthernet1/0 interface used a **100Base-FX fiber module**, requiring a fiber-optic cable instead of a copper Ethernet cable.

After selecting the correct cable for each connection, all link lights transitioned to their operational state.

![Completed network topology.]({{ site.baseurl }}/assets/images/lab07/topology.png)

---

## Verifying Connectivity

After completing the physical connections, I verified communication across the network.

Tests included:

- Pinging **netacad.pka** from the Family PC
- Pinging the switch (172.16.0.2) from the Home PC
- Opening the NetAcad web page from the Family PC

The first ping to the switch experienced brief timeouts while ARP resolved the destination MAC address, after which the remaining replies were successful.

---

## Verifying Router Interfaces

Using the console connection, I accessed Router0 and verified interface status with:

```text
show ip interface brief
```

The output confirmed that every connected interface was operational (`up/up`).

![Router0 interface status.]({{ site.baseurl }}/assets/images/lab07/ipbrief.png)

---

## Results

Packet Tracer confirmed that every required connection had been completed correctly.

- **100% Completion**
- **80 / 80 Points**
- **16 / 16 Connections Correct**

![Packet Tracer assessment results.]({{ site.baseurl }}/assets/images/lab07/results.png)

---

## What I Learned

This lab reinforced that selecting the correct cable depends on the **physical interface**, not simply the type of device being connected. While many Ethernet connections use copper straight-through cables, examining the actual interface is essential because different hardware modules may require fiber, serial, coaxial, or console connections instead.

I also gained additional experience verifying network connectivity using ICMP, accessing Cisco devices through the console port, and confirming interface status using Cisco IOS diagnostic commands.

Overall, this lab provided a solid introduction to identifying network media and physically assembling a mixed wired and wireless network.
