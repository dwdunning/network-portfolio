---
layout: post
title: "Packet Tracer Lab 33 - Connect to a Web Server"
date: 2026-08-08
categories: [Networking, Cisco, Packet Tracer, Module-17-Basic-Network-Administration]
tags: [networking, cisco, packet tracer, web server, ip addressing, ping, connectivity, network administration]
---

## Overview

In this lab, I used Cisco Packet Tracer to observe how a client communicates with a web server across the Internet using IP addresses. The activity demonstrated how an IP address can be used to verify connectivity to a remote server and then access a web service directly through a web browser.

The client PC, PC0, was configured with the IP address `192.168.1.100`, while the LearnIP web server used the IP address `172.33.100.50`.

## Objectives

- Verify connectivity between a client and a remote web server.
- Use an IP address to communicate with a server across the Internet.
- Connect to a web server using a web client.
- Observe the relationship between IP connectivity and application-layer web access.

## Network Topology

The topology consisted of PC0 connected across a simulated Internet connection to the LearnIP web server.

- **PC0:** `192.168.1.100`
- **LearnIP Web Server:** `172.33.100.50`
- **Website:** `www.learnIP.com`

![Lab 33 Network Topology]({{ '/assets/images/lab33/topology.png' | relative_url }})

## Part 1 - Verify Connectivity to the Web Server

I opened the command prompt on PC0 and tested connectivity to the LearnIP web server using its IP address:

```text
ping 172.33.100.50
```

Successful ICMP echo replies verified that PC0 could communicate with the remote server across the network.

The lab noted that an initial ping may time out while devices initialize and Address Resolution Protocol (ARP) information is learned. Once connectivity was established, replies from the server confirmed that the network path was operational.

This test demonstrated an important troubleshooting principle: verifying basic IP connectivity before attempting to troubleshoot a higher-layer service.

## Part 2 - Connect to the Web Server

After verifying IP connectivity, I opened the Web Browser application on PC0 and entered the server's IP address directly:

```text
172.33.100.50
```

The browser successfully connected to the server and displayed the Learn IP website.

![PC0 Connected to the LearnIP Web Server]({{ '/assets/images/lab33/learnIP.png' | relative_url }})

The page displayed the message:

> **Welcome to the Learn IP Web Site**

It also explained that the website was reachable because the IP address of the web server was known and the connecting PC had a web client running on the device.

This demonstrated that a client can access a web server directly through its IP address without entering a domain name.

## Results

The Packet Tracer activity completed successfully. The assessment window reported:

> **Congratulations Guest! You completed the activity.**

The activity contained no scored configuration items, so Packet Tracer displayed a score of `0/0`. The Network assessment item was marked **Correct**, confirming successful completion of the activity.

![Packet Tracer Lab 33 Activity Results]({{ '/assets/images/lab33/results.png' | relative_url }})

## What I Learned

This lab reinforced the relationship between basic IP connectivity and application-layer services. Before a client can communicate with a web server, the underlying network must provide a working path between the source and destination IP addresses.

Using `ping` provided a quick method of confirming that PC0 could reach the server before testing the web service itself. Once connectivity was verified, accessing `172.33.100.50` through the browser demonstrated that HTTP communication could take place over that established network path.

The lab also illustrated the distinction between an IP address and a domain name. Although the server was associated with `www.learnIP.com`, knowing the server's IP address was sufficient to connect directly to its web service.

## Conclusion

Lab 33 demonstrated a basic but essential network administration troubleshooting workflow: verify IP connectivity first, then test the required application service. PC0 successfully communicated with the LearnIP server at `172.33.100.50` and accessed its website through the built-in Packet Tracer web client.

This activity provided a practical example of how addressing, connectivity testing, and application services work together when a client accesses a remote resource across a network.
