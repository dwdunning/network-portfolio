---
layout: post
title: "Packet Tracer Lab 18 - Configure Basic Wireless Security"
date: 2026-08-05
categories: [Networking, Cisco, Packet Tracer, Module-14-DHCP-and-Wireless]
tags: [networking, cisco, packet tracer, wireless, wifi, wpa2, security, home-router]
---

## Overview

This lab focused on improving wireless network security by configuring **WPA2 Personal** on a home wireless router. Initially, the wireless network was unsecured, allowing any nearby client to connect without authentication. By enabling WPA2 Personal with AES encryption and reconnecting the wireless client using a pre-shared key, the network was protected from unauthorized access.

## Objectives

- Verify wireless connectivity before making changes.
- Access the wireless router's web-based management interface.
- Configure WPA2 Personal security on the 2.4 GHz wireless network.
- Configure a secure pre-shared key.
- Reconnect the wireless client using the new credentials.
- Verify successful completion of the activity.

## Network Topology

The provided Packet Tracer activity consisted of a laptop connected wirelessly to a home wireless router with Internet connectivity to a simulated web server.

![Packet Tracer topology.]({{ site.baseurl }}/assets/images/lab18/topology.png)

## Initial Router Configuration

Before enabling wireless security, I verified the router's basic network configuration. The router was configured with DHCP enabled and provided addressing information to wireless clients.

**Key settings included:**

- Router IP Address: **192.168.1.1**
- DHCP Enabled
- Client Address Range: **192.168.1.100 - 192.168.1.149**
- DNS Server: **198.51.100.2**

![Router configuration.]({{ site.baseurl }}/assets/images/lab18/router-config.png)

## Configuring WPA2 Personal

Using the router's web interface, I navigated to:

**Wireless → Wireless Security**

The following configuration was applied:

| Setting | Value |
|---------|-------|
| 2.4 GHz Security Mode | WPA2 Personal |
| Encryption | AES |
| Passphrase | Network123 |
| 5 GHz Networks | Disabled |

After configuring these settings, I saved the changes to apply the new wireless security configuration.

![Wireless security configuration.]({{ site.baseurl }}/assets/images/lab18/security.png)

## Reconnecting the Wireless Client

After enabling WPA2 Personal, the laptop disconnected from the previously open wireless network. Using the **PC Wireless** utility, I reconnected to the **Academy** wireless network by entering the configured pre-shared key:

```text
Network123
```

The wireless adapter successfully authenticated with the access point using WPA2-PSK.

![Wireless client successfully connected.]({{ site.baseurl }}/assets/images/lab18/client.png)

## Verification

After reconnecting the client with the new wireless security settings, Packet Tracer successfully validated the activity and confirmed completion.

![Activity completed successfully.]({{ site.baseurl }}/assets/images/lab18/results.png)

## Key Takeaways

This lab demonstrated how quickly an unsecured wireless network can be protected using WPA2 Personal security. Even in a small home or small business environment, enabling strong wireless encryption significantly improves network security while requiring only minimal configuration.

The activity also reinforced the importance of:

- Securing wireless access with WPA2 Personal.
- Using AES encryption for wireless communications.
- Configuring a strong pre-shared key.
- Reconnecting wireless clients after security changes.
- Verifying successful client authentication after configuration changes.

## Reflection

This lab highlighted one of the simplest yet most important security improvements that can be made to a wireless network. While configuring WPA2 Personal required only a few settings, the result was a significant increase in protection against unauthorized access.

Completing the activity also reinforced how wireless clients must be reauthenticated after security settings change, an important concept when deploying or modifying wireless networks in real-world environments.
