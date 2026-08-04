---
title: "Packet Tracer Lab 13 - Verify IPv4 and IPv6 Addressing"
date: 2026-08-04
categories:
  - Networking
  - Cisco
  - Packet Tracer
  - Module 13
tags:
  - IPv4
  - IPv6
  - Dual Stack
  - Ping
  - Tracert
  - Packet Tracer
---

## Overview

This lab explored a dual-stack network using both IPv4 and IPv6. The objectives were to verify host addressing, test end-to-end connectivity using both protocols, and analyze the path packets take across multiple routers using `tracert`.

## Network Topology

![Dual-stack network topology.]({{ site.baseurl }}/assets/images/lab13/topology.png)

The topology consists of three interconnected routers separating two LANs. Each LAN supports both IPv4 and IPv6 addressing.

---

## Part 1 – Verify Host Addressing

### PC1 IPv4 Configuration

![PC1 ipconfig /all output.]({{ site.baseurl }}/assets/images/lab13/pc1-ipconfig.png)

### PC1 IPv6 Configuration

![PC1 ipv6config /all output.]({{ site.baseurl }}/assets/images/lab13/pc1-ipv6config.png)

### PC2 IPv4 Configuration

![PC2 ipconfig /all output.]({{ site.baseurl }}/assets/images/lab13/pc2-ipconfig.png)

### PC2 IPv6 Configuration

![PC2 ipv6config /all output.]({{ site.baseurl }}/assets/images/lab13/pc2-ipv6config.png)

### Completed Addressing Table

| Device | IPv4 Address | Subnet Mask | IPv4 Gateway | IPv6 Address | Prefix | IPv6 Gateway |
|---------|--------------|-------------|---------------|--------------|--------|--------------|
| PC1 | 10.10.1.100 | 255.255.255.224 | 10.10.1.97 | 2001:DB8:1:1::A | /64 | FE80::1 |
| PC2 | 10.10.1.20 | 255.255.255.240 | 10.10.1.17 | 2001:DB8:1:4::A | /64 | FE80::3 |

---

## Part 2 – Test Connectivity Using Ping

### PC1 to PC2

![Ping results from PC1 to PC2 using IPv4 and IPv6.]({{ site.baseurl }}/assets/images/lab13/ping-pc2-from-pc1.png)

### IPv4 Results

The first IPv4 ping request timed out while ARP resolved the destination MAC address. The remaining three packets were successful.

**Question:** Was the result successful?

**Answer:** Yes.

### IPv6 Results

All four IPv6 echo requests completed successfully without packet loss.

**Question:** Was the result successful?

**Answer:** Yes.

### PC2 to PC1

Both IPv4 and IPv6 connectivity from PC2 back to PC1 were also successful.

**Question:** Was the result successful?

**Answer:** Yes.

---

## Part 3 – Trace the Route

### IPv4 Traceroute (PC1 → PC2)

![IPv4 and IPv6 tracert results from PC1.]({{ site.baseurl }}/assets/images/lab13/tracert-pc2-from-pc1.png)

#### Addresses Encountered

| Hop | Address | Interface |
|-----:|---------|-----------|
| 1 | 10.10.1.97 | R1 G0/0 |
| 2 | 10.10.1.5 | R2 S0/0/0 |
| 3 | 10.10.1.10 | R3 S0/0/1 |
| 4 | 10.10.1.20 | PC2 NIC |

### IPv6 Traceroute (PC1 → PC2)

| Hop | Address | Interface |
|-----:|---------|-----------|
| 1 | 2001:DB8:1:1::1 | R1 G0/0 |
| 2 | 2001:DB8:1:2::1 | R2 S0/0/0 |
| 3 | 2001:DB8:1:3::2 | R3 S0/0/1 |
| 4 | 2001:DB8:1:4::A | PC2 NIC |

---

### IPv4 Traceroute (PC2 → PC1)

![IPv4 and IPv6 tracert results from PC2.]({{ site.baseurl }}/assets/images/lab13/tracert-from-pc2.png)

#### Addresses Encountered

| Hop | Address | Interface |
|-----:|---------|-----------|
| 1 | 10.10.1.17 | R3 G0/0 |
| 2 | 10.10.1.9 | R2 S0/0/1 |
| 3 | 10.10.1.6 | R1 S0/0/1 |
| 4 | Destination timed out (10.10.1.100) | PC1 NIC |

Although the final destination did not respond to the traceroute probe, all intermediate routers replied correctly and normal IPv4 ping testing confirmed end-to-end connectivity. This behavior is common in Packet Tracer simulations.

### IPv6 Traceroute (PC2 → PC1)

| Hop | Address | Interface |
|-----:|---------|-----------|
| 1 | 2001:DB8:1:4::1 | R3 G0/0 |
| 2 | 2001:DB8:1:3::1 | R2 S0/0/1 |
| 3 | 2001:DB8:1:2::2 | R1 S0/0/1 |
| 4 | 2001:DB8:1:1::A | PC1 NIC |

---

## Observations

- Verified IPv4 and IPv6 addressing for both hosts.
- Confirmed successful dual-stack connectivity using `ping`.
- Observed the initial IPv4 ping timeout caused by ARP address resolution.
- Used `tracert` to identify each router along the packet path.
- Compared IPv4 and IPv6 routing through the same network topology.
- Observed a Packet Tracer-specific timeout on the final IPv4 traceroute hop while connectivity remained functional.

---

## Conclusion

This lab demonstrated how IPv4 and IPv6 operate simultaneously within a dual-stack network. Using `ipconfig`, `ipv6config`, `ping`, and `tracert`, I verified addressing information, confirmed end-to-end communication, and identified the routing path between hosts. The activity reinforced how subnetting, routing, and gateway configuration work together to provide reliable communication across multiple networks using both Internet Protocol versions.
