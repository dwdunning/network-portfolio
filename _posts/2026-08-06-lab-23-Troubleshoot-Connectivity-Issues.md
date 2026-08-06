---
layout: post
title: "Packet Tracer Lab 23 – Troubleshoot Connectivity Issues"
date: 2026-08-06
categories: [Networking, Cisco, Packet Tracer, Module-15-Routing]
tags: [networking, cisco, packet tracer, troubleshooting, routing, ip-addressing, ssh]
---

## Overview

This Packet Tracer lab focused on troubleshooting connectivity issues rather than configuring a network from scratch. Multiple users reported they could not access the web server after a network upgrade. My task was to identify the source of the problems, correct any issues within my control, and document anything that would require escalation.

The troubleshooting process involved validating host IP configurations, testing connectivity with `ping`, verifying web access, and using SSH to inspect the router configuration.

## Objectives

- Verify host IP addressing
- Troubleshoot connectivity using `ping`
- Verify DNS and web server access
- Access a router using SSH
- Identify and correct router interface configuration errors
- Validate end-to-end connectivity

## Network Topology

![Packet Tracer topology for Lab 23.]({{ site.baseurl }}/assets/images/lab23/topology.png)

## Troubleshooting Process

### Issue 1 – Incorrect IP Address on PC-01

The first step was verifying the IP configuration of PC-01 using the `ipconfig` command.

The IPv4 address was incorrectly configured as:

- **172.168.1.3**

The correct address should have been:

- **172.16.1.3**

After correcting the address in the Desktop > IP Configuration window, the workstation successfully communicated with the default gateway and external web server.

![Correcting the IP address on PC-01.]({{ site.baseurl }}/assets/images/lab23/pc1ipfix.png)

### Issue 2 – Incorrect Default Gateway on PC-02

PC-02 contained a valid IP address but an incorrect default gateway.

Incorrect gateway:

- **172.16.1.11**

Correct gateway:

- **172.16.1.1**

Once corrected, PC-02 successfully reached the local network and external resources.

### Issue 3 – Incorrect Router Interface Address

Both PC-A and PC-B could communicate with one another but were unable to reach their default gateway or any remote networks.

Initial testing showed:

- PCs on 172.16.1.0/24 could communicate normally.
- PCs on 172.16.2.0/24 could only communicate locally.
- The default gateway for the second network was unreachable.

Using SSH to access R1 allowed inspection of the router interfaces.

The `show ip interface brief` command revealed that GigabitEthernet0/1 was configured with the wrong IP address.

Expected:

| Interface | Correct Address |
|-----------|-----------------|
| G0/1 | 172.16.2.1 |

Actual:

| Interface | Incorrect Address |
|-----------|-------------------|
| G0/1 | 172.16.3.1 |

Correcting the interface address immediately restored connectivity between both LANs and the web server.

## Connectivity Testing

Throughout the troubleshooting process I used `ping` to verify connectivity between:

- Hosts and their default gateways
- Hosts on different LANs
- The external web server
- End-to-end routing after corrections

These tests clearly isolated each problem before any configuration changes were made.

![Connectivity testing during troubleshooting.]({{ site.baseurl }}/assets/images/lab23/ping.png)

## Verification

After correcting all three configuration errors:

- PC-01 communicated normally.
- PC-02 communicated normally.
- PC-A reached its default gateway.
- Both LANs communicated successfully.
- The web server was reachable using both its IP address and DNS name.
- The Packet Tracer assessment reached **100% completion**.

![Packet Tracer assessment results showing full completion.]({{ site.baseurl }}/assets/images/lab23/results.png)

## What I Learned

This lab emphasized systematic troubleshooting instead of simply entering configuration commands. Rather than assuming the router was the problem, I verified each device individually, beginning with host IP addressing, then validating connectivity using ping, and finally inspecting the router through SSH.

One incorrect IP address, one incorrect default gateway, and one incorrect router interface address were enough to prevent full network communication. Using a structured troubleshooting process made each issue easy to isolate and resolve.

This exercise reinforced the importance of verifying host configurations before assuming network infrastructure is at fault and demonstrated how basic diagnostic tools can quickly identify routing and addressing problems.
