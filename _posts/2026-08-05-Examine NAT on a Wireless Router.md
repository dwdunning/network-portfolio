---
layout: post
title: "Packet Tracer Lab 19 - Examine NAT on a Wireless Router"
date: 2026-08-05
categories: [Networking, Cisco, Packet Tracer, Module-14-Wireless]
tags: [networking, cisco, packet tracer, nat, dhcp, wireless, home-router]
---

## Objective

This lab examined how a wireless router uses Network Address Translation
(NAT) to allow devices with private IP addresses to communicate with a public
web server.

The activity included examining the router's WAN and LAN configurations,
connecting four PCs through DHCP, creating simulated HTTP traffic, and
observing the source IP address change as the packet crossed the wireless
router.

## Initial Network Topology

The starting topology contained PC0, a wireless router, an external network
cluster, and the `ciscolearn.nat.com` web server.

![Initial network topology.]({{ site.baseurl }}/assets/images/lab19/topology.png)

## Part 1 - Examine the External Network Configuration

PC0 was connected to one of the wireless router's LAN ports using a
straight-through Ethernet cable.

PC0 was then configured to obtain its IPv4 information automatically through
DHCP.

![PC0 receiving its configuration through DHCP.]({{ site.baseurl }}/assets/images/lab19/DHCP.png)

PC0 received the following configuration:

| Setting         | Value           |
|-----------------|-----------------|
| IPv4 address    | 192.168.1.100   |
| Subnet mask     | 255.255.255.0   |
| Default gateway | 192.168.1.1     |
| DNS server      | 209.165.200.226 |

The default gateway address, `192.168.1.1`, was used to access the wireless
router's web-based management interface.

The router's Internet status page showed the configuration assigned to its
WAN interface by the ISP DHCP server.

![Wireless router Internet status.]({{ site.baseurl }}/assets/images/lab19/router status.png)

| Internet setting | Value           |
|------------------|-----------------|
| Connection type  | DHCP            |
| Internet address | 209.165.200.227 |
| Subnet mask      | 255.255.255.224 |
| Default gateway  | 209.165.200.225 |
| DNS server       | 209.165.200.226 |

### Is the Internet address private or public?

The Internet address `209.165.200.227` is a **public IPv4 address**.

It does not belong to any of the private IPv4 ranges:

| Private range | Address block                 |
|---------------|-------------------------------|
| Class A       | 10.0.0.0/8                    |
| Class B       | 172.16.0.0 through 172.31.0.0 |
| Class C       | 192.168.0.0/16                |

## Part 2 - Examine the Internal Network Configuration

The Local Network status page displayed the router's internal address and its
DHCP server range.

![Wireless router LAN and DHCP status.]({{ site.baseurl }}/assets/images/lab19/LAN status.png)

| Local network setting | Value                         |
|-----------------------|-------------------------------|
| Router IP address     | 192.168.1.1                   |
| Subnet mask           | 255.255.255.0                 |
| DHCP server           | Enabled                       |
| Starting address      | 192.168.1.100                 |
| Ending address        | 192.168.1.149                 |
| DHCP pool             | 192.168.1.100 - 192.168.1.149 |

### Are the internal addresses private or public?

The internal `192.168.1.0/24` addresses are **private IPv4 addresses**.

Private addresses cannot be routed directly across the public Internet.
The wireless router must translate them into its public WAN address before
forwarding the traffic.

## Part 3 - Connect the Remaining PCs

Three additional PCs were connected to the remaining LAN ports on the
wireless router.

![Completed topology with four connected PCs.]({{ site.baseurl }}/assets/images/lab19/topology2.png)

Each PC was configured to receive its address through DHCP.

| Device | IPv4 address  | Default gateway |
|--------|---------------|-----------------|
| PC0    | 192.168.1.100 | 192.168.1.1     |
| PC1    | 192.168.1.101 | 192.168.1.1     |
| PC2    | 192.168.1.102 | 192.168.1.1     |
| PC3    | 192.168.1.103 | 192.168.1.1     |

The `ipconfig /all` command confirmed PC0's DHCP-assigned configuration.

![PC0 ipconfig output.]({{ site.baseurl }}/assets/images/lab19/IPPC0.png)

## Part 4 - Configure the Simulation

Packet Tracer was placed into Simulation mode.

The event filters were cleared, and only TCP and HTTP traffic were selected.

![TCP and HTTP simulation filters.]({{ site.baseurl }}/assets/images/lab19/filters.png)

A Complex PDU was then created from PC0 to the `ciscolearn.nat.com` server.

![Complex PDU configuration.]({{ site.baseurl }}/assets/images/lab19/complexPDU.png)

| PDU setting          | Value               |
|----------------------|---------------------|
| Source device        | PC0                 |
| Application          | HTTP                |
| Destination address  | 209.165.200.228     |
| Starting source port | 1000                |
| Destination port     | 80                  |
| Simulation type      | Periodic            |
| Interval             | 120 seconds         |

The simulation was advanced one event at a time so the packet could be
examined as it entered and exited the wireless router.

## Part 5 - Observe NAT Translation

The PDU information at the wireless router displayed the Layer 3 header before
and after the packet was processed.

![NAT translation shown in the PDU details.]({{ site.baseurl }}/assets/images/lab19/pdu.png)

### Packet entering the router

| Header field   | Value           |
|----------------|-----------------|
| Source IP      | 192.168.1.100   |
| Destination IP | 209.165.200.228 |

### Packet leaving the router

| Header field   | Value           |
|----------------|-----------------|
| Source IP      | 209.165.200.227 |
| Destination IP | 209.165.200.228 |

The wireless router changed the packet's source address from the private
address `192.168.1.100` to its public WAN address `209.165.200.227`.

The destination address remained `209.165.200.228` because the packet was
still being sent to the same web server.

| Packet stage | Source IP      | Destination IP |
|--------------|----------------|----------------|
| Before NAT   | 192.168.1.100  | 209.165.200.228|
| After NAT    | 209.165.200.227| 209.165.200.228|

This translation allows devices on the private LAN to communicate with
external networks while sharing the wireless router's public address.

## Results

Packet Tracer confirmed that the activity was completed successfully.

![Completed activity results.]({{ site.baseurl }}/assets/images/lab19/results.png)

## What I Learned

This lab demonstrated the relationship between DHCP, private addressing,
public addressing, and NAT.

The main observations were:

- The wireless router used separate addresses for its LAN and WAN interfaces.
- DHCP assigned private addresses to the four internal PCs.
- The internal clients used `192.168.1.1` as their default gateway.
- The router received the public address `209.165.200.227` from the ISP.
- NAT replaced PC0's private source address with the router's public address.
- The destination server address remained unchanged.
- Multiple private devices can share one public IPv4 address when accessing
  external networks.
