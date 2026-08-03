---
layout: post
title: "Packet Tracer Lab 10 - Implement Basic Connectivity"
date: 2026-08-03
categories: [Cisco, Networking, Packet Tracer]
tags: [packet tracer, cisco, switch, vlan, ipv4, network+, lab]
---

## Overview

In this lab, I configured two Cisco switches with basic security settings, assigned management IP addresses to the VLAN 1 interface, configured two PCs with static IPv4 addresses, and verified end-to-end connectivity across the network. This exercise reinforced the importance of switch management interfaces and basic Layer 2 network configuration.

## Objectives

- Configure hostnames on Cisco switches.
- Configure console and privileged EXEC passwords.
- Configure a login banner.
- Save switch configurations to NVRAM.
- Configure static IPv4 addresses on end devices.
- Configure management IP addresses on VLAN 1.
- Verify configurations using Cisco IOS commands.
- Test network connectivity using the `ping` command.

## Step 1 - Configure the Switches

Both S1 and S2 were configured with:

- Hostname
- Console password
- Enable secret password
- MOTD banner
- Saved startup configuration

The configuration was saved using:

```text
copy running-config startup-config
```

This copies the running configuration stored in RAM into NVRAM so it is retained after a reboot.

## Step 2 - Configure the PCs

Each PC was assigned a static IPv4 address using the **Desktop → IP Configuration** utility.

| Device | IP Address | Subnet Mask |
| ------ | ---------- | ----------- |
| PC1 | 192.168.1.1 | 255.255.255.0 |
| PC2 | 192.168.1.2 | 255.255.255.0 |

Because this is a single local network, no default gateway was required.

![PC1 static IPv4 configuration.]({{ site.baseurl }}/assets/images/lab10/ip-config.png)

## Step 3 - Configure the Switch Management Interface

Unlike routers, switches do not require IP addresses to forward Ethernet frames. However, assigning an IP address to VLAN 1 allows administrators to remotely manage the switch using tools such as SSH, Telnet, or SNMP.

The following commands were used on each switch:

```text
interface vlan 1
ip address 192.168.1.253 255.255.255.0
no shutdown
```

The `no shutdown` command enabled the management interface so it could communicate on the network.

Verification was performed using:

```text
show ip interface brief
```

which confirmed that VLAN 1 was configured with the correct IP address and was operational.

![S1 interface status showing the VLAN 1 management interface.]({{ site.baseurl }}/assets/images/lab10/s1-ip-brief.png)

## Step 4 - Verify Connectivity

After configuring both switches and PCs, I tested connectivity using the `ping` command.

Successful communication was verified between:

- PC1 and PC2
- PC1 and S1
- PC1 and S2

The first ping to each switch timed out once before succeeding. This is expected behavior because the devices must first resolve the destination MAC address using the Address Resolution Protocol (ARP). Once the ARP table is populated, subsequent pings complete successfully.

![Successful ping tests between the PCs and both switches.]({{ site.baseurl }}/assets/images/lab10/ping.png)

## Packet Tracer Assessment

The completed activity received a perfect score.

- **Overall Score:** 88/88
- **Assessment Items Completed:** 22/22

![Packet Tracer assessment results showing a perfect score.]({{ site.baseurl }}/assets/images/lab10/results.png)

## Reflection

This lab demonstrated that Layer 2 switches can forward network traffic without having IP addresses, but management tasks require a configured management interface. Configuring VLAN 1 with an IPv4 address allows administrators to remotely monitor and manage the switch while maintaining normal switching operations.

The exercise also reinforced several foundational Cisco IOS skills that appear throughout both Cisco networking courses and the CompTIA Network+ certification objectives, including basic device configuration, securing console access, assigning static IPv4 addresses, verifying configurations with `show` commands, and testing connectivity with `ping`.
