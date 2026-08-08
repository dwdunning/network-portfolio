---
layout: post
title: "Packet Tracer Lab 32 - Use the Cisco IOS Show Commands"
date: 2026-08-08
categories: [Networking, Cisco, Packet Tracer, Module-17-Basic-Network-Administration]
tags: [networking, cisco, packet tracer, ios, show commands, router, troubleshooting, network administration]
---

## Overview

In this lab, I used Cisco IOS `show` commands to examine the status and configuration of an ISP router. The activity demonstrated how `show` commands can be used to gather information about ARP entries, flash storage, routing tables, interfaces, IOS versions, protocols, licensing, and the running configuration.

The lab also demonstrated an important difference between **User EXEC mode** and **Privileged EXEC mode**. Some diagnostic information is available from the `ISPRouter>` prompt, while more detailed configuration information requires the `ISPRouter#` privileged prompt.

## Objectives

- Connect to the Cisco 4321 ISP router through a terminal session.
- Explore Cisco IOS `show` commands.
- Examine the router's ARP table.
- Identify the IOS image stored in flash.
- Examine the IPv4 routing table.
- Determine interface status.
- Identify enabled technology packages and protocols.
- Compare commands available in User EXEC and Privileged EXEC modes.
- View the router's running configuration.

## Part 1 - Connect to the ISP Router

I opened **ISP PC** in Packet Tracer and selected:

**Desktop > Terminal**

After accepting the default terminal configuration, I reached the router's User EXEC prompt:

```text
ISPRouter>
```

The `>` prompt indicates **User EXEC mode**, which provides access to basic monitoring and diagnostic commands.

## Part 2 - Explore the Show Commands

### User EXEC Mode

I started by entering:

```text
show ?
```

This displayed the `show` commands available from User EXEC mode.

Some of the available commands included:

```text
show arp
show cdp
show clock
show flash
show history
show interfaces
show ip
show ipv6
show protocols
show version
```

These commands allow an administrator to examine many aspects of the router without modifying its configuration.

## Examining the ARP Table

I entered:

```text
show arp
```

The router returned the following ARP entry:

| Field | Value |
|---|---|
| IP Address | `209.165.201.1` |
| MAC Address | `0001.96CD.2501` |
| Type | ARPA |
| Interface | GigabitEthernet0/0/0 |

The ARP table associates IPv4 addresses with Layer 2 MAC addresses.

## Examining Flash Storage

Next, I entered:

```text
show flash
```

The router displayed the contents of its flash storage.

The IOS image was:

```text
isr4300-universalk9.03.16.05.S.155-3.S5-ext.SPA.bin
```

The flash directory also contained:

```text
sigdef-category.xml
sigdef-default.xml
```

The `.bin` file is the Cisco IOS image used by the router.

## Examining the Routing Table

I entered:

```text
show ip route
```

The routing table contained two routes:

```text
C    209.165.201.0/27 is directly connected, GigabitEthernet0/0/0
L    209.165.201.1/32 is directly connected, GigabitEthernet0/0/0
```

The router also reported:

```text
Gateway of last resort is not set
```

There were **2 routes** in the routing table.

The route codes indicate:

- `C` - Connected network
- `L` - Local interface address

The connected route represents the network attached to `GigabitEthernet0/0/0`, while the local route represents the router's own IPv4 address.

## Examining Router Interfaces

I entered:

```text
show interfaces
```

The command displayed detailed information about each interface.

The important interface states were:

| Interface | Status | Protocol |
|---|---|---|
| GigabitEthernet0/0/0 | Up | Up |
| GigabitEthernet0/0/1 | Administratively down | Down |
| Serial0/1/0 | Down | Down |
| Serial0/1/1 | Down | Down |

The only interface that was fully operational was:

```text
GigabitEthernet0/0/0
```

The router reported:

```text
GigabitEthernet0/0/0 is up, line protocol is up (connected)
```

This shows that both the physical interface and its line protocol were operational.

`GigabitEthernet0/0/1` was **administratively down**, indicating that the interface had been disabled through the router configuration.

## Examining IP Interface Information

I entered:

```text
show ip interface
```

The command confirmed:

```text
GigabitEthernet0/0/0 is up, line protocol is up (connected)
Internet address is 209.165.201.1/27
```

Therefore, the connected interface was:

```text
GigabitEthernet0/0/0
```

The command also showed the following interface information:

| Interface | IP Address | State |
|---|---|---|
| GigabitEthernet0/0/0 | `209.165.201.1/27` | Up / Up |
| GigabitEthernet0/0/1 | None | Administratively Down / Down |
| Serial0/1/0 | `209.165.200.226/27` | Down / Down |
| Serial0/1/1 | None | Down / Down |
| VLAN 1 | None | Administratively Down / Down |

## Examining the IOS Version

I entered:

```text
show version
```

The router reported:

```text
Cisco IOS XE Software, Version 03.16.05.S
Cisco IOS Software, Version 15.5(3)S5
```

The system image was:

```text
bootflash:/isr4300-universalk9.03.16.05.S.155-3.S5-ext.SPA.bin
```

The router was identified as a:

```text
Cisco ISR4321/K9
```

The enabled technology packages were:

```text
securityk9
ipbasek9
```

Both were listed as **Permanent** licenses.

The `appxk9` and `uck9` technology packages were not enabled.

## Examining Enabled Protocols

I entered:

```text
show protocols
```

The output reported:

```text
Global values:
  Internet Protocol routing is enabled
```

This confirmed that **Internet Protocol routing** was enabled on the router.

The command also provided a concise summary of the router's interface states and IP addresses.

## Attempting to View the Running Configuration

While in User EXEC mode, I attempted:

```text
ISPRouter>show running-config
```

The router returned:

```text
               ^
% Invalid input detected at '^' marker.
```

This demonstrated that the running configuration could not be accessed from User EXEC mode.

The command requires a higher privilege level.

## Privileged EXEC Mode

I entered:

```text
enable
```

The prompt changed from:

```text
ISPRouter>
```

to:

```text
ISPRouter#
```

The `#` indicates **Privileged EXEC mode**.

I then entered:

```text
show ?
```

Additional commands were available in this mode, including:

```text
show access-lists
show debugging
show dhcp
show license
show logging
show mac-address-table
show processes
show snmp
show spanning-tree
show startup-config
show tech-support
```

Privileged EXEC mode therefore provides access to additional administrative and diagnostic information that is unavailable from User EXEC mode.

## Viewing the Running Configuration

From Privileged EXEC mode, I entered:

```text
show running-config
```

This time the command successfully displayed the router's active configuration.

Some of the important configuration information included:

```text
hostname ISPRouter
```

The active Gigabit Ethernet interface was configured as:

```text
interface GigabitEthernet0/0/0
 ip address 209.165.201.1 255.255.255.224
 duplex auto
 speed auto
```

The second Gigabit Ethernet interface was shut down:

```text
interface GigabitEthernet0/0/1
 no ip address
 duplex auto
 speed auto
 shutdown
```

The first serial interface was configured as:

```text
interface Serial0/1/0
 ip address 209.165.200.226 255.255.255.224
 clock rate 2000000
```

The second serial interface had no IP address:

```text
interface Serial0/1/1
 no ip address
 clock rate 2000000
```

VLAN 1 was also shut down:

```text
interface Vlan1
 no ip address
 shutdown
```

This demonstrated the privilege difference clearly. The command failed at the `ISPRouter>` User EXEC prompt but successfully displayed the active configuration at the `ISPRouter#` Privileged EXEC prompt.

## User EXEC vs. Privileged EXEC

| Feature | User EXEC | Privileged EXEC |
|---|---|---|
| Prompt | `ISPRouter>` | `ISPRouter#` |
| Basic show commands | Yes | Yes |
| Interface information | Yes | Yes |
| Routing information | Yes | Yes |
| ARP information | Yes | Yes |
| Running configuration | No | Yes |
| Startup configuration | No | Yes |
| Additional administrative commands | Limited | Yes |

This separation helps protect router configuration information and administrative functions from users who only require basic monitoring access.

## Results

The Packet Tracer activity was completed successfully.

![Packet Tracer Lab 32 completion results](/assets/images/lab32/results.png)![Packet Tracer Lab 32 completion results]({{ '/assets/images/lab32/results.png' | relative_url }})

Packet Tracer reported:

```text
Congratulations Guest! You completed the activity.
```

The assessment item also showed the network status as **Correct**.

## What I Learned

This lab gave me practical experience using Cisco IOS `show` commands to inspect a router without changing its configuration. I used different commands to examine the ARP table, flash storage, routing table, interfaces, IP configuration, IOS version, licensing information, and enabled protocols.

One of the most important parts of the lab was seeing the difference between **User EXEC mode** and **Privileged EXEC mode**. Commands such as `show interfaces`, `show ip route`, and `show version` were available from User EXEC mode, but `show running-config` was not. After entering `enable`, the prompt changed to `ISPRouter#`, and the running configuration became accessible.

I also saw how several `show` commands can be used together during network administration and troubleshooting. For example, `show interfaces` can identify whether an interface and its line protocol are operational, while `show ip route` verifies which networks the router currently knows how to reach. These commands provide administrators with a systematic way to examine device state before making configuration changes.

## Conclusion

Module 17 Lab 32 demonstrated how Cisco IOS `show` commands provide visibility into the current state of a network device. By examining information from multiple commands, I was able to identify the router's active interface, IP addresses, routes, IOS image, technology packages, protocol status, and current configuration.

The lab also reinforced the purpose of Cisco IOS privilege levels. User EXEC mode provides useful monitoring capabilities, while Privileged EXEC mode provides access to more detailed administrative information. Understanding these commands and privilege levels is an important part of basic network administration and provides a foundation for troubleshooting Cisco network devices.
