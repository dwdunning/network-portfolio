---
layout: post
title: "Packet Tracer Lab 05: Basic Switch and End Device Configuration"
date: 2026-07-31
categories: [networking, cisco, packet-tracer]
tags: [Network+, Cisco, Packet Tracer, Switches, IPv4, VLAN]
---

*Continuing my hands-on networking practice for CompTIA Network+ and Cisco networking fundamentals.*

## Objective

This lab focused on configuring two Cisco switches and two end devices to create a functioning local area network (LAN). Unlike my previous switch configuration lab, this exercise required using a console connection to access each switch, simulating the initial setup of a new switch before remote management was available.

The objectives included:

- Configuring switch hostnames
- Setting console and privileged mode passwords
- Encrypting passwords
- Configuring a Message of the Day (MOTD) banner
- Assigning management IP addresses to VLAN 1
- Configuring static IP addresses on two PCs
- Saving configurations to NVRAM
- Verifying end-to-end network connectivity

---

## Network Topology

The lab consisted of:

- Two Cisco Catalyst 2960 switches:
  - Room-145
  - Room-146
- Two PCs:
  - Manager
  - Reception

The switches were connected together, with one PC attached to each switch.

![Lab topology.]({{ site.baseurl }}/assets/images/lab05/lab.png)

---

## Configuration Tasks

Each switch was configured through the console using the Cisco IOS command-line interface (CLI).

Configuration included:

- Assigning hostnames
- Configuring the enable secret password
- Configuring console access passwords
- Configuring VTY passwords
- Encrypting plaintext passwords
- Creating an MOTD banner
- Assigning a management IP address to VLAN 1
- Saving the running configuration to startup-config

Each PC received a static IPv4 address according to the addressing table provided in the lab.

---

## Challenge Encountered

Unlike previous Packet Tracer labs, the switches could not be accessed directly. Clicking on a switch displayed a message indicating that the interface was locked. This initially appeared to be a problem with the activity.

The solution was to connect a console cable from a PC to each switch and use the **Terminal** application to access the Cisco IOS CLI. This mirrors how new Cisco switches are configured before a management IP address has been assigned.

Working through this issue reinforced the importance of understanding not only the configuration commands but also the different methods used to access network devices.

---

## Results

After configuring both switches and assigning static IP addresses to the PCs, all Packet Tracer assessment items and connectivity tests passed successfully.

### Final Score: 98/98 (100%)

![Lab results.]({{ site.baseurl }}/assets/images/lab05/results.png)

---

## What I Learned

This lab reinforced several networking concepts:

- Initial Cisco switch configuration
- Console-based device management
- VLAN 1 management addressing
- Static IPv4 addressing
- Password security best practices
- Saving configurations to non-volatile memory (NVRAM)
- Verifying end-to-end connectivity between hosts

The biggest takeaway was understanding the purpose of the console port. Previous labs allowed direct access to the switch CLI, but this exercise demonstrated the process used to configure a new switch before it can be managed over the network.

---

## Reflection

Each Packet Tracer lab builds on the previous one. This exercise expanded beyond basic switch configuration by introducing console-based management, making the experience much closer to configuring real Cisco hardware.

Troubleshooting the locked interfaces turned out to be one of the most valuable parts of the lab. Once I realized the activity expected console access instead of direct CLI access, the rest of the configuration process made much more sense. It was a good reminder that successful network administration depends on understanding both device configuration and the methods used to access those devices.
