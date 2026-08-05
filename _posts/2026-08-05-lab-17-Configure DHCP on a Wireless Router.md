---
layout: post
title: "Packet Tracer Lab 17 - Configure DHCP on a Wireless Router"
date: 2026-08-05
categories: [Networking, Cisco, Packet Tracer, Module-14-Network Communications Protocols]
tags: [networking, cisco, packet tracer, dhcp, wireless router, ipv4]
---

## Overview

This lab focused on configuring and verifying DHCP services on a consumer wireless router. Instead of manually assigning IP addresses, the router automatically distributed network configuration information to connected clients. I modified the router's default network, adjusted the DHCP address pool, renewed client leases, and verified end-to-end connectivity between all devices.

## Objectives

- Connect three PCs to a wireless router.
- Configure clients to obtain IP addresses automatically using DHCP.
- Change the router's default LAN IP address.
- Modify the DHCP address pool.
- Verify IP addressing and network connectivity.

## Initial Topology

The activity began with a DHCP-enabled wireless router that we connected to three PCs using copper straight-through cables.

![Initial network topology.]({{ site.baseurl }}/assets/images/lab17/initial-topology.png)

## Obtaining the Initial DHCP Configuration

After configuring PC0 to obtain its address automatically, the router assigned the following information:

| Setting | Value |
|---------|-------|
| IPv4 Address | 192.168.0.100 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.0.1 |

Using the default gateway address, I logged into the router's web interface with the default administrator credentials.

The default configuration showed:

- Router IP: **192.168.0.1**
- DHCP Enabled
- Starting IP Address: **192.168.0.100**
- Maximum Users: **50**

![Default router configuration.]({{ site.baseurl }}/assets/images/lab17/webUI.png)

## Changing the Router Network

The router's LAN address was changed from **192.168.0.1** to:

```text
192.168.5.1
```

After saving the configuration, the browser displayed a timeout because the router immediately switched to the new network address.

This behavior is expected since the PC was still attempting to communicate with the previous IP address.

![Expected timeout after changing the router IP.]({{ site.baseurl }}/assets/images/lab17/IPerror.png)

## Renewing the DHCP Lease

After renewing the DHCP lease, PC0 received a new address from the updated network.

| Setting | Value |
|---------|-------|
| IPv4 Address | 192.168.5.101 |
| Default Gateway | 192.168.5.1 |

![PC0 receiving an address on the new subnet.]({{ site.baseurl }}/assets/images/lab17/newIP.png)

## Configuring the DHCP Address Pool

Once logged back into the router using the new address (**192.168.5.1**), I modified the DHCP server configuration.

The DHCP pool was changed to:

| Setting | Value |
|---------|-------|
| Start IP Address | 192.168.5.126 |
| Maximum Users | 75 |

This produced an available DHCP range of:

```text
192.168.5.126 - 192.168.5.200
```

![Updated DHCP address pool.]({{ site.baseurl }}/assets/images/lab17/DHCP-Pool.png)

## Verifying Client Address Assignment

After renewing its lease again, PC0 received the first address from the new DHCP pool.

| Device | Assigned Address |
|---------|-----------------|
| PC0 | 192.168.5.126 |

![PC0 receiving the first address from the updated DHCP pool.]({{ site.baseurl }}/assets/images/lab17/pc0ip.png)

DHCP was then enabled on the remaining clients.

| Device | Assigned Address |
|---------|-----------------|
| PC1 | 192.168.5.127 |
| PC2 | 192.168.5.128 |

The sequential addressing confirmed that the DHCP pool was functioning correctly.

![PC2 automatically receiving its DHCP configuration.]({{ site.baseurl }}/assets/images/lab17/pc2.png)

## Connectivity Verification

Using the command prompt, I successfully verified communication with:

- The wireless router (192.168.5.1)
- PC0 (192.168.5.126)
- PC1 (192.168.5.127)

All ping requests completed successfully, confirming proper IP addressing and connectivity throughout the network.

## Results

The Packet Tracer assessment verified that every required task was completed successfully.

- Router IP updated
- DHCP pool modified
- All clients obtained addresses automatically
- Connectivity verified
- **Score: 9/9 (100%)**

![Packet Tracer assessment results.]({{ site.baseurl }}/assets/images/lab17/results.png)

## What I Learned

This lab demonstrated how DHCP simplifies network administration by automatically assigning IP addresses and other network settings to clients. Changing the router's LAN address also required renewing each client's DHCP lease before communication could continue. Adjusting the DHCP scope allowed me to control both the starting address and the total number of available client addresses, illustrating how DHCP pools are customized to meet different network requirements.
