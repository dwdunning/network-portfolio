---
layout: post
title: "Packet Tracer Lab 36 - Use ICMP to Test and Correct Network Connectivity"
date: 2026-08-08
categories: [Networking, Cisco, Packet Tracer, Module-17-Basic-Network-Administration]
tags: [networking, cisco, packet tracer, icmp, ping, tracert, troubleshooting, ipv4, ipv6, eigrp, network administration, Network+]
---

## Overview

In Module 17 Lab 36, I used ICMP troubleshooting tools to identify and correct several IPv4 and IPv6 connectivity problems in a Packet Tracer network. The network contained three routers, multiple IPv4 and IPv6 LANs, and a Corporate Server that hosts on both sides of the network needed to reach.

The primary troubleshooting tools were `ping` and `tracert`. Rather than immediately changing device configurations, I tested connectivity from known-good locations, identified which destinations were unreachable, traced the path toward those destinations, and then inspected the devices near the point of failure.

![Packet Tracer Lab 36 network topology]({{ '/assets/images/lab36/topology.png' | relative_url }})

## Objectives

The objectives of this lab were to:

- Use ICMP to locate network connectivity issues.
- Use `ping` to determine which hosts were reachable.
- Use `tracert` to locate the general point of a network failure.
- Inspect host and router configurations to identify specific errors.
- Correct IPv4 and IPv6 configuration problems.
- Verify that connectivity was restored after each correction.

## Addressing Scheme

The network used both IPv4 and IPv6 addressing.

| Device | Interface | Address | Mask/Prefix | Default Gateway |
|---|---|---|---|---|
| RTR-1 | G0/0/0 | 192.168.1.1 | 255.255.255.0 | N/A |
| RTR-1 | G0/0/0 | 2001:db8:4::1 | /64 | N/A |
| RTR-1 | S0/1/0 | 10.10.2.2 | 255.255.255.252 | N/A |
| RTR-1 | S0/1/0 | 2001:db8:2::2 | /126 | N/A |
| RTR-1 | S0/1/1 | 10.10.3.1 | 255.255.255.252 | N/A |
| RTR-1 | S0/1/1 | 2001:db8:3::1 | /126 | N/A |
| RTR-2 | G0/0/0 | 10.10.1.1 | 255.255.255.0 | N/A |
| RTR-2 | G0/0/1 | 2001:db8:1::1 | /64 | N/A |
| RTR-2 | S0/1/0 | 10.10.2.1 | 255.255.255.252 | N/A |
| RTR-2 | S0/1/0 | 2001:db8:2::1 | /126 | N/A |
| RTR-3 | G0/0/0 | 10.10.5.1 | 255.255.255.0 | N/A |
| RTR-3 | G0/0/1 | 2001:db8:5::1 | /64 | N/A |
| RTR-3 | S0/1/0 | 10.10.3.2 | 255.255.255.252 | N/A |
| RTR-3 | S0/1/0 | 2001:db8:3::2 | /126 | N/A |
| PC-1 | NIC | 10.10.1.10 | 255.255.255.0 | 10.10.1.1 |
| Laptop A | NIC | 10.10.1.20 | 255.255.255.0 | 10.10.1.1 |
| PC-2 | NIC | 2001:db8:1::10 | /64 | fe80::1 |
| PC-3 | NIC | 2001:db8:1::20 | /64 | fe80::1 |
| PC-4 | NIC | 10.10.5.10 | 255.255.255.0 | 10.10.5.1 |
| Server1 | NIC | 10.10.5.20 | 255.255.255.0 | 10.10.5.1 |
| Laptop B | NIC | 2001:db8:5::10 | /64 | fe80::1 |
| Laptop C | NIC | 2001:db8:5::20 | /64 | fe80::1 |
| Corporate Server | NIC | 203.0.113.100 | 255.255.255.0 | 203.0.113.1 |
| Corporate Server | NIC | 2001:db8:acad::100 | /64 | fe80::1 |

## IPv4 Connectivity Testing

I began troubleshooting from PC-1 on the `10.10.1.0/24` network.

The first test was the local default gateway:

```text
ping 10.10.1.1
```

This succeeded, confirming that PC-1 could communicate with RTR-2.

I then tested several destinations:

```text
ping 10.10.1.20
ping 10.10.5.10
ping 10.10.5.20
ping 203.0.113.100
```

PC-1 could successfully reach Laptop A and the Corporate Server, but it could not reach PC-4 or Server1 on the `10.10.5.0/24` network.

This was an important clue because successful communication with the Corporate Server showed that PC-1, RTR-2, and the route through RTR-1 were operational.

## Tracing the IPv4 Failure

I used `tracert` to determine how far packets could travel toward PC-4:

```text
tracert 10.10.5.10
```

The trace reached:

```text
10.10.1.1
10.10.2.2
10.10.3.2
```

The third address is RTR-3. This showed that packets were successfully crossing RTR-2, RTR-1, and RTR-3.

The problem therefore existed at or beyond RTR-3 on the `10.10.5.0/24` LAN.

## Fault 1 - Incorrect PC-4 Default Gateway

PC-4 could successfully ping RTR-3 at:

```text
10.10.5.1
```

However, `ipconfig` revealed that PC-4 was configured with the following default gateway:

```text
10.10.5.11
```

The correct gateway from the addressing table was:

```text
10.10.5.1
```

I changed PC-4's default gateway from `10.10.5.11` to `10.10.5.1`.

I then returned to PC-1 and tested connectivity again:

```text
ping 10.10.5.10
```

The test returned four successful replies with 0% packet loss.

This demonstrated an important troubleshooting concept. PC-4 could reach its local router even with the incorrect default gateway because `10.10.5.1` was on the same subnet. The incorrect gateway became a problem when PC-4 needed to return traffic to a remote network.

## Fault 2 - Incorrect Server1 IPv4 Configuration

Server1 was also unreachable from PC-1.

Running:

```text
ipconfig
```

on Server1 revealed:

```text
Autoconfiguration IPv4 Address: 169.254.20.91
Subnet Mask:                    255.255.0.0
Default Gateway:                0.0.0.0
```

The `169.254.0.0/16` address indicated that Server1 had assigned itself an APIPA address rather than using the required IPv4 configuration.

According to the addressing table, Server1 required:

```text
IPv4 Address:    10.10.5.20
Subnet Mask:     255.255.255.0
Default Gateway: 10.10.5.1
```

I corrected the IPv4 configuration and then tested from PC-1:

```text
ping 10.10.5.20
```

The first ICMP request timed out while the devices resolved the required Layer 2 information, but the remaining replies succeeded. Connectivity to Server1 had been restored.

## IPv6 Connectivity Testing

After correcting the IPv4 problems, I moved to PC-2 to test IPv6 connectivity.

PC-2 was correctly configured with:

```text
IPv6 Address:    2001:db8:1::10
Default Gateway: fe80::1
```

I first tested its local router:

```text
ping 2001:db8:1::1
```

The ping succeeded.

I then tested the remote IPv6 hosts:

```text
ping 2001:db8:5::10
ping 2001:db8:5::20
```

Both failed.

However, PC-2 could successfully reach the Corporate Server:

```text
ping 2001:db8:acad::100
```

This showed that IPv6 connectivity and routing were functioning in general, but the `2001:db8:5::/64` network behind RTR-3 was specifically unreachable.

## Tracing the IPv6 Failure

I traced the route toward Laptop B:

```text
tracert 2001:db8:5::10
```

The trace failed to progress normally toward the destination.

I then inspected the IPv6 routing table on RTR-2:

```text
show ipv6 route
```

RTR-2 did not have a route for the expected network:

```text
2001:db8:5::/64
```

Instead, it had learned:

```text
2001:db8:6::/64
```

This indicated that a router was advertising the wrong IPv6 network.

## Fault 3 - Incorrect IPv6 Address on RTR-3

I inspected RTR-3 with:

```text
show ipv6 interface brief
```

The output showed that GigabitEthernet0/0/1 was configured as:

```text
2001:DB8:6::1
```

According to the addressing table, it should have been:

```text
2001:DB8:5::1/64
```

I corrected the interface with:

```text
configure terminal
interface gigabitEthernet 0/0/1
no ipv6 address 2001:DB8:6::1/64
ipv6 address 2001:DB8:5::1/64
end
```

The corrected network was then propagated through the routing environment.

## Final IPv6 Verification

I returned to PC-2 and repeated the failed tests:

```text
ping 2001:db8:5::10
ping 2001:db8:5::20
```

Both destinations now returned four replies with 0% packet loss.

![Successful IPv6 ping tests after correcting the RTR-3 configuration]({{ '/assets/images/lab36/ping.png' | relative_url }})

This confirmed that the IPv6 routing problem had been resolved.

## Troubleshooting Summary

Three separate configuration problems were identified and corrected during the lab:

| Device | Problem | Correction |
|---|---|---|
| PC-4 | Default gateway configured as `10.10.5.11` | Changed gateway to `10.10.5.1` |
| Server1 | APIPA address `169.254.20.91/16` instead of required IPv4 configuration | Configured `10.10.5.20/24` with gateway `10.10.5.1` |
| RTR-3 | G0/0/1 configured as `2001:db8:6::1/64` | Changed address to `2001:db8:5::1/64` |

## Troubleshooting Process

The lab reinforced a systematic process for troubleshooting network connectivity:

1. Verify the local host configuration.
2. Ping the local default gateway.
3. Ping known local and remote destinations.
4. Compare successful and unsuccessful tests to narrow the affected area.
5. Use `tracert` to determine how far packets travel.
6. Inspect the configuration of devices near the failure point.
7. Compare actual configuration values against the addressing plan.
8. Correct only the identified configuration error.
9. Repeat the original connectivity test to verify the repair.

This approach avoided making unnecessary changes to devices that were already operating correctly.

## Results

Packet Tracer reported:

```text
Score:      7/7
Item Count: 7/7
```

All assessed configuration items were marked **Correct**, confirming successful completion of the activity.

![Packet Tracer Lab 36 results showing 7 out of 7 assessment items correct]({{ '/assets/images/lab36/results.png' | relative_url }})

## Reflection

This lab demonstrated why `ping` and `tracert` are most useful when they are used together with an addressing plan. A failed ping established that a connectivity problem existed, while successful pings to other destinations helped eliminate working portions of the network from consideration. `tracert` then narrowed the problem to a particular part of the network.

The PC-4 problem also demonstrated the difference between local and remote communication. PC-4 could reach RTR-3 because both addresses were on the same subnet, even though PC-4's default gateway was incorrect. The problem appeared when communication required routing beyond the local subnet.

Server1 provided an example of recognizing an APIPA address as an immediate indication of an IPv4 configuration problem.

The IPv6 problem demonstrated how an incorrect router interface address can affect routing beyond the router itself. RTR-3's incorrect `2001:db8:6::1/64` address caused the wrong network to appear in the IPv6 routing table. Correcting the interface to `2001:db8:5::1/64` restored the expected route and connectivity to both IPv6 hosts.

The completed activity received a score of **7/7**, confirming that all identified IPv4 and IPv6 configuration problems were successfully corrected.
