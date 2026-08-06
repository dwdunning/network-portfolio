---
layout: post
title: "Packet Tracer Lab 24 – Compare In-Band and Out-of-Band Management Access"
date: 2026-08-06
categories: [Networking, Cisco, Packet-Tracer, Module-15-Routing-Network-Assessment]
tags: [networking, cisco, packet tracer, ssh, console, usb console, in-band management, out-of-band management]
---

## Overview

This lab compared two common methods of managing Cisco network devices:

- **Out-of-band management**, using console and USB console connections that do not require network connectivity.
- **In-band management**, using SSH across an operational IP network.

The activity demonstrated how administrators can perform initial device configuration locally before enabling secure remote administration over the network.

## Objectives

- Establish an out-of-band console connection.
- Connect to a router using a USB console connection.
- Establish secure in-band management using SSH.
- Compare the differences between local and remote device management.

## Network Topology

![Packet Tracer topology for Lab 24.]({{ site.baseurl }}/assets/images/lab24/topology.png)

## Part 1 – Out-of-Band Management

The first portion of the lab focused on connecting directly to Cisco devices without relying on the network.

### Console Connection

A light blue console cable was connected between the PC's **RS-232** port and the router's **Console** port.

After powering on the router and opening the Terminal application, the initial configuration dialog was skipped.

The router presented the following prompt:

```text
Router>
```

### Switch Console Access

The console cable was then moved from the router to the switch.

Opening the Terminal again displayed the switch prompt:

```text
Switch>
```

### USB Console Connection

Finally, a USB console cable was connected between the PC's **USB0** port and the **USB Console** port on router **East**.

Opening the Terminal provided direct console access to the router:

```text
East>
```

This demonstrated that both traditional serial console and modern USB console connections provide local management without requiring IP connectivity.

## Part 2 – In-Band Management

With the routers already configured, management shifted to secure remote access using SSH.

### SSH from East to West

From the East router console, an SSH session was established to the West router:

```text
ssh -l admin 64.100.1.1
```

After entering the password:

```text
class
```

the session successfully connected to the remote router.

Prompt received:

```text
West#
```

### SSH from the PC to East

After obtaining an IP address through DHCP, the PC established an SSH session directly to the East router.

Command used:

```text
ssh -l admin 209.165.200.226
```

After authenticating with:

```text
class
```

the remote session opened successfully.

Prompt received:

```text
East#
```

## Key Differences

| Out-of-Band Management | In-Band Management |
|-------------------------|--------------------|
| Uses console or USB console cables | Uses Ethernet network connectivity |
| Does not require IP addressing | Requires IP addressing and network connectivity |
| Ideal for initial configuration and troubleshooting | Ideal for routine remote administration |
| Requires physical access | Can be performed remotely |
| Uses Terminal software | Uses protocols such as SSH |

## Results

The lab was completed successfully.

![Packet Tracer assessment results.]({{ site.baseurl }}/assets/images/lab24/results.png)

## SSH Verification

The successful SSH session from the PC to the East router confirmed that secure in-band management was functioning correctly.

![Successful SSH session to the East router.]({{ site.baseurl }}/assets/images/lab24/bounds.png)

## What I Learned

This lab clearly illustrated the distinction between out-of-band and in-band management.

Console and USB console connections provide dependable local access even when a network is unavailable, making them essential for initial deployment and recovery. Once network connectivity is established, SSH provides encrypted remote administration, allowing devices to be managed securely without physical access.

Understanding when to use each management method is an important skill for both Cisco networking and the CompTIA Network+ certification.
