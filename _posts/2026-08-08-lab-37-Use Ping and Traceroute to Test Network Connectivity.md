---
layout: post
title: "Packet Tracer Lab 37 - Use Ping and Traceroute to Test Network Connectivity"
date: 2026-08-08
categories: [Networking, Cisco, Packet Tracer, Module-17-Basic-Network-Administration]
tags: [networking, cisco, packet tracer, ping, traceroute, tracert, icmp, ipv4, ipv6, eigrp, troubleshooting, network-administration, Network+]
---

## Overview

In this lab, I used `ping`, `tracert`, and Cisco IOS diagnostic commands to troubleshoot IPv4 and IPv6 connectivity failures across a routed network.

The network contained two separate problems. The IPv4 failure was caused by an incorrect IP address on an R2 serial interface, while the IPv6 failure was caused by an incorrect default gateway on PC4.

Rather than changing configurations immediately, I used a systematic troubleshooting process:

1. Document the host addressing.
2. Use `ping` to verify the connectivity failure.
3. Use `tracert` to determine how far packets could travel.
4. Examine router interfaces and routing tables.
5. Compare the observed configuration against the documented addressing plan.
6. Correct the identified configuration error.
7. Repeat the connectivity tests to verify the repair.

![Lab 37 network topology]({{ '/assets/images/lab37/topology.png' | relative_url }})

## Objectives

The objectives of this activity were:

- Test and restore IPv4 connectivity.
- Test and restore IPv6 connectivity.
- Use `ping` to determine whether remote hosts are reachable.
- Use `tracert` to locate where communication fails along a routed path.
- Examine router interfaces and routing tables.
- Identify incorrect IPv4 and IPv6 configurations.
- Implement corrections and verify restored end-to-end connectivity.

## Network Addressing

The routed network used separate IPv4 and IPv6 networks.

| Device | Interface | Address / Prefix |
|---|---|---|
| R1 | G0/0 | `2001:DB8:1:1::1/64` |
| R1 | G0/1 | `10.10.1.97/27` |
| R1 | S0/0/1 | `10.10.1.6/30` |
| R1 | S0/0/1 | `2001:DB8:1:2::2/64` |
| R1 | S0/0/1 | `FE80::1` |
| R2 | S0/0/0 | `10.10.1.5/30` |
| R2 | S0/0/0 | `2001:DB8:1:2::1/64` |
| R2 | S0/0/1 | `10.10.1.9/30` |
| R2 | S0/0/1 | `2001:DB8:1:3::1/64` |
| R2 | S0/0/1 | `FE80::2` |
| R3 | G0/0 | `2001:DB8:1:4::1/64` |
| R3 | G0/1 | `10.10.1.17/28` |
| R3 | S0/0/1 | `10.10.1.10/30` |
| R3 | S0/0/1 | `2001:DB8:1:3::2/64` |
| R3 | S0/0/1 | `FE80::3` |

The host addressing was collected during troubleshooting rather than assumed from the documentation.

## Part 1 - Test and Restore IPv4 Connectivity

### Documenting PC1

I began on PC1 with:

```text
ipconfig /all
```

PC1 reported:

| Setting | Value |
|---|---|
| IPv4 address | `10.10.1.98` |
| Subnet mask | `255.255.255.224` |
| Default gateway | `10.10.1.97` |

This placed PC1 on the `10.10.1.96/27` network with R1 G0/1 as its default gateway.

### Documenting PC3

I repeated the process on PC3:

```text
ipconfig /all
```

PC3 reported:

| Setting | Value |
|---|---|
| IPv4 address | `10.10.1.18` |
| Subnet mask | `255.255.255.240` |
| Default gateway | `10.10.1.17` |

PC3 was therefore on the `10.10.1.16/28` network with R3 G0/1 as its default gateway.

Both PCs appeared to have correct local IPv4 configurations.

## Testing IPv4 Connectivity

From PC1, I attempted to ping PC3:

```text
ping 10.10.1.18
```

The test failed with messages from `10.10.1.97` stating:

```text
Destination host unreachable.
```

This was significant because `10.10.1.97` was PC1's default gateway. PC1 could communicate with R1, but R1 was unable to successfully forward the traffic toward PC3.

I then traced the route:

```text
tracert 10.10.1.18
```

The last successful IPv4 address reached from PC1 was:

```text
10.10.1.97
```

![PC1 IPv4 ping and traceroute failure]({{ '/assets/images/lab37/pc1.png' | relative_url }})

### Tracing from PC3

I performed the reverse test from PC3:

```text
tracert 10.10.1.98
```

The last successful address reached was:

```text
10.10.1.17
```

This was R3's G0/1 interface and PC3's default gateway.

![PC3 IPv4 traceroute failure]({{ '/assets/images/lab37/pc3.png' | relative_url }})

The two traces produced a useful pattern:

| Trace | Last Successful Address |
|---|---|
| PC1 to PC3 | `10.10.1.97` |
| PC3 to PC1 | `10.10.1.17` |

Both hosts could reach their local routers, but traffic was not successfully crossing the routed WAN.

## Examining R1

I logged into R1 and entered privileged EXEC mode.

I first checked the interfaces:

```text
show ip interface brief
```

The important IPv4 interfaces were:

```text
GigabitEthernet0/1    10.10.1.97    up    up
Serial0/0/1           10.10.1.6     up    up
```

The other IPv4 address requested by the lab was therefore:

```text
10.10.1.6
```

Both interfaces were `up/up`, which helped rule out a shutdown interface or physical link problem.

I then examined the routing table:

```text
show ip route
```

R1 showed:

```text
C    10.10.1.4/30 is directly connected, Serial0/0/1
L    10.10.1.6/32 is directly connected, Serial0/0/1
C    10.10.1.96/27 is directly connected, GigabitEthernet0/1
L    10.10.1.97/32 is directly connected, GigabitEthernet0/1
```

The two entries associated with Serial0/0/1 were:

- `10.10.1.4/30`
- `10.10.1.6/32`

![R1 IPv4 interface and routing information]({{ '/assets/images/lab37/R1.png' | relative_url }})

R1's interface addressing matched the documented network.

## Examining R3

I performed the same checks on R3:

```text
show ip interface brief
show ip route
```

R3 reported:

```text
GigabitEthernet0/1    10.10.1.17    up    up
Serial0/0/1           10.10.1.10    up    up
```

Its routing table contained:

```text
C    10.10.1.8/30 is directly connected, Serial0/0/1
L    10.10.1.10/32 is directly connected, Serial0/0/1
C    10.10.1.16/28 is directly connected, GigabitEthernet0/1
L    10.10.1.17/32 is directly connected, GigabitEthernet0/1
```

R3 also reported an EIGRP adjacency with R2 at `10.10.1.9`.

![R3 IPv4 interface and routing information]({{ '/assets/images/lab37/R3.png' | relative_url }})

R3's IPv4 configuration also matched the addressing documentation.

## Locating the IPv4 Error on R2

I next examined R2:

```text
show ip interface brief
show ip route
```

This revealed the problem.

R2 showed:

```text
Serial0/0/0    10.10.1.2    up    up
Serial0/0/1    10.10.1.9    up    up
```

The addressing table specified that R2 Serial0/0/0 should have been:

```text
10.10.1.5/30
```

Instead, it had:

```text
10.10.1.2/30
```

The incorrect address also caused R2's routing table to contain:

```text
C    10.10.1.0/30 is directly connected, Serial0/0/0
L    10.10.1.2/32 is directly connected, Serial0/0/0
```

![R2 showing the incorrect IPv4 serial interface configuration]({{ '/assets/images/lab37/R2.png' | relative_url }})

This explained the connectivity failure.

R1's serial interface was `10.10.1.6/30`, placing it on:

```text
10.10.1.4/30
```

R2 incorrectly believed its corresponding interface belonged to:

```text
10.10.1.0/30
```

The two ends of the serial link therefore had incompatible IPv4 subnet configurations.

## Correcting the IPv4 Problem

The error was:

> R2 Serial0/0/0 was configured as `10.10.1.2/30` instead of the documented `10.10.1.5/30`.

I corrected the interface:

```text
configure terminal
interface serial0/0/0
ip address 10.10.1.5 255.255.255.252
end
```

Immediately after the correction, R2 reported a new EIGRP adjacency with R1:

```text
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 10.10.1.6 (Serial0/0/0) is up: new adjacency
```

The routing table then correctly contained the connected WAN network:

```text
C    10.10.1.4/30 is directly connected, Serial0/0/0
L    10.10.1.5/32 is directly connected, Serial0/0/0
```

R2 also learned both remote LANs through EIGRP:

```text
D    10.10.1.16/28 via 10.10.1.10
D    10.10.1.96/27 via 10.10.1.6
```

This confirmed that correcting the interface address restored the missing routing relationship.

## Verifying IPv4 Connectivity

I repeated the original tests.

From PC1:

```text
ping 10.10.1.18
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

From PC3:

```text
ping 10.10.1.98
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

IPv4 connectivity was fully restored in both directions.

### IPv4 Resolution

**Problem:** R2 S0/0/0 had the incorrect address `10.10.1.2/30`.

**Correction:** Changed R2 S0/0/0 to `10.10.1.5/30`.

**Result:** R1 and R2 established their EIGRP adjacency, remote IPv4 routes were learned, and PC1 and PC3 could communicate successfully.

## Part 2 - Test and Restore IPv6 Connectivity

The second part of the lab used the same troubleshooting methodology for IPv6.

## Documenting PC2

On PC2, I entered:

```text
ipv6config /all
```

PC2 reported:

| Setting | Value |
|---|---|
| IPv6 address | `2001:DB8:1:1::2` |
| Prefix | `/64` |
| Default gateway | `FE80::1` |

PC2's configuration matched the `2001:DB8:1:1::/64` LAN.

## Documenting PC4

On PC4, I entered:

```text
ipv6config /all
```

PC4 initially reported:

| Setting | Value |
|---|---|
| IPv6 address | `2001:DB8:1:4::2` |
| Prefix | `/64` |
| Default gateway | `FE80::2` |

The global unicast address was on the expected `2001:DB8:1:4::/64` network.

The default gateway would later prove to be the source of the IPv6 failure.

## Testing IPv6 Connectivity

From PC2, I attempted to reach PC4:

```text
ping 2001:DB8:1:4::2
```

The ping failed with 100% packet loss.

I then traced the route:

```text
tracert 2001:DB8:1:4::2
```

The successful portion of the path was:

```text
1    2001:DB8:1:1::1
2    2001:DB8:1:2::1
3    2001:DB8:1:3::2
4    Request timed out.
```

The last successful IPv6 address reached was:

```text
2001:DB8:1:3::2
```

This address belonged to R3.

The trace demonstrated that IPv6 packets could successfully travel:

```text
PC2 -> R1 -> R2 -> R3
```

The failure therefore occurred after the traffic reached R3.

## Tracing from PC4

From PC4, I attempted the reverse trace:

```text
tracert 2001:DB8:1:1::2
```

PC4 could not successfully reach even the first hop.

This was a major clue because it indicated that PC4 could not communicate with its own IPv6 default gateway.

The two traces together isolated the problem to the R3-PC4 LAN:

- PC2 could successfully reach R3.
- PC4 could not successfully leave its local network.

## Examining R3's IPv6 Configuration

On R3, I entered:

```text
show ipv6 interface brief
```

R3's G0/0 interface showed:

```text
GigabitEthernet0/0    [up/up]
    FE80::3
    2001:DB8:1:4::1
```

PC4, however, was configured with:

```text
Default Gateway: FE80::2
```

There was therefore a clear discrepancy.

R3's actual LAN-side link-local address was:

```text
FE80::3
```

while PC4 was attempting to use:

```text
FE80::2
```

as its gateway.

## Correcting the IPv6 Problem

The IPv6 error was:

> PC4 had the incorrect IPv6 default gateway `FE80::2`.

I changed PC4's IPv6 default gateway to:

```text
FE80::3
```

The corrected PC4 configuration was therefore:

| Setting | Correct Value |
|---|---|
| IPv6 address | `2001:DB8:1:4::2` |
| Prefix | `/64` |
| Default gateway | `FE80::3` |

No change to PC4's global unicast IPv6 address was necessary.

## Verifying IPv6 Connectivity

After correcting the gateway, I repeated the connectivity tests.

From PC2:

```text
ping 2001:DB8:1:4::2
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

From PC4:

```text
ping 2001:DB8:1:1::2
```

Result:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

IPv6 connectivity was successfully restored in both directions.

### IPv6 Resolution

**Problem:** PC4 used `FE80::2` as its IPv6 default gateway even though R3 G0/0 used `FE80::3`.

**Correction:** Changed PC4's IPv6 default gateway to `FE80::3`.

**Result:** PC2 and PC4 successfully communicated across the routed IPv6 network with 0% packet loss.

## Troubleshooting Summary

| Protocol | Symptom | Diagnostic Evidence | Root Cause | Correction |
|---|---|---|---|---|
| IPv4 | PC1 and PC3 could not communicate | Traces stopped at their local gateways | R2 S0/0/0 used `10.10.1.2/30` instead of `10.10.1.5/30` | Corrected R2 S0/0/0 to `10.10.1.5/30` |
| IPv6 | PC2 and PC4 could not communicate | PC2 reached R3, while PC4 could not reach its first hop | PC4 used `FE80::2` instead of R3's `FE80::3` | Changed PC4's default gateway to `FE80::3` |

## Commands Used

The primary troubleshooting commands used during this lab were:

```text
ipconfig /all
ipv6config /all
ping
tracert
show ip interface brief
show ip route
show ipv6 interface brief
```

The IPv4 repair on R2 used:

```text
configure terminal
interface serial0/0/0
ip address 10.10.1.5 255.255.255.252
end
```

## What I Learned

This lab demonstrated why `ping` and `tracert` serve different but complementary purposes during network troubleshooting.

`ping` answered the basic question of whether two hosts could communicate. When the ping failed, however, it did not by itself identify the exact source of the problem.

`tracert` provided the next level of information by showing how far packets traveled before communication stopped.

The IPv4 problem showed how an incorrect address on one router interface can prevent routing protocols from establishing the expected neighbor relationship. R1 and R2 were physically connected and their interfaces were operational, but their IPv4 addresses placed them in different subnets. Correcting R2's address restored the EIGRP adjacency and allowed the routers to exchange routes.

The IPv6 problem demonstrated a different type of failure. The routed IPv6 infrastructure from PC2 through R1 and R2 to R3 was already functioning. Traceroute proved this by successfully reaching R3. PC4 could not even reach its first hop, which shifted the investigation toward its local configuration. Comparing PC4's gateway with R3's link-local address exposed the incorrect `FE80::2` gateway.

The most important lesson from this lab was to use the results of each test to narrow the troubleshooting scope. Instead of changing multiple configurations at once, I identified the last known working point, inspected the devices immediately around the failure, compared their configurations with the network documentation, made one targeted correction, and then verified the result.

## Final Result

Both network problems were successfully diagnosed and corrected.

- IPv4 connectivity between PC1 and PC3 was restored.
- R1 and R2 established the required IPv4 EIGRP adjacency.
- R2 learned the remote IPv4 LAN routes.
- IPv6 connectivity between PC2 and PC4 was restored.
- Final IPv4 tests completed with 0% packet loss.
- Final IPv6 tests completed with 0% packet loss.

Lab 37 demonstrated a structured approach to network troubleshooting using ICMP connectivity testing, hop-by-hop path analysis, interface verification, routing-table inspection, and comparison against documented network addressing.
