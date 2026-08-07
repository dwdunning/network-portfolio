---
layout: post
title: "Packet Tracer Lab 28 – Design and Implement a VLSM Addressing Scheme"
date: 2026-08-07
categories: [Networking, Cisco, Packet Tracer, Module-16-Routing]
tags: [networking, cisco, packet tracer, vlsm, subnetting, ipv4, cidr, routing]
---

## Objective

Design and implement a Variable Length Subnet Masking (VLSM) addressing scheme using the `10.1.1.0/24` network.

The activity required me to:

- Design a VLSM addressing scheme based on the required number of hosts on each LAN.
- Keep the assigned subnets contiguous.
- Use the most efficient subnet possible for the point-to-point WAN connection.
- Configure IPv4 addressing on both routers.
- Configure management addressing and default gateways on the switches.
- Configure static IPv4 addressing on the workstations.
- Verify connectivity between devices across all four LANs.

## Network Topology

The topology contains two routers, four switches, and four workstations. HQ and Remote are connected through a serial WAN link, with two LANs connected to each router.

![Packet Tracer Lab 28 topology]({{ site.baseurl }}/assets/images/lab28/topology.png)

The customer provided the network:

```text
10.1.1.0/24
```

The LAN host requirements were:

| LAN | Number of Addresses Required |
|---|---:|
| HQ-1 LAN | 11 |
| HQ-2 LAN | 28 |
| Remote-1 LAN | 5 |
| Remote-2 LAN | 47 |

A fifth subnet was also required for the point-to-point connection between HQ and Remote.

## VLSM Design

To create the VLSM addressing scheme efficiently, I assigned subnets from the largest host requirement to the smallest.

The order was:

1. Remote-2 LAN: 47 hosts
2. HQ-2 LAN: 28 hosts
3. HQ-1 LAN: 11 hosts
4. Remote-1 LAN: 5 hosts
5. HQ-to-Remote WAN: 2 hosts

### Remote-2 LAN

Remote-2 requires 47 host addresses.

A `/27` provides only 30 usable addresses, so it is too small. A `/26` provides 62 usable host addresses.

```text
Network:        10.1.1.0/26
Subnet Mask:    255.255.255.192
First Usable:   10.1.1.1
Last Usable:    10.1.1.62
Broadcast:      10.1.1.63
```

### HQ-2 LAN

HQ-2 requires 28 host addresses.

A `/27` provides 30 usable addresses.

```text
Network:        10.1.1.64/27
Subnet Mask:    255.255.255.224
First Usable:   10.1.1.65
Last Usable:    10.1.1.94
Broadcast:      10.1.1.95
```

### HQ-1 LAN

HQ-1 requires 11 host addresses.

A `/28` provides 14 usable addresses.

```text
Network:        10.1.1.96/28
Subnet Mask:    255.255.255.240
First Usable:   10.1.1.97
Last Usable:    10.1.1.110
Broadcast:      10.1.1.111
```

### Remote-1 LAN

Remote-1 requires 5 host addresses.

A `/29` provides 6 usable addresses.

```text
Network:        10.1.1.112/29
Subnet Mask:    255.255.255.248
First Usable:   10.1.1.113
Last Usable:    10.1.1.118
Broadcast:      10.1.1.119
```

### HQ-to-Remote WAN

The point-to-point link requires only two usable addresses.

A `/30` provides exactly two traditional usable host addresses.

```text
Network:        10.1.1.120/30
Subnet Mask:    255.255.255.252
First Usable:   10.1.1.121
Last Usable:    10.1.1.122
Broadcast:      10.1.1.123
```

## Completed VLSM Table

| Subnet Description | Number of Hosts Needed | Network Address/CIDR | First Usable Host Address | Broadcast Address |
|---|---:|---|---|---|
| Remote-2 LAN | 47 | `10.1.1.0/26` | `10.1.1.1` | `10.1.1.63` |
| HQ-2 LAN | 28 | `10.1.1.64/27` | `10.1.1.65` | `10.1.1.95` |
| HQ-1 LAN | 11 | `10.1.1.96/28` | `10.1.1.97` | `10.1.1.111` |
| Remote-1 LAN | 5 | `10.1.1.112/29` | `10.1.1.113` | `10.1.1.119` |
| HQ-to-Remote WAN | 2 | `10.1.1.120/30` | `10.1.1.121` | `10.1.1.123` |

The subnets are contiguous, with the next subnet beginning immediately after the broadcast address of the previous subnet.

## Address Assignment Requirements

The activity specified how addresses from each subnet should be assigned:

- HQ receives the first usable address on both HQ LANs.
- HQ receives the first usable address on the WAN.
- Remote receives the first usable address on both Remote LANs.
- Remote receives the last usable address on the WAN.
- Each switch receives the second usable address in its LAN.
- Each workstation receives the last usable address in its LAN.
- Each switch and workstation uses the router interface on its LAN as the default gateway.

## Completed Addressing Table

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| HQ | G0/0 | `10.1.1.97` | `255.255.255.240` | N/A |
| HQ | G0/1 | `10.1.1.65` | `255.255.255.224` | N/A |
| HQ | S0/0/0 | `10.1.1.121` | `255.255.255.252` | N/A |
| Remote | G0/0 | `10.1.1.113` | `255.255.255.248` | N/A |
| Remote | G0/1 | `10.1.1.1` | `255.255.255.192` | N/A |
| Remote | S0/0/0 | `10.1.1.122` | `255.255.255.252` | N/A |
| HQ-1 | VLAN 1 | `10.1.1.98` | `255.255.255.240` | `10.1.1.97` |
| HQ-2 | VLAN 1 | `10.1.1.66` | `255.255.255.224` | `10.1.1.65` |
| Remote-1 | VLAN 1 | `10.1.1.114` | `255.255.255.248` | `10.1.1.113` |
| Remote-2 | VLAN 1 | `10.1.1.2` | `255.255.255.192` | `10.1.1.1` |
| WS116 | NIC | `10.1.1.110` | `255.255.255.240` | `10.1.1.97` |
| WS145 | NIC | `10.1.1.94` | `255.255.255.224` | `10.1.1.65` |
| WS203 | NIC | `10.1.1.118` | `255.255.255.248` | `10.1.1.113` |
| WS234 | NIC | `10.1.1.62` | `255.255.255.192` | `10.1.1.1` |

## Router Configuration

### HQ

I configured the HQ router with the first usable address from each of its three subnets.

For G0/0:

```text
enable
configure terminal
interface g0/0
ip address 10.1.1.97 255.255.255.240
no shutdown
```

For G0/1:

```text
interface g0/1
ip address 10.1.1.65 255.255.255.224
no shutdown
```

For the serial WAN connection:

```text
interface s0/0/0
ip address 10.1.1.121 255.255.255.252
no shutdown
```

### Remote

Remote received the first usable address on each LAN and the last usable address on the WAN.

For G0/0:

```text
enable
configure terminal
interface g0/0
ip address 10.1.1.113 255.255.255.248
no shutdown
```

For G0/1:

```text
interface g0/1
ip address 10.1.1.1 255.255.255.192
no shutdown
```

For the serial WAN connection:

```text
interface s0/0/0
ip address 10.1.1.122 255.255.255.252
no shutdown
```

Once both sides of the serial link were configured, the interface came up and the routers formed an EIGRP neighbor adjacency across the WAN.

## Switch Management Configuration

Each switch received the second usable address from its LAN. I also configured a default gateway so that the management interface could communicate with devices outside its local subnet.

### HQ-1

```text
enable
configure terminal
interface vlan 1
ip address 10.1.1.98 255.255.255.240
no shutdown
exit
ip default-gateway 10.1.1.97
```

### HQ-2

```text
enable
configure terminal
interface vlan 1
ip address 10.1.1.66 255.255.255.224
no shutdown
exit
ip default-gateway 10.1.1.65
```

### Remote-1

```text
enable
configure terminal
interface vlan 1
ip address 10.1.1.114 255.255.255.248
no shutdown
exit
ip default-gateway 10.1.1.113
```

### Remote-2

```text
enable
configure terminal
interface vlan 1
ip address 10.1.1.2 255.255.255.192
no shutdown
exit
ip default-gateway 10.1.1.1
```

## Workstation Configuration

The instructions required each workstation to use the last usable address in its LAN.

### WS116

```text
IP Address:       10.1.1.110
Subnet Mask:      255.255.255.240
Default Gateway:  10.1.1.97
```

### WS145

```text
IP Address:       10.1.1.94
Subnet Mask:      255.255.255.224
Default Gateway:  10.1.1.65
```

### WS203

```text
IP Address:       10.1.1.118
Subnet Mask:      255.255.255.248
Default Gateway:  10.1.1.113
```

### WS234

```text
IP Address:       10.1.1.62
Subnet Mask:      255.255.255.192
Default Gateway:  10.1.1.1
```

## Connectivity Verification

After configuring all of the devices, I tested connectivity from WS234.

First, I tested the local default gateway:

```text
C:\>ping 10.1.1.1

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This confirmed that WS234 could communicate with the Remote router on its local LAN.

I then tested connectivity from WS234 to WS116 on the opposite side of the network:

```text
C:\>ping 10.1.1.110
```

The first test had one initial timeout while address resolution occurred, followed by three successful replies. Repeating the test produced:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This verified the complete path:

```text
WS234
  |
Remote-2 LAN
  |
Remote
  |
Serial WAN
  |
HQ
  |
HQ-1 LAN
  |
WS116
```

I also tested the HQ-1 switch management interface from WS234:

```text
C:\>ping 10.1.1.98

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This confirmed that the switch management interface and its configured default gateway were working across the routed network.

## Results

Packet Tracer confirmed that the VLSM design and all device configurations were correct.

![Packet Tracer Lab 28 results]({{ site.baseurl }}/assets/images/lab28/results.png)

The final assessment showed:

| Assessment | Score |
|---|---:|
| Default Gateway | 6/6 |
| Default Gateway Configuration | 6/6 |
| Device Interface Configuration | 9/9 |
| PC Address | 6/6 |
| VLSM Addressing Implementation | 20/20 |
| **Item Count** | **43/43** |
| **Final Score** | **111/111** |

The activity was completed successfully with a score of **111/111**.

## What I Learned

This lab tied together subnetting, VLSM, device configuration, routing, and connectivity testing in a single topology.

The most important part of the exercise was designing the address space before configuring any devices. Starting with the largest host requirement made it possible to allocate each subnet efficiently while keeping all of the networks contiguous.

The final VLSM allocation was:

```text
10.1.1.0/26       Remote-2
10.1.1.64/27      HQ-2
10.1.1.96/28      HQ-1
10.1.1.112/29     Remote-1
10.1.1.120/30     HQ-to-Remote WAN
```

I also reinforced the relationship between the network address, usable host range, broadcast address, subnet mask, and default gateway. Once the VLSM plan was correct, the device configuration became much more straightforward.

Finally, testing connectivity from one of the Remote LANs to hosts and switch management interfaces at HQ demonstrated that successful subnetting is not only about calculating address ranges. The addressing plan, router interfaces, switch management configuration, default gateways, and routing all have to work together for end-to-end communication to succeed.
