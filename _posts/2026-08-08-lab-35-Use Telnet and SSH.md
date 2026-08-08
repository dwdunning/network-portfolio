---
layout: post
title: "Packet Tracer Lab 35 - Use Telnet and SSH"
date: 2026-08-08
categories: [Networking, Cisco, Packet Tracer, Module-17-Basic-Network-Administration]
tags: [networking, cisco, packet tracer, telnet, ssh, remote-access, network-security, dhcp, network-administration]
---

## Overview

In this lab, I used Cisco Packet Tracer to establish remote connectivity to a router using Telnet and SSH. The purpose of the activity was to verify basic IP connectivity, attempt an insecure Telnet connection, and then use SSH to securely access the remote HQ router.

The activity demonstrated an important network administration concept: basic network connectivity does not necessarily mean that every remote management protocol is permitted. Although the HQ router was reachable across the network, it was configured to reject Telnet and require SSH for remote administrative access.

![Lab 35 network topology]({{ '/assets/images/lab35/topology.png' | relative_url }})

## Addressing Table

| Device | Interface | IP Address | Subnet Mask |
| --- | --- | --- | --- |
| HQ | G0/0/1 | 64.100.1.1 | 255.255.255.0 |
| PC0 | NIC | DHCP | DHCP |
| PC1 | NIC | DHCP | DHCP |

## Objectives

The objectives of this activity were to:

- Verify that a PC received its network configuration through DHCP.
- Verify IP connectivity between the PC and the remote HQ router.
- Attempt remote access using Telnet.
- Determine why Telnet access was unsuccessful.
- Establish a secure remote connection using SSH.
- Verify successful access to the HQ router.

## Part 1 - Verify Connectivity

### Verify the PC IP Configuration

I opened the Command Prompt on PC0 and checked its network configuration with:

```text
ipconfig /all
```

The PC had successfully received its IPv4 configuration from DHCP.

```text
IPv4 Address:    192.168.1.11
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.1.1
DHCP Server:     192.168.1.1
```

**Question: What command did you use to verify the IP address from DHCP?**

I used:

```text
ipconfig /all
```

This provided the assigned IPv4 address as well as additional information confirming that the address was obtained through DHCP.

### Verify Connectivity to HQ

Next, I tested connectivity from PC0 to the HQ router at `64.100.1.1`.

```text
ping 64.100.1.1
```

The first attempt experienced an initial timeout while the network resolved the path, but subsequent replies were successful. I repeated the test and received four successful replies:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This confirmed that PC0 had end-to-end IP connectivity to the HQ router before attempting remote management.

## Part 2 - Access a Remote Device

### Attempt Telnet Access

I first attempted to remotely access HQ using Telnet:

```text
telnet 64.100.1.1
```

The connection reached the router, but HQ immediately terminated the Telnet session:

```text
Trying 64.100.1.1 ...Open

[Connection to 64.100.1.1 closed by foreign host]
```

**Question: Were you successful? What was the output?**

No. The Telnet connection was not successful. The connection reached `64.100.1.1`, but the remote host immediately closed it.

This behavior was expected because HQ was configured not to permit insecure Telnet remote access.

### Connect Using SSH

Because Telnet was disabled, I connected to the HQ router using SSH instead.

The SSH command specified the `admin` username and the IP address of the HQ router:

```text
ssh -l admin 64.100.1.1
```

When prompted, I entered the configured password:

```text
class
```

The authentication succeeded and the router displayed:

```text
HQ#
```

**Question: What is the prompt after accessing the router successfully via SSH?**

```text
HQ#
```

The `HQ#` prompt confirmed that I had successfully established an authenticated SSH session with the router.

![PC0 connectivity, Telnet rejection, and successful SSH connection]({{ '/assets/images/lab35/lab.png' | relative_url }})

## Telnet vs. SSH

This activity demonstrated an important difference between Telnet and SSH.

Telnet provides remote command-line access, but the protocol does not securely protect transmitted credentials and session data. Because of this, Telnet should generally not be used for administrative access across an untrusted network.

SSH also provides remote command-line access, but it encrypts the session. This protects authentication credentials and administrative traffic while they travel across the network.

The HQ router demonstrated this security practice by remaining reachable through IP while rejecting Telnet and accepting authenticated SSH access.

## Troubleshooting and Verification

A useful troubleshooting sequence in this lab was to verify connectivity before troubleshooting the remote access protocol itself.

The process was:

1. Verify that PC0 received a valid DHCP configuration.
2. Verify the PC's IP address, subnet mask, default gateway, and DHCP server.
3. Ping `64.100.1.1` to establish that the remote router was reachable.
4. Attempt a Telnet connection.
5. Observe that the remote host deliberately closed the Telnet connection.
6. Attempt SSH using the configured username.
7. Authenticate with the configured password.
8. Verify the `HQ#` router prompt.

Because the ping succeeded before the Telnet test, the failed Telnet connection could not simply be attributed to a loss of IP connectivity. The successful SSH session further confirmed that remote management was functioning and that the failure was specific to Telnet.

## Results

The Packet Tracer activity was completed successfully.

![Packet Tracer Lab 35 completion results]({{ '/assets/images/lab35/results.png' | relative_url }})

The completed tests demonstrated that:

- PC0 successfully obtained its network configuration through DHCP.
- PC0 could communicate with the HQ router.
- HQ was reachable at `64.100.1.1`.
- Telnet access was rejected.
- SSH access was permitted.
- The `admin` account successfully authenticated through SSH.
- The `HQ#` prompt confirmed successful remote router access.

## Reflection

This lab demonstrated why verifying basic connectivity should come before troubleshooting an application or network service. A failed Telnet connection did not mean that the router was unreachable. The successful ping showed that the underlying network path was working, which narrowed the problem to the remote access service.

The comparison between Telnet and SSH also reinforced why secure management protocols are important in network administration. Disabling Telnet while allowing SSH reduces the risk of exposing administrative credentials and management traffic. In a production network, SSH should be preferred when remotely managing routers, switches, and other network infrastructure.

The lab also reinforced a useful troubleshooting approach: test the network layer first, observe the exact response from the service, and then determine whether the failure is caused by connectivity, configuration, authentication, or an intentionally disabled protocol.
