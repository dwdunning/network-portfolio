---
layout: post
title: "Packet Tracer Lab 21 - Secure Network Devices"
date: 2026-08-05
categories: [Networking, Cisco, Packet Tracer, Module-15-Routing]
tags: [networking, cisco, packet tracer, router, switch, ssh, security, passwords]
---

## Overview

In this lab, I secured a router and switch by implementing several Cisco IOS security best practices. The activity focused on securing local and remote management access, configuring SSH, strengthening password policies, and disabling unused switch interfaces. After completing the required configurations and correcting the status of the active switch ports, the activity completed successfully with a perfect score.

## Objectives

- Configure secure router and switch management.
- Disable DNS lookups for mistyped commands.
- Configure secure passwords and password encryption.
- Configure a local administrator account.
- Enable SSH for remote management.
- Configure VTY lines to accept only SSH connections.
- Configure login protection against brute force attacks.
- Configure a switch management interface.
- Disable all unused switch ports.

## Addressing Table

|  Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---------|-----------|------------|-------------|-----------------|
| RTR-A | G0/0/0 | 192.168.1.1 | 255.255.255.0 | N/A |
| RTR-A | G0/0/1 | 192.168.2.1 | 255.255.255.0 | N/A |
| SW-1 | VLAN 1 | 192.168.1.254 | 255.255.255.0 | 192.168.1.1 |
| PC | NIC | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| Laptop | NIC | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 |
| Remote PC | NIC | 192.168.2.10 | 255.255.255.0 | 192.168.2.1 |

## Router Configuration

The router was configured to improve security and restrict unauthorized access.

### Basic Security

- Hostname changed to **RTR-A**
- Disabled DNS lookup
- Required a minimum password length of 10 characters
- Configured an encrypted enable secret
- Enabled password encryption
- Configured a MOTD warning banner

### Console Security

- Console password configured
- Console login required
- Session timeout set to 7 minutes

### Router SSH configuration

- Local administrator account created
- Domain name configured as **security.com**
- Generated 1024-bit RSA keys
- Enabled SSH version 2
- Restricted VTY access to SSH only
- Configured VTY authentication using the local user database

### Login Protection

Configured login blocking to temporarily prevent brute force login attempts after multiple failed logins.

## Switch Configuration

The switch was configured for secure remote management.

### Management Interface

- Hostname changed to **SW-1**
- VLAN 1 assigned management IP address
- Default gateway configured
- Password encryption enabled
- Enable secret configured
- MOTD banner configured

### Switch SSH Configuration

- Local administrator account created
- Domain name configured
- Generated RSA keys
- Enabled SSH version 2
- Restricted VTY access to SSH only

### Port Security

Unused switch interfaces were administratively shut down while keeping only the required active interfaces enabled.

Active interfaces:

- GigabitEthernet0/1 (Router)
- FastEthernet0/2 (PC)
- FastEthernet0/10 (Laptop)

All remaining unused access ports were disabled.

## Verification

The router and switch were verified using several IOS commands including:

```text
show ip interface brief
show interfaces status
show running-config
show cdp neighbors
```

These commands confirmed:

- Interface status
- SSH configuration
- User accounts
- Switch management configuration
- Active and disabled interfaces

## Results

The completed lab successfully met every assessment requirement.

![Completed Packet Tracer assessment results.]({{ site.baseurl }}/assets/images/lab21/results.png)

Final score:

- **98/98 Points**
- **52/52 Assessment Items**
- **100% Complete**

## Final Topology

The completed topology included a secured router, two switches, three end devices, and secure management connectivity.

![Completed Packet Tracer topology.]({{ site.baseurl }}/assets/images/lab21/topology.png)

## Lessons Learned

This lab reinforced several important security concepts for Cisco IOS devices.

- SSH should always be used instead of Telnet for remote management.
- Password encryption helps protect locally stored credentials.
- Limiting VTY access to authenticated local users improves security.
- Disabling unused switch ports reduces the attack surface.
- Login blocking can slow automated password attacks.
- Verifying active interfaces before shutting down unused ports is essential to avoid disconnecting production devices.

## Conclusion

This lab demonstrated how multiple Cisco IOS security features work together to secure both routers and switches. Proper password management, SSH, restricted management access, encrypted credentials, and disabling unused interfaces are all important best practices that help protect network infrastructure from unauthorized access.
