---
layout: post
title: "Packet Tracer Lab 04 - Configure Initial Router Settings"
date: 2026-07-31
categories: [networking, cisco, packet-tracer]
tags: [ccna, network+, router, cisco-ios, packet-tracer]
categories: [module-11]
---

## Overview

This lab introduced the initial configuration of a Cisco router using the Cisco IOS command-line interface (CLI). While the process is very similar to configuring a switch, routers include additional interface types and routing capabilities that become important in later networking labs.

The objectives of this lab were to:

- Examine a router's default configuration
- Configure basic security settings
- Secure console and privileged access with passwords
- Configure a Message of the Day (MOTD) banner
- Encrypt stored passwords
- Save the running configuration to NVRAM

---

## Lab Results

Packet Tracer includes an automated assessment tool that verifies each required configuration step. After completing the lab, every required task passed successfully, resulting in a perfect score.

![Packet Tracer assessment showing a perfect score.]({{ site.baseurl }}/assets/images/lab04/results.png)

---

## Part 1 – Exploring the Default Configuration

After connecting to the router through the console port, I entered Privileged EXEC mode and examined the running configuration.

```text
Router> enable
Router# show running-config
```

The default configuration revealed several useful pieces of information about the hardware before any configuration changes were made.

### Default Hardware Configuration

| Item | Value |
| ---- | ----: |
| Hostname | Router |
| Gigabit Ethernet Interfaces | 2 |
| Fast Ethernet Interfaces | 4 |
| Serial Interfaces | 2 |
| VTY Lines | 0–4 |

One interesting observation was that the Cisco 1941 router includes multiple interface types. Unlike the switches used in my previous lab, the router includes Gigabit Ethernet, Fast Ethernet, and Serial interfaces that will later be used to connect multiple networks together.

### Startup Configuration

Running the following command:

```text
show startup-config
```

returned:

```text
startup-config is not present
```

This occurs because the router has not yet had a configuration saved into NVRAM. At this point, all configuration exists only in RAM and would be lost if the router were rebooted.

---

## Part 2 – Securing the Router

The next step was to perform the router's initial security configuration. These settings establish a secure baseline before the router is placed into service.

The following changes were made:

- Changed the hostname from **Router** to **R1**
- Configured a Message of the Day (MOTD) banner
- Configured an `enable password`
- Configured an `enable secret`
- Enabled password encryption
- Configured and secured the console line

The running configuration reflects these changes.

![Running configuration showing the hostname, password encryption, and privileged access configuration.]({{ site.baseurl }}/assets/images/lab04/password.png)

Further down in the configuration, the Message of the Day banner and console login configuration can be seen.

![MOTD banner and console line configuration.]({{ site.baseurl }}/assets/images/lab04/motd.png)

One detail I found particularly interesting is that the `enable secret` is stored as a secure hash, while the `enable password` is encrypted after enabling `service password-encryption`. When both passwords are configured, Cisco IOS always uses the `enable secret` because it provides stronger protection.

---

## Part 3 – Saving the Configuration

After verifying that the router accepted the new configuration and that the passwords worked correctly, I saved the running configuration to NVRAM.

```text
copy running-config startup-config
```

Saving the running configuration creates a startup configuration that the router automatically loads during the next boot. Without this step, all configuration changes would be lost after a reboot or power failure.

As an optional exercise, I also examined the router's flash memory. Flash stores the Cisco IOS operating system along with any additional files saved on the device. After copying the startup configuration to flash, the backup appeared alongside the IOS image.

![Contents of the router's flash memory showing the IOS image and startup configuration backup.]({{ site.baseurl }}/assets/images/lab04/flash.png)

---

## Key Concepts Learned

This lab reinforced several important Cisco IOS concepts that will continue to appear throughout future networking exercises.

- **Running Configuration** is stored in RAM and is lost unless it is saved.
- **Startup Configuration** is stored in NVRAM and is loaded automatically when the router boots.
- The `enable secret` password is preferred over the older `enable password` because it provides stronger protection.
- The `service password-encryption` command prevents passwords from appearing in plain text within the configuration.
- A Message of the Day (MOTD) banner provides a warning to unauthorized users before they log in.
- Flash memory stores the Cisco IOS operating system and can also hold configuration backups.

---

## Final Thoughts

Although this lab built directly on the switch configuration completed previously, it introduced several router-specific concepts that are fundamental to Cisco networking. Learning how routers store their operating system and configuration files, how to secure administrative access, and how to preserve configuration changes across reboots provides a solid foundation for future labs involving interface configuration, routing protocols, and multi-network connectivity.

Each networking lab continues to build on the previous one, making these basic configuration tasks an essential part of developing practical Cisco IOS skills.
