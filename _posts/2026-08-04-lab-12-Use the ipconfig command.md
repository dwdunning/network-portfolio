---
title: "Packet Tracer Lab 12 - Use the ipconfig Command"
date: 2026-08-04
categories:
  - Networking
  - Packet Tracer
  - Module 13
tags:
  - Cisco
  - Packet Tracer
  - Network+
  - IP Configuration
  - ipconfig
  - Troubleshooting
---

## Overview

In this lab, I used the `ipconfig /all` command to troubleshoot a small office network. Four PCs were configured with static IPv4 addresses on a `192.168.1.0/24` network, but one workstation was unable to connect to the internet. By examining each computer's network configuration, I identified the incorrect settings and corrected the problem.

## Objectives

- Use the `ipconfig /all` command to examine IP configuration.
- Compare IP settings across multiple hosts.
- Identify an incorrect static IP configuration.
- Correct the misconfigured workstation.
- Verify successful completion of the activity.

## Network Topology

The network consisted of four PCs connected to a wireless router, which provided internet connectivity.

![Packet Tracer Topology](/assets/images/lab12/topology.png)

## Verifying the Configuration

I opened the Command Prompt on each PC and executed:

```text
ipconfig /all
```

### PC1

The first workstation was correctly configured.

- IPv4 Address: `192.168.1.101`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.1.1`

![PC1 ipconfig](/assets/images/lab12/pc1-ipconfig.png)

### PC2

PC2 immediately stood out because its IPv4 address belonged to a different network.

- IPv4 Address: `192.168.10.102` ❌
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.1.1`

The lab specified that every workstation should be on the `192.168.1.0/24` network, making this the incorrect configuration.

![PC2 ipconfig](/assets/images/lab12/pc2-ipconfig.png)

### PC3

PC3 was configured correctly.

- IPv4 Address: `192.168.1.103`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.1.1`

![PC3 ipconfig](/assets/images/lab12/pc3-ipconfig.png)

### PC4

PC4 also matched the expected configuration.

- IPv4 Address: `192.168.1.104`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.1.1`

![PC4 ipconfig](/assets/images/lab12/pc4-ipconfig.png)

## Correcting the Misconfiguration

Using **Desktop → IP Configuration** on PC2, I changed the IPv4 address from:

```text
192.168.10.102
```

to:

```text
192.168.1.102
```

The subnet mask, default gateway, and DNS server were already correct and did not require any changes.

![Corrected IP Configuration](/assets/images/lab12/pc2-ip-fix.png)

## Results

After correcting the IPv4 address, Packet Tracer immediately recognized the repair and marked the lab as successfully completed.

![Completed Activity](/assets/images/lab12/lab12-results.png)

## Key Takeaways

This lab demonstrated how quickly the `ipconfig /all` command can identify addressing problems.

Important troubleshooting points included:

- Verify the IPv4 address belongs to the correct subnet.
- Confirm the subnet mask matches the network design.
- Verify the default gateway is on the same network.
- Compare configurations across multiple hosts to quickly locate inconsistencies.
- A single incorrect octet in an IP address can prevent network communication even when every other setting is correct.

## Conclusion

This exercise reinforced the importance of verifying IP configuration before investigating more complex networking issues. Simple mistakes in static addressing are among the most common causes of connectivity problems, and tools such as `ipconfig /all` provide an efficient way to diagnose them.
