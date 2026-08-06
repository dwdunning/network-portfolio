---
layout: post
title: "Packet Tracer Lab 22 - Troubleshoot Default Gateway Issues"
date: 2026-08-06
categories: [Networking, Cisco, Packet Tracer, Module-15-Routing]
tags: [networking, cisco, packet tracer, troubleshooting, default-gateway, routing, switches]
---

## Overview

This lab focused on troubleshooting connectivity problems caused by incorrect or missing default gateway configurations. Using a structured troubleshooting process, I verified local connectivity, identified configuration issues, implemented corrections one at a time, and verified that end-to-end communication was restored.

## Objectives

- Complete the network documentation by identifying the correct default gateways.
- Verify local connectivity within each subnet.
- Test end-to-end connectivity between networks.
- Troubleshoot and correct addressing and gateway configuration issues.
- Verify that all devices could communicate successfully.

## Network Topology

![Network topology for Lab 22.]({{ site.baseurl }}/assets/images/lab22/topology.png)

## Initial Configuration

The network consisted of two /24 LANs connected by a router.

| Network | Router Interface | Default Gateway |
| ------- | ---------------- | --------------- |
| 192.168.10.0/24 | R1 G0/0 | 192.168.10.1 |
| 192.168.11.0/24 | R1 G0/1 | 192.168.11.1 |

The following default gateways were documented and configured:

| Device | Default Gateway |
| ------- | --------------- |
| PC1 | 192.168.10.1 |
| PC2 | 192.168.10.1 |
| PC3 | 192.168.11.1 |
| PC4 | 192.168.11.1 |
| S1 | 192.168.10.1 |
| S2 | 192.168.11.1 |

## Troubleshooting Process

Following the recommended methodology, I verified one problem at a time instead of making multiple changes simultaneously.

### Issue 1 - Incorrect IP Address on PC1

The first connectivity test failed because PC1 had an incorrect IPv4 address. The address was changed from the 192.168.11.0/24 network to the correct 192.168.10.0/24 network.

### Correct configuration

- IP Address: 192.168.10.10
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.10.1

After correcting the address, local connectivity between PC1 and PC2 was restored.

### Issue 2 - Default Gateway Configuration

The default gateway information was completed for all hosts and switches based on the router interfaces attached to each subnet.

This allowed traffic destined for remote networks to be forwarded correctly through R1.

### Issue 3 - Missing Management Address on S2

While testing management connectivity, PC3 could not reach switch S2.

Using:

```text
show ip interface brief
```

revealed that **VLAN 1** had no IP address assigned.

The management interface was configured with:

```text
interface vlan 1
ip address 192.168.11.2 255.255.255.0
no shutdown
```

After assigning the management IP, S2 became reachable from PC3.

## Connectivity Verification

The following tests verified successful connectivity after troubleshooting:

- PC1 → PC2
- PC1 → R1
- PC1 → PC4
- PC1 → S1
- PC3 → PC4
- PC3 → S2

The first ping to a new device occasionally timed out because of ARP resolution, which is expected behavior in Packet Tracer. Subsequent replies confirmed successful connectivity.

![Successful connectivity verification.]({{ site.baseurl }}/assets/images/lab22/ping.png)

## Skills Demonstrated

- Troubleshooting IPv4 addressing
- Configuring default gateways on PCs and switches
- Configuring switch management interfaces
- Using `ipconfig`
- Using `ping` for local and remote connectivity testing
- Using `show ip interface brief`
- Following a structured troubleshooting methodology

## Final Result

All connectivity issues were successfully resolved, management connectivity to both switches was restored, and the Packet Tracer activity completed successfully.

![Packet Tracer completion screen.]({{ site.baseurl }}/assets/images/lab22/results.png)
