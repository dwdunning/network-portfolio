---
layout: post
title: "Packet Tracer Lab 25 - Use the ping Command"
date: 2026-08-06
categories: [Networking, Cisco, Packet Tracer, Module-15-Routing-Network-Assessment]
tags: [networking, cisco, packet tracer, ping, dns, troubleshooting]
---

## Overview

In this lab, I used the `ping` command to troubleshoot why one PC could not access a website. By comparing the behavior of a working PC with the failing PC, I determined that the problem was not basic network connectivity, but an incorrect DNS server configuration. After correcting the DNS server address, the PC was able to resolve the hostname and successfully reach the web server.

## Objectives

- Verify web connectivity from multiple PCs.
- Use `ping` to diagnose connectivity problems.
- Compare successful and unsuccessful name resolution.
- Examine network settings with `ipconfig /all`.
- Identify and correct a DNS configuration error.
- Verify successful connectivity after the repair.

## Network Topology

![Network topology for Lab 25.]({{ site.baseurl }}/assets/images/lab25/topology.png)

## Step 1 - Verify Web Connectivity

Each PC attempted to browse to `**www.cisco.pka**`.

| Device | Result |
| --- | --- |
| PC1 | ✅ Connected |
| PC2 | ❌ Failed |
| PC3 | ✅ Connected |
| PC4 | ✅ Connected |

Since only PC2 experienced the problem, the issue was likely isolated to that workstation.

## Step 2 - Test Name Resolution

Running the following command on PC2:

```text
ping www.cisco.pka
```

produced the error:

```text
Ping request could not find host www.cisco.pka.
```

This indicated that the PC could not resolve the hostname into an IP address.

![PC2 failing to resolve the hostname.]({{ site.baseurl }}/assets/images/lab25/badDNS.png)

## Step 3 - Compare Network Configuration

Using the command:

```text
ipconfig /all
```

I compared the configuration of a working PC with PC2.

The working PC showed the correct DNS server configuration.

![Working PC network configuration.]({{ site.baseurl }}/assets/images/lab25/good-ip.png)

The comparison revealed a typo in the DNS server address.

| Setting | Working PC | PC2 |
| --- | --- | --- |
| DNS Server | 192.15.2.5 | 191.15.2.5 |

Although only one digit was incorrect, it prevented DNS from translating `**www.cisco.pka**` into its IP address.

## Step 4 - Correct the Configuration

On PC2:

1. Opened **Desktop → IP Configuration**.
2. Corrected the DNS Server address from:

```text
191.15.2.5
```

to:

```text
192.15.2.5
```

After saving the change, PC2 successfully loaded `**www.cisco.pka**` in the web browser.

## Verification

The Packet Tracer assessment completed successfully.

![Packet Tracer completion screen.]({{ site.baseurl }}/assets/images/lab25/completion.png)

## Troubleshooting Process

1. Verified which PC could not access the website.
2. Used `ping` to test hostname resolution.
3. Recognized that "could not find host" indicated a DNS issue rather than a routing problem.
4. Compared the output of `ipconfig /all` between a working PC and the failing PC.
5. Identified the incorrect DNS server address.
6. Corrected the DNS configuration.
7. Confirmed successful web access and completed the assessment.

## Key Takeaways

- The `ping` command is useful for distinguishing between network connectivity problems and DNS resolution failures.
- The message **"Ping request could not find host"** typically indicates a DNS configuration issue.
- Comparing the configuration of a working system with a failing system is one of the fastest troubleshooting methods.
- A single incorrect digit in a DNS server address is enough to prevent hostname resolution while leaving the rest of the network configuration functional.
  