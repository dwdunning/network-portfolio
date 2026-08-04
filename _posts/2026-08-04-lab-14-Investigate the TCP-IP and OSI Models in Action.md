---
layout: post
title: "Packet Tracer Lab 14 – Investigate the TCP/IP and OSI Models in Action"
date: 2026-08-04
categories: [Networking, Cisco, Packet Tracer, Module-13-Network-Addressing]
tags: [networking, cisco, packet tracer, tcp/ip, osi, dns, http, tcp, arp, simulation]
---

## Overview

This lab explored how the TCP/IP protocol suite maps to the OSI model using Cisco Packet Tracer's **Simulation Mode**. By capturing and examining protocol data units (PDUs), I observed how HTTP, DNS, TCP, and ARP traffic is encapsulated, transmitted, and processed between a web client and web server.

The exercise provided a visual understanding of how data moves through each OSI layer and how multiple protocols work together to establish communication.

---

## Objectives

- Examine HTTP web traffic in Simulation Mode.
- Observe encapsulation through the TCP/IP and OSI models.
- Investigate DNS name resolution.
- Observe TCP session establishment and termination.
- Identify the protocols involved in a simple web request.

---

## Network Topology

![Network topology.]({{ site.baseurl }}/assets/images/lab14/topology.png)

The network consists of a single Web Client connected directly to a Web Server.

---

## Part 1 – Examine HTTP Web Traffic

### Launching the HTTP Request

After switching Packet Tracer to **Simulation Mode**, I filtered for HTTP traffic, opened the Web Browser on the client, and browsed to:

```text
http://www.osi.local
```

After stepping through the simulation, the browser successfully displayed the web page.

![Web Server home page displayed.]({{ site.baseurl }}/assets/images/lab14/jump4.png)

### Observation

The HTTP request completed successfully and the Web Server returned the requested web page.

---

## Examining the Initial HTTP Request

The first HTTP PDU originated at the Web Client.

![Initial HTTP request at the Web Client.]({{ site.baseurl }}/assets/images/lab14/PDU-at-client.png)

### Observations

| Layer   |                               Observation                          |
|---------|--------------------------------------------------------------------|
| Layer 7 |        The HTTP client sends an HTTP request to the server.        |
| Layer 4 |                  Destination Port = **80**                         |
| Layer 3 |              Destination IP = **192.168.1.254**                    |
| Layer 2 | Ethernet II header containing source and destination MAC addresses |

---

## Outbound HTTP PDU Details

Viewing the outbound packet reveals how each protocol contributes to encapsulation.

![Outbound HTTP PDU.]({{ site.baseurl }}/assets/images/lab14/outbound-PDU-details.png)

### Key Findings

| Section  | Information                                                  |
|----------|--------------------------------------------------------------|
|    IP    | Source IP: **192.168.1.1** Destination IP: **192.168.1.254** |
|    TCP   |        Source Port **1025**, Destination Port **80**         |
|   HTTP   |                    Host: `www.osi.local`                     |

The HTTP **Host** header belongs to the **Application Layer (Layer 7)**.

![HTTP Host field.]({{ site.baseurl }}/assets/images/lab14/host.png)

---

## Server Receives the Request

Packet Tracer then showed the Web Server processing the incoming HTTP request.

![PDU at the Web Server.]({{ site.baseurl }}/assets/images/lab14/PDU-at-server.png)

Comparing the inbound and outbound layers demonstrates how the server prepares its response.

### Differences Between Inbound and Outbound Layers

- Source and destination IP addresses are reversed.
- Source and destination MAC addresses are reversed.
- TCP source and destination ports are reversed.
- The inbound request becomes an outbound HTTP response.

---

## HTTP Response

The outbound PDU from the Web Server contained the HTTP response.

![Outbound PDU at the Web Server.]({{ site.baseurl }}/assets/images/lab14/outbound-PDU-at-server-details.png)

The HTTP response included:

- Connection: close
- Content-Length: 170

---

## Final HTTP Event

The final HTTP event occurred when the Web Client received the response.

![Final HTTP event at the client.]({{ site.baseurl }}/assets/images/lab14/PDU-at-client-final.png)

Only two tabs were available:

- OSI Model
- Inbound PDU Details

Since the packet had reached its destination, there was no outbound packet to display.

---

## Part 2 – Display Elements of the TCP/IP Protocol Suite

After enabling all event types and restarting the simulation, additional protocols became visible.

![Simulation events.]({{ site.baseurl }}/assets/images/lab14/new-events.png)

### Protocols Observed

- ARP
- DNS
- TCP
- HTTP

This illustrates that a simple web request depends on several protocols working together.

---

## DNS Query

The first DNS event resolved the server's hostname.

![DNS query.]({{ site.baseurl }}/assets/images/lab14/DNS.png)

| Field |      Value        |
|-------|-------------------|
| Name  | `www.osi.local`   |

---

## DNS Response

The DNS response returned the IP address associated with the hostname.

![DNS response.]({{ site.baseurl }}/assets/images/lab14/DNS-at-client.png)

### DNS Answer

|   Field   |      Value        |
|-----------|-------------------|
| Hostname  |  `www.osi.local`  |
| IP Address| **192.168.1.254** |

---

## TCP Connection Establishment

Examining the TCP events showed the completion of the TCP three-way handshake.

![TCP connection establishment.]({{ site.baseurl }}/assets/images/lab14/tcp.png)

The final two TCP steps indicated:

1. The TCP connection is successful.
2. The connection state is set to **ESTABLISHED**.

This confirms reliable communication has been established before any HTTP data is exchanged.

---

## Challenge Questions

### Which port is the Web Server listening on for HTTP?

```text
TCP Port 80
```

---

### Which port is the Web Server listening on for DNS?

```text
UDP Port 53
```

---

## What I Learned

This lab clearly demonstrated how multiple protocols cooperate during a normal web request.

The communication sequence followed this general process:

1. ARP resolves the destination MAC address.
2. DNS resolves the hostname into an IP address.
3. TCP establishes a reliable connection.
4. HTTP transfers the web page.
5. TCP gracefully closes the session.

Viewing each protocol through Packet Tracer's Simulation Mode made it much easier to understand how the TCP/IP protocol suite maps onto the OSI model and how encapsulation occurs at each networking layer.

---

## Skills Demonstrated

- Using Packet Tracer Simulation Mode
- Following packet encapsulation through the OSI model
- Inspecting HTTP PDUs
- Examining DNS queries and responses
- Understanding TCP session establishment
- Identifying protocol interactions within the TCP/IP suite
  