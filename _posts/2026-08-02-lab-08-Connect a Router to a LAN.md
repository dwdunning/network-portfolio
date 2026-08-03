---
layout: post
title: "Packet Tracer Lab 08 - Connect a Router to a LAN"
date: 2026-08-02
categories: [Networking, Cisco, Packet Tracer, Module-11-Network-Topologies]
tags: [Cisco, Packet Tracer, Routing, LAN, CCNA, Network+, IPv4]

---

In this lab, I configured router interfaces to connect multiple local area networks (LANs) and verified connectivity between them. The activity focused on examining router interface information, assigning IPv4 addresses, bringing interfaces online, documenting connections with interface descriptions, and verifying the resulting configuration using Cisco IOS show commands.

Unlike previous labs that emphasized switches or basic connectivity, this exercise introduced configuring multiple routed interfaces and interpreting the router's routing table.

## Objectives

- Examine router interface information using Cisco IOS show commands.
- Configure Gigabit Ethernet interfaces with IPv4 addressing.
- Activate interfaces using the `no shutdown` command.
- Document interfaces with descriptions.
- Save the router configuration to NVRAM.
- Verify interface status and routing information.
- Test end-to-end network connectivity.

## Network Topology

The completed topology consists of two interconnected routers, each providing connectivity to two separate LANs.

![Completed network topology.]({{ site.baseurl }}/assets/images/lab08/topology.png)

## Displaying Router Information

Before making any configuration changes, I explored several useful Cisco IOS verification commands:

- `show interfaces`
- `show interfaces serial 0/0/0`
- `show interfaces gigabitethernet 0/0`
- `show ip interface brief`
- `show ip route`

These commands provided information about:

- Interface status
- Assigned IP addresses
- MAC addresses
- Bandwidth
- Routing table entries
- Connected and local routes

One useful observation was that the Gigabit Ethernet interfaces initially appeared as **administratively down** because they had not yet been enabled with the `no shutdown` command.

## Configuring Router Interfaces

Using the addressing table provided by the lab, I configured each Gigabit Ethernet interface on both routers.

Each interface required:

- Assigning the correct IPv4 address and subnet mask
- Bringing the interface online with `no shutdown`
- Adding an interface description documenting the connected switch

For example:

```text
interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 description LAN connection to S1
 no shutdown
```

After enabling each interface, Cisco IOS immediately reported that both the physical interface and line protocol had transitioned to the **up** state.

## Saving the Configuration

After verifying the interfaces were operating correctly, I saved the running configuration to NVRAM on both routers using:

```text
copy running-config startup-config
```

Saving the configuration ensures that the router retains its settings after a reboot or power cycle.

## Verification

After configuration, I verified the router interfaces using:

```text
show ip interface brief
```

This confirmed that:

- All configured interfaces had the correct IPv4 addresses.
- Interfaces were in the **up/up** state.
- Serial connectivity between the routers remained operational.

I also used:

```text
show ip route
```

to verify that the routing table contained the expected connected routes.

The CLI below shows the completed interface configuration and successful verification on R2.

![Router CLI showing completed interface configuration and verification.]({{ site.baseurl }}/assets/images/lab08/cli.png)

## Results

After configuring both routers and saving their configurations, Packet Tracer awarded a perfect score.

![Packet Tracer assessment results showing a perfect score.]({{ site.baseurl }}/assets/images/lab08/results.png)

**Final Score:** **54 / 54**

## Skills Demonstrated

- Cisco IOS CLI navigation
- Router interface configuration
- IPv4 addressing
- Interface activation using `no shutdown`
- Interface documentation using descriptions
- Configuration verification using Cisco IOS show commands
- Reading routing tables
- Saving configurations to NVRAM
- Basic router-to-router LAN connectivity

## Reflection

This lab reinforced the importance of verifying device state before and after configuration. The various `show` commands made it easy to inspect interface status, routing information, and hardware details before assigning addresses.

I also gained additional experience working with router interfaces, understanding the difference between configured and active interfaces, and verifying that changes had been successfully written to startup configuration. These are foundational Cisco administration skills that directly support future routing, dynamic routing protocols, and troubleshooting exercises.
