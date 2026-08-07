---
layout: post
title: "Packet Tracer Lab 30 – Deploy and Cable Devices"
date: 2026-08-07
categories: [Networking, Cisco, Packet Tracer, Module-16-Routing]
tags: [networking, cisco, packet tracer, cabling, switches, ethernet, physical-layer, network-topology]
---

## Overview

In this lab, I deployed network devices in Cisco Packet Tracer and connected them using the appropriate Ethernet cable types and switch interfaces.

The completed network consisted of two Cisco 2960 switches and six PCs. Each group of three PCs was connected to a switch using copper straight-through cables, while the two switches were connected using a copper cross-over cable.

This lab provided hands-on practice with device deployment, Ethernet interfaces, cable selection, and basic network topology construction.

## Objectives

- Deploy Cisco 2960 switches in Packet Tracer.
- Deploy PC end devices.
- Connect PCs to switches using copper straight-through cables.
- Connect two switches using a copper cross-over cable.
- Identify the appropriate FastEthernet and GigabitEthernet interfaces.
- Verify that the completed physical topology is correct.

## Deploying the Network Devices

The first step was to deploy all of the required devices into the logical workspace.

The topology required:

| Device Type | Quantity |
|---|---:|
| Cisco 2960 Switch | 2 |
| PC-PT | 6 |

The switches were placed as `Switch0` and `Switch1`.

The six PCs were placed around the switches as:

- PC0
- PC1
- PC2
- PC3
- PC4
- PC5

PC0 through PC2 were associated with Switch0, while PC3 through PC5 were associated with Switch1.

## Connecting the PCs to the Switches

Each PC was connected to its respective switch using a **Copper Straight-Through** cable.

The connections were made according to the following table:

| PC | PC Interface | Switch | Switch Interface | Cable |
|---|---|---|---|---|
| PC0 | FastEthernet0 | Switch0 | FastEthernet0/1 | Copper Straight-Through |
| PC1 | FastEthernet0 | Switch0 | FastEthernet0/2 | Copper Straight-Through |
| PC2 | FastEthernet0 | Switch0 | FastEthernet0/3 | Copper Straight-Through |
| PC3 | FastEthernet0 | Switch1 | FastEthernet0/1 | Copper Straight-Through |
| PC4 | FastEthernet0 | Switch1 | FastEthernet0/2 | Copper Straight-Through |
| PC5 | FastEthernet0 | Switch1 | FastEthernet0/3 | Copper Straight-Through |

This demonstrates the traditional Ethernet cabling rule of using straight-through cables when connecting different types of Ethernet devices, such as a PC and a switch.

## Connecting the Switches

After connecting all six PCs, I connected the two switches together.

Because this connection was between two devices of the same type, the lab required a **Copper Cross-Over** cable.

The connection was:

| Device | Interface | Device | Interface | Cable |
|---|---|---|---|---|
| Switch0 | GigabitEthernet0/1 | Switch1 | GigabitEthernet0/1 | Copper Cross-Over |

The switch-to-switch connection appears as a dashed cable in Packet Tracer, distinguishing it visually from the straight-through connections used for the PCs.

After allowing the switches time to initialize their ports, the link indicators changed to green.

## Completed Topology

The final network contained all six PCs connected to their respective switches, with the two switches connected through their GigabitEthernet interfaces.

![Completed Packet Tracer topology showing six PCs and two Cisco 2960 switches](/assets/images/lab30/final-topology.png)

All of the visible link indicators were green, confirming that the physical connections were operational.

## Cable Selection

One of the main concepts demonstrated in this lab was selecting the appropriate Ethernet cable for each connection.

| Connection | Cable Type |
|---|---|
| PC to Switch | Copper Straight-Through |
| Switch to Switch | Copper Cross-Over |

The completed topology therefore used:

- **6 copper straight-through cables**
- **1 copper cross-over cable**
- **7 Ethernet connections total**

Modern Ethernet equipment commonly supports Auto-MDI/MDIX, which can automatically compensate for cable wiring differences. However, understanding traditional straight-through and cross-over cable selection remains useful for networking fundamentals and troubleshooting.

## Interface Selection

The lab also reinforced the importance of identifying the correct interfaces on networking devices.

Each PC used:

`FastEthernet0`

The first three access ports on each switch were:

`FastEthernet0/1`

`FastEthernet0/2`

`FastEthernet0/3`

The inter-switch connection used:

`GigabitEthernet0/1`

This reflects a common network design in which end devices connect to switch access ports while higher-speed GigabitEthernet interfaces can be used for connections between networking devices.

## Verification

After completing the topology, Packet Tracer's activity assessment confirmed that the required connections and cable types were correct.

![Packet Tracer activity results showing successful completion](/assets/images/lab30/results.png)

The final assessment reported:

| Assessment | Result |
|---|---:|
| Score | 2/2 |
| Item Count | 2/2 |
| Connection | Correct |
| Cable Type | Correct |
| Activity Status | Completed |

Packet Tracer displayed:

> Congratulations Guest! You completed the activity.

## What I Learned

This lab reinforced the physical side of Ethernet networking rather than IP addressing or device configuration.

The most important concepts were:

- Network devices must be connected through the appropriate physical interfaces.
- PCs normally connect to switch access ports using copper straight-through cables.
- Traditional switch-to-switch Ethernet connections use copper cross-over cables.
- FastEthernet and GigabitEthernet interfaces have different names and must be selected correctly.
- Link indicators provide immediate information about the physical state of a connection.
- A network must have a functioning physical connection before higher-layer configuration and communication can work.

Unlike the previous subnetting and VLSM labs, this activity did not require IP addressing or Cisco IOS configuration. The focus was entirely on deploying devices and constructing the physical network topology correctly.

## Conclusion

Module 16 Lab 30 provided hands-on practice deploying and cabling a small switched Ethernet network in Cisco Packet Tracer.

The completed topology consisted of six PCs connected across two Cisco 2960 switches. Copper straight-through cables connected the PCs to the switches, while a copper cross-over cable connected the two switches through their GigabitEthernet0/1 interfaces.

After all seven connections were completed, the links became operational and Packet Tracer confirmed successful completion with a **2/2 score**.
