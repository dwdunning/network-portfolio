---
layout: post
title: "Packet Tracer Lab 43 - TCP and UDP Communications"
date: 2026-08-11
categories: [Networking, Cisco, Packet Tracer, Module-18-Network-Monitoring]
tags: [networking, cisco, packet tracer, tcp, udp, http, ftp, dns, smtp, pop3, network monitoring]
---

## Overview

In this lab, I used Cisco Packet Tracer's Simulation Mode to generate, monitor, filter, and inspect network traffic between several client devices and a MultiServer. The activity demonstrated how application protocols use TCP and UDP, how port numbers identify application services, how multiple network conversations are multiplexed across the same LAN, and how TCP provides reliability through connection establishment, sequence numbers, acknowledgments, and flags.

The topology consisted of HTTP, FTP, DNS, and E-Mail clients connected through a switch to a MultiServer at `192.168.1.254`.

![Packet Tracer Lab 43 network topology]({{ '/assets/images/lab43/topology.png' | relative_url }})

## Objectives

The objectives of this lab were to:

- Generate network traffic in Packet Tracer Simulation Mode.
- Observe multiple protocols sharing the network through multiplexing.
- Inspect protocol data units (PDUs).
- Examine TCP connection establishment and application data transfer.
- Compare TCP and UDP behavior.
- Identify source and destination port numbers.
- Observe sequence and acknowledgment numbers used by TCP.
- Examine HTTP, FTP, DNS, SMTP, and POP3 traffic.

## Generating and Monitoring Network Traffic

I first generated traffic from each client application. Before beginning the simulation, I used the MultiServer to ping the LAN broadcast address:

```text
ping -n 1 192.168.1.255
```

This populated the ARP tables and reduced unnecessary ARP traffic during the simulation.

I then generated several types of application traffic:

- HTTP traffic from the HTTP Client to `192.168.1.254`
- FTP traffic using `ftp 192.168.1.254`
- DNS traffic using `nslookup multiserver.pt.ptu`
- Email traffic to `user@multiserver.pt.ptu`

Packet Tracer displayed colored PDU envelopes for the different communications.

![Multiple pending PDUs generated in Packet Tracer Simulation Mode]({{ '/assets/images/lab43/pendingPDUs.png' | relative_url }})

Using **Capture/Forward**, I observed the PDUs moving from the clients through the switch and toward the server. Multiple applications were able to share the same network infrastructure. This process is known as **multiplexing**.

The different colors assigned to the PDUs in Simulation Mode identify different protocols and correspond with the protocol colors displayed in the Simulation Panel.

## Monitoring HTTP over TCP

I filtered Simulation Mode to display HTTP and TCP traffic and generated an HTTP request to the MultiServer.

HTTP uses TCP, so application data was not transmitted until TCP had established a connection. This explains why the HTTP PDU did not immediately appear. TCP first performed its connection-establishment process.

I inspected an outbound HTTP PDU and recorded the following TCP information:

| TCP Field | Value |
| --- | --- |
| Source Port | 1026 |
| Destination Port | 80 |
| Sequence Number | 1 |
| Acknowledgment Number | 1 |
| Flags | ACK and PSH |

Port `80` identifies the HTTP service. Port `1026` was the temporary client-side source port used for this conversation.

![Outbound HTTP request showing TCP and HTTP PDU details]({{ '/assets/images/lab43/httpPDU.png' | relative_url }})

The TCP flag value was `0b00011000`, indicating that the **ACK** and **PSH** flags were set. ACK acknowledges previously received information, while PSH indicates that TCP should promptly deliver the application data.

The bottom of the PDU also showed the HTTP request itself, demonstrating that HTTP application data was encapsulated inside TCP.

### HTTP Server Response

I then followed the communication until the response returned from the MultiServer.

The inbound response contained:

| TCP Field | Value |
| --- | --- |
| Source Port | 80 |
| Destination Port | 1026 |
| Sequence Number | 1 |
| Acknowledgment Number | 103 |

![Inbound HTTP response showing reversed TCP port numbers]({{ '/assets/images/lab43/inboundPDU.png' | relative_url }})

The source and destination ports were reversed because the traffic was now traveling from the HTTP server back to the client:

```text
Client to Server:
192.168.1.1:1026 -> 192.168.1.254:80

Server to Client:
192.168.1.254:80 -> 192.168.1.1:1026
```

The acknowledgment number increased to `103`, demonstrating TCP's use of acknowledgment and sequencing information to track reliable communication.

Later in the HTTP conversation, I observed sequence number `103`, acknowledgment number `234`, and the FIN and ACK flags. This showed TCP terminating the established connection after the HTTP exchange was completed.

## Monitoring FTP over TCP

Next, I generated FTP traffic with:

```text
ftp 192.168.1.254
```

I filtered Simulation Mode to FTP and TCP and inspected the initial outbound PDU.

![Outbound FTP TCP PDU showing the initial SYN]({{ '/assets/images/lab43/ftpPDU.png' | relative_url }})

The first FTP TCP PDU contained:

| TCP Field | Value |
| --- | --- |
| Source Port | 1028 |
| Destination Port | 21 |
| Sequence Number | 0 |
| Acknowledgment Number | 0 |
| Flags | SYN |

TCP port `21` identifies the FTP control service. The SYN flag showed that this was the beginning of the TCP connection.

I followed the packets through the simulation and observed the complete TCP three-way handshake:

```text
FTP Client                         FTP Server
Port 1028                          Port 21

       -------- SYN -------->
       SEQ 0, ACK 0

       <----- SYN + ACK -----
             SEQ 0, ACK 1

       -------- ACK -------->
       SEQ 1, ACK 1
```

The server's SYN-ACK reversed the source and destination ports, using source port `21` and destination port `1028`. The client's final ACK switched them back to source port `1028` and destination port `21`.

After the TCP connection was established, the FTP server sent the following application-layer response:

```text
220 Welcome to PT Ftp server
```

This demonstrated that TCP established the reliable transport connection before the FTP application exchanged its data.

## Monitoring DNS over UDP

I then generated DNS traffic using:

```text
nslookup multiserver.pt.ptu
```

For this portion of the lab, I filtered Simulation Mode to DNS and UDP.

The outbound DNS query used:

| UDP Field | Value |
| --- | --- |
| Source Port | 1026 |
| Destination Port | 53 |
| Sequence Number | None |
| Acknowledgment Number | None |

The Layer 4 protocol was **UDP**. Unlike TCP, UDP did not perform a three-way handshake before transmitting the DNS query.

The DNS response reversed the port numbers:

```text
DNS Query:
Client port 1026 -> Server port 53

DNS Response:
Server port 53 -> Client port 1026
```

There were still no sequence or acknowledgment numbers because UDP does not provide TCP's connection-oriented reliability mechanisms.

The final section of the response PDU was labeled **DNS Answer** and resolved:

```text
multiserver.pt.ptu -> 192.168.1.254
```

This was an important contrast with the HTTP and FTP traffic. DNS could immediately send its UDP query without first exchanging SYN, SYN-ACK, and ACK packets.

## Monitoring Email over TCP

Finally, I monitored email traffic by filtering the simulation for POP3, SMTP, and TCP.

The initial connection used:

| TCP Field | Value |
| --- | --- |
| Source Port | 1026 |
| Destination Port | 25 |
| Sequence Number | 0 |
| Acknowledgment Number | 0 |
| Flags | SYN |

Port `25` identifies **SMTP (Simple Mail Transfer Protocol)**, which is used to send email.

As with FTP, I observed TCP perform its three-way handshake:

```text
E-Mail Client                     MultiServer
Port 1026                         SMTP Port 25

       -------- SYN -------->
       SEQ 0, ACK 0

       <----- SYN + ACK -----
             SEQ 0, ACK 1

       -------- ACK -------->
       SEQ 1, ACK 1
```

After the connection was established, the SMTP PDU contained source port `1026`, destination port `25`, sequence number `1`, acknowledgment number `1`, and the PSH and ACK flags. The application portion of the PDU was identified as **SMTP Data**.

The email protocols examined in the activity were:

- **TCP port 25 - SMTP**, used for sending email.
- **TCP port 110 - POP3**, used for retrieving email.

## TCP and UDP Comparison

Monitoring the individual PDUs made the differences between TCP and UDP visible rather than just theoretical.

| Characteristic | TCP | UDP |
| --- | --- | --- |
| Connection type | Connection-oriented | Connectionless |
| Reliability | Reliable delivery mechanisms | Best-effort delivery |
| Three-way handshake | Yes | No |
| Sequence numbers | Yes | No |
| Acknowledgment numbers | Yes | No |
| Connection flags | SYN, ACK, FIN, etc. | None |
| Relative overhead | Higher | Lower |
| Protocols observed | HTTP, FTP, SMTP, POP3 | DNS |
| Example ports observed | 80, 21, 25, 110 | 53 |

TCP required additional communication to establish and maintain a reliable connection. During the lab, I could see this directly through SYN, SYN-ACK, and ACK packets as well as changing sequence and acknowledgment numbers.

UDP did not establish a connection first. The DNS client sent its query directly to UDP port 53, and the server returned a response to the client's temporary port.

This lower overhead makes UDP useful when applications prioritize speed and simplicity and can tolerate the lack of TCP's built-in delivery guarantees. TCP is appropriate when reliable, ordered communication is important.

## Network Monitoring and Troubleshooting

Packet Tracer Simulation Mode served as the primary network-monitoring technology in this lab. Instead of only determining whether an application worked, I could inspect the traffic responsible for that application.

The monitoring process included:

- Filtering traffic by protocol.
- Following PDUs between endpoints.
- Inspecting Layer 3 and Layer 4 headers.
- Identifying source and destination IP addresses.
- Identifying application services through port numbers.
- Examining TCP sequence and acknowledgment numbers.
- Interpreting TCP flags.
- Distinguishing TCP from UDP behavior.
- Verifying DNS name resolution.
- Verifying that application data followed successful TCP connection establishment.

These techniques can also be applied to troubleshooting. For example, if an HTTP client failed to receive a page, monitoring could help determine whether the TCP handshake completed, whether traffic reached TCP port 80, and whether an HTTP response returned. If DNS resolution failed, inspecting DNS traffic could help determine whether the query reached UDP port 53 and whether a DNS Answer was returned.

This provides more diagnostic information than simply knowing that an application succeeded or failed.

## Results

The lab successfully demonstrated communication between multiple client applications and the MultiServer.

I observed:

- HTTP communication using TCP port 80.
- FTP control communication using TCP port 21.
- DNS communication using UDP port 53.
- SMTP communication using TCP port 25.
- POP3 associated with TCP port 110.
- TCP three-way handshakes.
- TCP sequence and acknowledgment tracking.
- TCP connection termination.
- UDP communication without connection establishment.
- DNS resolution of `multiserver.pt.ptu` to `192.168.1.254`.
- Multiplexing of multiple application conversations across the LAN.

## Reflection

This lab helped connect application protocols, transport protocols, and port numbers to the traffic that actually travels across a network. Packet Tracer Simulation Mode was especially useful because I could slow the communication down and inspect each PDU rather than only seeing the final result at the application.

The clearest difference I observed was between TCP and UDP. With FTP and email, I could watch TCP establish a connection through SYN, SYN-ACK, and ACK before application data was exchanged. TCP then used sequence and acknowledgment numbers to maintain the conversation. With DNS, the client sent the UDP query without establishing a connection first, and there were no TCP-style sequence or acknowledgment fields.

I also gained a better understanding of how port numbers allow multiple applications to communicate across the same network. The temporary client ports changed depending on the conversation, while the server used well-known ports such as 21 for FTP, 25 for SMTP, 53 for DNS, and 80 for HTTP.

The main challenge was following individual PDUs while several protocols were present at the same time. Using Packet Tracer's protocol filters made the traffic much easier to analyze. This reinforced why filtering is important when monitoring or troubleshooting a real network, where significantly more traffic would be present.

Overall, the activity demonstrated how network monitoring can be used not only to observe whether communication succeeds, but also to determine how applications communicate and where a failure could occur.
