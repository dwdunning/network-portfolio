---
layout: post
title: "Packet Tracer Lab 31 – Implement a Subnetted IPv6 Addressing Scheme"
date: 2026-08-07
categories: [Networking, Cisco, Packet Tracer, Module-16-Routing]
tags: [networking, cisco, packet tracer, ipv6, subnetting, slaac, routing, network+]
---

## Overview

In this Packet Tracer lab, I implemented a subnetted IPv6 addressing scheme across a network containing two routers, four LANs, and a point-to-point connection between the routers.

The activity started with the IPv6 network `2001:DB8:ACAD:00C8::/64`. From this starting network, I created four additional consecutive `/64` subnets, assigned IPv6 addresses to the router interfaces, configured link-local addresses, enabled IPv6 routing, and configured the PCs to obtain their IPv6 addresses automatically.

The major objectives were:

- Determine the required IPv6 subnets.
- Assign IPv6 addresses to router interfaces.
- Configure link-local addresses.
- Enable IPv6 unicast routing.
- Configure hosts using IPv6 Auto Config.
- Verify IPv6 connectivity across the network.

## IPv6 Subnetting Scheme

The starting subnet provided by the activity was:

`2001:DB8:ACAD:00C8::/64`

Because each network uses a `/64` prefix, the subnet identifier is contained in the fourth hextet. I incremented this hextet by one for each additional network.

| Network | IPv6 Subnet |
|---|---|
| R1 G0/0 LAN | `2001:DB8:ACAD:00C8::/64` |
| R1 G0/1 LAN | `2001:DB8:ACAD:00C9::/64` |
| R2 G0/0 LAN | `2001:DB8:ACAD:00CA::/64` |
| R2 G0/1 LAN | `2001:DB8:ACAD:00CB::/64` |
| R1 to R2 Link | `2001:DB8:ACAD:00CC::/64` |

Each LAN receives its own `/64` IPv6 network, with the fourth hextet identifying the subnet.

## Addressing Table

The first IPv6 address in each LAN subnet was assigned to its router interface.

For the connection between R1 and R2, R1 received the first address and R2 received the second address.

| Device | Interface | IPv6 Address | Link-Local Address |
|---|---|---|---|
| R1 | G0/0 | `2001:DB8:ACAD:00C8::1/64` | `FE80::1` |
| R1 | G0/1 | `2001:DB8:ACAD:00C9::1/64` | `FE80::1` |
| R1 | S0/0/0 | `2001:DB8:ACAD:00CC::1/64` | `FE80::1` |
| R2 | G0/0 | `2001:DB8:ACAD:00CA::1/64` | `FE80::2` |
| R2 | G0/1 | `2001:DB8:ACAD:00CB::1/64` | `FE80::2` |
| R2 | S0/0/0 | `2001:DB8:ACAD:00CC::2/64` | `FE80::2` |
| PC1 | NIC | Auto Config | Automatic |
| PC2 | NIC | Auto Config | Automatic |
| PC3 | NIC | Auto Config | Automatic |
| PC4 | NIC | Auto Config | Automatic |

## Configuring R1

Before R1 could route IPv6 traffic between networks, IPv6 unicast routing needed to be enabled globally.

```text
enable
configure terminal
ipv6 unicast-routing
```

I then configured the first LAN interface:

```text
interface g0/0
ipv6 address 2001:DB8:ACAD:00C8::1/64
ipv6 address FE80::1 link-local
no shutdown
exit
```

The second LAN interface used the next `/64` subnet:

```text
interface g0/1
ipv6 address 2001:DB8:ACAD:00C9::1/64
ipv6 address FE80::1 link-local
no shutdown
exit
```

Finally, I configured R1's side of the serial connection:

```text
interface s0/0/0
ipv6 address 2001:DB8:ACAD:00CC::1/64
ipv6 address FE80::1 link-local
no shutdown
exit
end
```

R1 therefore used `FE80::1` as its manually assigned link-local address on each interface.

## Configuring R2

IPv6 routing also needed to be enabled on R2:

```text
enable
configure terminal
ipv6 unicast-routing
```

I configured R2 G0/0 for the PC3 LAN:

```text
interface g0/0
ipv6 address 2001:DB8:ACAD:00CA::1/64
ipv6 address FE80::2 link-local
no shutdown
exit
```

R2 G0/1 provided connectivity for the PC4 LAN:

```text
interface g0/1
ipv6 address 2001:DB8:ACAD:00CB::1/64
ipv6 address FE80::2 link-local
no shutdown
exit
```

Finally, I configured R2's side of the serial connection:

```text
interface s0/0/0
ipv6 address 2001:DB8:ACAD:00CC::2/64
ipv6 address FE80::2 link-local
no shutdown
exit
end
```

R2 used `FE80::2` as its manually configured link-local address.

## Configuring the PCs with SLAAC

The four PCs did not require manually assigned global IPv6 addresses.

On each PC, I opened:

**Desktop > IP Configuration**

I then selected **Auto Config** under IPv6 Configuration.

I repeated this process for:

- PC1
- PC2
- PC3
- PC4

This allows the hosts to use IPv6 Stateless Address Autoconfiguration (SLAAC).

With IPv6 routing enabled on the routers, Router Advertisement messages provide the PCs with the network prefix information necessary to automatically generate their own IPv6 addresses.

This eliminates the need to manually configure a unique global unicast IPv6 address on every host.

## Link-Local Addresses

This activity also demonstrated the importance of IPv6 link-local addresses.

R1 used:

`FE80::1`

R2 used:

`FE80::2`

Link-local addresses use the `FE80::/10` address space and are used for communication on the local network segment.

Unlike global unicast addresses, link-local addresses do not need to be unique throughout the entire routed network. They only need to be unique on the individual link.

This is why R1 can use `FE80::1` on multiple interfaces and R2 can use `FE80::2` on multiple interfaces.

## Enabling IPv6 Routing

One of the most important commands in this activity was:

```text
ipv6 unicast-routing
```

Configuring IPv6 addresses on router interfaces alone does not enable the router to forward IPv6 packets between networks.

The `ipv6 unicast-routing` command enables IPv6 packet forwarding and allows the routers to send Router Advertisements to hosts.

This is particularly important when the PCs are using SLAAC because the Router Advertisements provide the information the hosts need to configure themselves.

## Verifying the Network

After all router interfaces were configured and the PCs were set to IPv6 Auto Config, the network links became operational.

The PCs could then communicate across their respective IPv6 networks through R1 and R2.

The final Packet Tracer assessment confirmed that the addressing, host configuration, interfaces, and routing were all configured correctly.

![Packet Tracer Lab 31 completed with a score of 42 out of 42]({{ '/assets/images/lab31/results.png' | relative_url }})

The final results were:

| Assessment Component | Score |
|---|---:|
| Device Interface Configuration | 6/6 |
| IPv6 Address Configuration | 4/4 |
| IPv6 Host Address Calculation | 24/24 |
| IP | 6/6 |
| Routing | 2/2 |
| **Total** | **42/42** |

Packet Tracer also reported **30/30 assessment items correct**, completing the activity with a 100% score.

## Key Takeaways

This lab provided practical experience implementing IPv6 across multiple routed networks.

The most important concepts demonstrated were:

- IPv6 `/64` networks can be assigned consecutively by incrementing the subnet hextet.
- Each LAN receives its own IPv6 subnet.
- Router interfaces require global unicast and link-local IPv6 addresses.
- Link-local addresses can be reused on different interfaces as long as they are unique on each individual link.
- `ipv6 unicast-routing` must be enabled for a Cisco router to forward IPv6 traffic.
- SLAAC allows hosts to automatically generate IPv6 addresses using information received from Router Advertisements.
- Router Advertisements allow IPv6 hosts to learn network prefix information automatically.
- IPv6 addressing and SLAAC reduce the amount of manual host configuration required.

This activity tied IPv6 subnetting, router configuration, SLAAC, link-local addressing, and routing together into a complete working IPv6 network.
