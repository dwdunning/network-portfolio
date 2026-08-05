---
layout: post
title: "Packet Tracer Lab 20 - Troubleshoot a Wireless Connection"
date: 2026-08-05
categories: [Networking, Cisco, Packet Tracer, Module-14-Network-Communications]
tags: [networking, cisco, packet tracer, wireless, troubleshooting, wpa2, wifi]
---

## Objectives

In this lab, I practiced troubleshooting a wireless connectivity issue by:

- Identifying a client unable to connect to the network
- Examining IP addressing information
- Verifying wireless client configuration
- Inspecting wireless router settings
- Correcting the wireless security configuration
- Verifying successful connectivity

## Network Topology

The lab consists of a wireless router connected to the Internet, two wired PCs, and two wireless PCs.

### Initial Topology

![Initial network topology.]({{ site.baseurl }}/assets/images/lab20/topology.png)

## Part 1 - Verify Connectivity

Each PC was tested by opening the web browser and attempting to access`**www.cisco.pka**.`

PC3 and PC4 (wired) connected successfully, while **PC1** could not access the web server.

## Part 2 - Examine the IP Configuration

Running `ipconfig /all` on PC1 revealed that the wireless client had not obtained a valid IPv4 configuration.

![PC1 IP configuration showing an APIPA address.]({{ site.baseurl }}/assets/images/lab20/PC1-no-IP.png)

### PC1 IP Configuration

| Setting | Value |
| --- | --- |
| IPv4 Address | 169.254.122.96 |
| Subnet Mask | 255.255.0.0 |
| Default Gateway | None |
| DHCP Server | None |
| DNS Server | None |

The **169.254.x.x** APIPA address indicated that the client was not successfully communicating on the wireless network.

## Part 3 - Examine the Wireless Client

Opening **Desktop → PC Wireless** showed that the wireless client detected the **Academy** wireless network.

![Wireless client detecting the Academy SSID.]({{ site.baseurl }}/assets/images/lab20/wifi.png)

The wireless adapter was functioning and could see the access point, but connecting required the correct WPA2 pre-shared key.

## Part 4 - Examine the Wireless Router

Using a wired PC, I logged into the wireless router using the default credentials.

| Setting | Value |
| --- | --- |
| Username | admin |
| Password | admin |

The wireless configuration was verified.

| Setting | Value |
| --- | --- |
| SSID | Academy |
| Security Mode | WPA2-PSK |
| Passphrase | cisco123 |
| DHCP | Enabled |

The client was attempting to connect without the proper WPA2 passphrase.

## Part 5 - Correct the Wireless Configuration

On PC1, I:

1. Selected the **Academy** wireless network.
2. Entered the correct WPA2 pre-shared key (`cisco123`).
3. Connected to the wireless network.
4. Verified successful access to`**www.cisco.pka**.`

After authentication, PC1 successfully joined the wireless network.

### Final Topology

![Completed network topology with all devices connected.]({{ site.baseurl }}/assets/images/lab20/topologyfixed.png)

## Verification

Packet Tracer confirmed that all required objectives had been completed successfully.

![Successful completion of the Packet Tracer activity.]({{ site.baseurl }}/assets/images/lab20/results.png)

## Reflection

This lab demonstrated a structured approach to troubleshooting wireless connectivity problems. Instead of assuming the issue was with the router, I verified connectivity, examined the client's IP configuration, confirmed the wireless network settings, and compared them with the router's configuration. The client was able to detect the wireless network, but it could not authenticate until the correct WPA2 pre-shared key was entered. Once the security settings matched, the client successfully connected and regained network access.

## Key Takeaways

- APIPA addresses (169.254.x.x) indicate that a client does not have a valid IPv4 configuration.
- Always verify connectivity before making configuration changes.
- Compare the client's wireless configuration with the access point's settings.
- Matching the correct SSID alone is not enough when WPA2 security is enabled.
- Verifying authentication settings is an essential step when troubleshooting wireless networks.
  