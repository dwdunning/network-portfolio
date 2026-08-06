---
layout: post
title: "Packet Tracer Lab 26 - Network Representation"
date: 2026-08-06
categories: [Networking, Cisco, Packet-Tracer, Module-16-Network-Systems]
tags: [networking, cisco, packet tracer, lan, wan, routers, switches, topology]
---

## Overview

In this lab, I explored a complete small-to-medium business network built in Cisco Packet Tracer. Unlike previous labs, there was no device configuration or troubleshooting required. Instead, the objective was to become familiar with how common networking devices, end devices, and media are represented within a realistic network topology.

The activity combined a home office, central office, and branch office connected through both the Internet and an intranet. This provided a good overview of how different network components work together.

---

## Network Topology

The completed Packet Tracer topology is shown below.

![Packet Tracer network representation topology.]({{ site.baseurl }}/assets/images/lab26/network-topology.png)

---

## Identifying Network Components

### Intermediary Device Categories

Packet Tracer organizes intermediary devices into several categories:

- Routers
- Switches
- Hubs
- Wireless Devices
- Security
- WAN Emulation

These devices forward traffic between end devices and make communication across the network possible.

---

### Endpoint Devices

Endpoint devices have only a single network connection. The topology contains **15 endpoint devices**:

| Device Type | Quantity |
| :---------- | -------: |
| Desktop PCs | 6 |
| Laptops | 2 |
| Servers | 2 |
| Printers | 2 |
| Tablet | 1 |
| Smart Phone | 1 |
| Guest Laptop | 1 |
| **Total** | **15** |

---

### Intermediary Devices

Devices with multiple network connections act as intermediary devices.

| Device Type | Quantity |
| :---------- | -------: |
| Routers | 3 |
| Switches | 6 |
| Wireless Router | 1 |
| Wireless Access Point | 1 |
| Modem | 1 |
| IP Phones | 2 |
| **Total** | **13** |

---

### End Devices That Are Not Desktop Computers

The topology contains **8** end devices that are not desktop PCs:

- Home laptop
- Tablet
- Inkjet printer
- Central server
- Branch server
- Laser printer
- Smart phone
- Guest laptop

---

## Network Media

The topology demonstrates four different connection types:

| Media Type | Example |
| :--------- | :------ |
| Ethernet | Wired connections between PCs, switches, and routers |
| Wireless | Connections between mobile devices and wireless access points |
| Serial WAN | Connections between routers and WAN clouds |
| Cable/DSL | Modem connection to the Internet |

Each media type is selected based on the needs of the network, such as speed, distance, mobility, or Internet connectivity.

---

## Client-Server Model

The client-server model allows client devices to request services from centralized servers.

Examples of services include:

- Web hosting
- File storage
- Email
- DNS
- DHCP

This architecture simplifies administration while improving scalability and security.

---

## Functions of Intermediary Devices

Intermediary devices perform several important networking functions, including:

- Forwarding packets between devices
- Determining the best path for network traffic
- Connecting multiple networks together
- Filtering and securing network traffic
- Managing network communication

Without intermediary devices, communication between separate networks would not be possible.

---

## Choosing Network Media

Several factors influence which media type should be used:

- Required bandwidth
- Maximum transmission distance
- Installation cost
- Environmental conditions
- Resistance to electromagnetic interference
- Security requirements

No single media type is ideal for every situation.

---

## LANs and WANs

A **Local Area Network (LAN)** connects devices within a limited geographic area, such as a home, office, or campus.

A **Wide Area Network (WAN)** connects multiple LANs over much greater distances.

In this topology:

| Network Type | Quantity |
| :----------- | -------: |
| LANs | 3 |
| WANs | 2 |

The LANs include:

- Home Office
- Central Office
- Branch Office

The WANs include:

- Internet
- Intranet

---

## Internet Connectivity

Home users commonly connect to the Internet using:

- Cable Internet
- Fiber
- DSL
- Satellite
- Fixed wireless
- Cellular (4G/5G)

Businesses often use:

- Dedicated fiber
- Business cable
- Metro Ethernet
- MPLS
- Leased lines
- Fixed wireless
- Cellular backup connections

---

## What I Learned

Although this lab did not require any configuration, it was useful for seeing how the individual networking concepts from previous Packet Tracer labs fit together into a complete enterprise network.

The topology demonstrated the relationships between routers, switches, wireless devices, servers, printers, IP phones, and end-user devices while also illustrating how LANs communicate across WAN connections. It also reinforced the distinction between endpoint devices and intermediary devices, along with the different media types used throughout a modern business network.

This activity served as a helpful overview before moving into more advanced Cisco networking topics.
