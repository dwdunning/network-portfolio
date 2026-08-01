---
layout: post
title: "Packet Tracer Lab 5: Basic Switch and End Device Configuration"
date: 2026-07-31
categories: [networking, cisco, packet-tracer]
tags: [Network+, Cisco, Packet Tracer, Switches, IPv4, VLAN]
---

# Cisco Packet Tracer Lab 5: Basic Switch and End Device Configuration

*Continuing my hands-on networking practice for CompTIA Network+ and Cisco networking fundamentals.*

## Objective

This lab focused on configuring two Cisco switches and two end devices to create a functioning local area network (LAN). Unlike my previous switch configuration lab, this exercise required using a **console connection** to access each switch, simulating the initial setup of a new switch before remote management is available.

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

- Two Cisco Catalyst 2960 switches
  - Room-145
  - Room-146
- Two PCs
  - Manager
  - Reception

The switches were connected together, with one PC attached to each switch.

![Lab topology](/assets/images/lab5-topology.png)

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

Each PC received a static IPv4 address based on the addressing table provided in the lab.

---

## Challenge Encountered

This lab intentionally disabled direct access to the switches' configuration interfaces. At first, clicking on the switches produced a message stating that the interface was locked, which made it seem like something was wrong with the activity.

The solution was to connect a **console cable** from a PC to each switch and use the **Terminal** application to access the Cisco IOS command-line interface.

This was a valuable lesson because it reflects how brand-new Cisco switches are configured in real environments before network management is available.

---

## Results

After configuring both switches and assigning static IP addresses to the PCs, all Packet Tracer assessment items and connectivity tests passed successfully.

**Final Score: 98/98 (100%)**

![Lab results](/assets/images/lab5-results.png)

---

## What I Learned

This lab reinforced several networking concepts:

- Initial Cisco switch configuration
- Console-based device management
- VLAN 1 management addressing
- Static IPv4 addressing
- Password security best practices
- Saving configurations to non-volatile memory (NVRAM)
- Verifying connectivity between hosts

The biggest takeaway was understanding why the **console port** exists. Previous labs allowed direct access to the switch CLI, but this exercise demonstrated the process used to configure a new switch before it has a management IP address.

---

## Reflection

Each Packet Tracer lab builds on the previous one. This exercise expanded beyond basic switch configuration by introducing console-based management, making the experience feel much closer to configuring real Cisco hardware.

I also spent some time troubleshooting why the switches appeared to be "locked." Figuring out that the lab required a console connection was a useful reminder that understanding how to access a device is just as important as knowing which commands to enter. That troubleshooting process ended up being one of the most valuable parts of the exercise.