---
layout: post
title: "Packet Tracer Lab 09 - Connect the Physical Layer"
date: 2026-08-02
categories: [Networking, Cisco, Packet Tracer, module-12]
tags: [Network+, Cisco, Packet Tracer, Physical Layer, Cabling, Switching, Routing]
---


## Overview

This lab focused on the physical layer of a network by exploring router and switch hardware, selecting appropriate expansion modules, installing new hardware, connecting devices with the correct cable types, and verifying end-to-end connectivity. Unlike earlier Packet Tracer exercises that focused primarily on configuration, this lab emphasized the physical components that make network communication possible.

## Objectives

- Identify management, LAN, and WAN interfaces on a Cisco router.
- Explore modular expansion options for routers and switches.
- Select the appropriate hardware modules for specific networking requirements.
- Install expansion modules by powering devices down safely.
- Connect devices using the correct cable types.
- Verify interface status and network connectivity.

## Initial Topology

The lab began with an incomplete network containing routers, switches, PCs, an access point, a DHCP server, and wireless devices that still required hardware installation and cabling.

![Initial Packet Tracer topology.]({{ site.baseurl }}/assets/images/lab09/initial-topology.png)

## Exploring Router and Switch Hardware

The first task was identifying the physical interfaces available on the Cisco 1941 router. I examined the router's physical view and identified the available management interfaces, including the Console, USB Console, and AUX ports.

Using the IOS CLI, I verified the router's available interfaces with:

```text
show ip interface brief
```

The router contained:

- Two Gigabit Ethernet LAN interfaces
- Two Serial WAN interfaces
- Four physical interfaces in total

I also inspected the default bandwidth values for both Gigabit Ethernet and Serial interfaces using the `show interface` command, reinforcing the difference between Ethernet and WAN interface characteristics.

One interesting part of this lab was examining the modular switch. Packet Tracer presents the switch as a collection of expansion positions that include installed interface modules, blank covers, and an available slot for new hardware. Distinguishing between existing interface modules and available expansion bays helped me better understand how modular network devices are represented.

![Switch2 physical view showing modular expansion slots.]({{ site.baseurl }}/assets/images/lab09/switch2.png)

## Installing Expansion Modules

The lab required selecting hardware based on networking needs instead of simply following configuration instructions.

For the East router, I installed the **HWIC-4ESW** module, which provides four switched Fast Ethernet ports. This allowed three PCs to connect directly to the router without requiring an additional switch.

For Switch2, I installed the **PT-SWITCH-NM-1FGE** module to provide a Gigabit fiber interface capable of connecting to Switch3.

An important lesson from this section was that Cisco router interfaces are **not hot-swappable**. Attempting to install a module while the router was powered on generated an error, requiring the device to be powered off before hardware could be added.

## Connecting the Network

After installing the required modules, I completed the network using multiple cable types.

The completed topology included:

- Copper straight-through cables
- Copper cross-over cable
- Fiber optic connection
- Serial DCE WAN connection
- Wireless connectivity
- Cellular connectivity

Choosing the correct media for each connection reinforced how different physical connections serve different networking purposes.

- **Copper Straight-Through** connected routers, switches, PCs, and the access point.
- **Copper Cross-Over** connected two switches.
- **Fiber** connected the Gigabit fiber interfaces between Switch2 and Switch3.
- **Serial DCE** connected the East and West routers across the simulated WAN.

After completing the cabling, the network topology was fully connected.

![Completed network topology with wired, fiber, serial, wireless, and cellular connections.]({{ site.baseurl }}/assets/images/lab09/final-topology.png)

## Verifying Connectivity

Using the CLI, I verified that all required interfaces transitioned to the expected operational state.

```text
show ip interface brief
```

The interface status matched the expected results provided by the lab, confirming that the hardware installation and cabling were completed correctly.

![East router interface verification using show ip interface brief.]({{ site.baseurl }}/assets/images/lab09/east-ipbrief.png)

I also verified connectivity by:

- Connecting the Laptop via Wi-Fi.
- Browsing to the internal Cisco Packet Tracer web server.
- Connecting the TabletPC through Wi-Fi.
- Switching the TabletPC from wireless to the simulated 3G/4G cellular connection.
- Confirming that web access remained functional using the alternate network path.

These tests demonstrated that both wired and wireless components of the network were functioning correctly.

## Results

The completed activity earned a perfect score.

![Packet Tracer activity completed with a score of 51/51.]({{ site.baseurl }}/assets/images/lab09/results.png)

## Reflection

This lab felt much closer to working with real networking equipment than previous Packet Tracer exercises. Instead of only configuring devices through the CLI, I had to understand how hardware modules affect network capabilities, when devices must be powered down before upgrades, and why different media types are appropriate for different connections.

The discussion around the modular switch was particularly valuable because it required examining the physical hardware carefully rather than simply following instructions. Understanding how Packet Tracer models expansion slots, installed modules, and blank covers helped me think more critically about modular networking equipment.

I also gained a better appreciation for why selecting the correct cable type matters. Using straight-through, cross-over, fiber, and serial cables appropriately reinforced how the physical layer supports everything built on top of it.

## Key Takeaways

- Cisco routers support modular hardware expansion but often require powering down before installation.
- Different expansion modules provide different networking capabilities, such as switched Ethernet ports or fiber interfaces.
- Selecting the correct cable type is just as important as selecting the correct interface.
- `show ip interface brief` is a quick way to verify interface status after making physical changes.
- Physical layer knowledge is essential because every higher network layer depends on a functioning physical infrastructure.
  