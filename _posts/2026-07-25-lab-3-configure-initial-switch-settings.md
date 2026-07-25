---
layout: post
title: "Packet Tracer Lab 3 - Configure Initial Switch Settings"
date: 2026-07-25
categories: [Cisco, Packet Tracer, Networking]
tags: [Cisco, CCNA, Catalyst 2960, Packet Tracer, Switch Configuration]
---

# Packet Tracer Lab 3 - Configure Initial Switch Settings

**Course:** Cisco Networking Academy / Calbright College  
**Device:** Cisco Catalyst 2960 Switch

## Overview

This lab introduces the initial configuration of a Cisco Catalyst 2960 switch. Before making any configuration changes, I examined the default switch configuration to identify the available interfaces, management lines, and configuration storage. Later sections of the lab will cover securing administrative access, configuring a login banner, encrypting passwords, and saving the configuration.

## Step 1 - Enter Privileged EXEC Mode

When a Cisco switch first boots, it starts in **User EXEC mode**, indicated by the `>` prompt.

```text
Switch> enable
Switch#
```

The `enable` command enters **Privileged EXEC mode**, which provides access to administrative commands such as viewing the running configuration and entering configuration mode.

The prompt changes from `>` to `#`, indicating that the switch is now operating with administrative privileges.

## Step 2 - Examine the Default Configuration

Before making any changes, I examined the switch's current configuration.

```text
Switch# show running-config
```

The `show running-config` command displays the active configuration currently stored in RAM. Reviewing the existing configuration before making changes establishes a baseline and confirms the switch's default state.

### Verification

#### Fast Ethernet Interfaces

The Catalyst 2960 provides **24 Fast Ethernet interfaces**, numbered `FastEthernet0/1` through `FastEthernet0/24`.

#### Gigabit Ethernet Interfaces

The switch includes **2 Gigabit Ethernet uplink interfaces**, `GigabitEthernet0/1` and `GigabitEthernet0/2`.

#### Virtual Terminal (VTY) Lines

The default configuration includes **VTY lines 0–4**, which are used for remote management connections such as Telnet or SSH.

#### Viewing the Startup Configuration

The command below displays the configuration stored in **Non-Volatile RAM (NVRAM)**.

```text
show startup-config
```

#### Why is "startup-config is not present" displayed?

A new or recently reset switch does not yet have a saved startup configuration. At this point, only the **running configuration** exists in RAM. The startup configuration is created only after the running configuration is saved to NVRAM using:

```text
copy running-config startup-config
```

## Step 3 - Configure the Switch Hostname

To make the switch easier to identify, I changed the default hostname from **Switch** to **S1**.

```text
Switch# configure terminal
Switch(config)# hostname S1
S1(config)# exit
```

Changing the hostname is one of the first tasks performed when configuring a network device. It makes command prompts, logs, and troubleshooting much easier, especially when managing multiple switches.

## Step 4 - Secure Console Access

To prevent unauthorized users from accessing the switch through the console port, I configured a console password and enabled password authentication.

```text
S1# configure terminal
S1(config)# line console 0
S1(config-line)# password letmein
S1(config-line)# login
S1(config-line)# exit
S1(config)# exit
```

The `login` command is required because it instructs the switch to prompt for the configured password whenever someone connects through the console. Without `login`, the password would be stored but never enforced.

After exiting the session and reconnecting to the console, the switch prompted for the console password before allowing access.

## Step 5 - Secure Privileged EXEC Mode

Next, I configured an **enable password** to protect access to Privileged EXEC mode.

```text
S1# configure terminal
S1(config)# enable password c1$c0
S1(config)# exit
```

After reconnecting to the switch, I verified that two separate passwords were required:

1. The **console password** (`letmein`) to access User EXEC mode.
2. The **enable password** (`c1$c0`) to enter Privileged EXEC mode using the `enable` command.

Examining the running configuration showed that both passwords were stored in plain text.

## Step 6 - Configure an Enable Secret

Cisco IOS provides a more secure alternative to the `enable password` command by storing the password as a cryptographic hash.

```text
S1# configure terminal
S1(config)# enable secret itsasecret
S1(config)# exit
```

When both an `enable password` and an `enable secret` are configured, the **enable secret** takes precedence.

## Step 7 - Verify the Running Configuration

After configuring the enable secret, I examined the running configuration again.

```text
S1# show run
```

The configuration now displayed the enable secret as an encrypted hash rather than the original password.

- **What is displayed for the enable secret password?**

  The password is displayed as an encrypted hash instead of plain text.

- **Why is it displayed differently?**

  The `enable secret` command automatically hashes the password before storing it in the configuration. This prevents anyone viewing the configuration file from reading the original password.

## Step 8 - Encrypt Plain Text Passwords

Although the enable secret was already protected, the console and enable passwords were still visible in plain text. To encrypt them, I enabled Cisco IOS password encryption.

```text
S1# configure terminal
S1(config)# service password-encryption
S1(config)# exit
```

After running the command, the passwords were no longer displayed in plain text.

![Running configuration after enabling password encryption.]({{ '/assets/images/lab03/encrypted-passwords.png' | relative_url }})

*Figure 1. Running configuration after enabling password encryption.*

Notice that the console password and enable password now appear as **type 7 encrypted passwords**, while the enable secret continues to use a stronger one-way hash.

### Verification

**If additional passwords are configured after enabling `service password-encryption`, will they appear in plain text?**

No. Any passwords configured with commands that support `service password-encryption` will be stored in encrypted form rather than plain text. The `enable secret` remains protected using its own, stronger hashing mechanism.
## Step 9 - Configure a Message of the Day (MOTD) Banner

To display a security notice to anyone connecting to the switch, I configured a **Message of the Day (MOTD)** banner.

```text
S1# configure terminal
S1(config)# banner motd "This is a secure system. Authorized Access Only!"
S1(config)# exit
```

The `banner motd` command displays a message before the user is prompted to log in. Cisco IOS allows the banner text to be enclosed in quotation marks or separated by a custom delimiter.

### Verification

After reconnecting to the console, the switch displayed the configured banner before prompting for a password.

![Message of the Day (MOTD) banner displayed before login.]({{ '/assets/images/lab03/MOTD.png' | relative_url }})

**When is the MOTD banner displayed?**

The banner is displayed whenever someone connects to the switch, before the login prompt appears. This ensures that every user sees the message before attempting to authenticate.

**Why should every switch have an MOTD banner?**

An MOTD banner informs users that the device is intended for authorized access only. In production environments, it can also serve as a legal notice that unauthorized access is prohibited and that activity may be monitored. Configuring an MOTD banner is considered a networking security best practice because it clearly communicates the intended use of the device.

## Step 10 - Save the Configuration to NVRAM

After verifying that the running configuration was correct, I saved it to the switch's startup configuration so it would persist after a reboot or power loss.

```text
S1# copy running-config startup-config
Destination filename [startup-config]?
Building configuration...
[OK]
```

The `copy running-config startup-config` command copies the active configuration stored in RAM to **Non-Volatile RAM (NVRAM)**. Without this step, any configuration changes would be lost the next time the switch is restarted.

### Verification

The running configuration can be saved using the full command shown above, or the abbreviated form:

```text
copy run start
```

To verify that the configuration was successfully saved, I displayed the startup configuration.

```text
S1# show startup-config
```

The output confirmed that the configuration stored in NVRAM matched the running configuration, including:

- The hostname (`S1`)
- The encrypted console password
- The enable password and enable secret
- The MOTD banner
- Password encryption settings

**Which command displays the contents of NVRAM?**

```text
show startup-config
```

The `show startup-config` command displays the configuration stored in NVRAM, which is loaded when the switch boots.

**Are all the changes recorded in the startup configuration?**

Yes. After saving the running configuration, all of the configuration changes made during the lab—including the hostname, passwords, password encryption, and MOTD banner—were successfully written to NVRAM and will be retained after the switch is restarted.

## Step 11 - Configure the Second Switch

The final task was to configure a second Catalyst 2960 switch (**S2**) using the same procedures completed on **S1**.

Rather than following the instructions step by step, I repeated the configuration process from memory. This included:

- Assigning the hostname `S2`
- Configuring the console password
- Setting both an `enable password` and an `enable secret`
- Configuring the MOTD banner
- Enabling password encryption
- Verifying the running configuration
- Saving the startup configuration to NVRAM

After completing the configuration, Packet Tracer verified that both switches met all of the lab requirements.

![Packet Tracer assessment results showing a successful completion of the lab.]({{ '/assets/images/lab03/lab-complete.png' | relative_url }})

The assessment confirmed that both switches were configured correctly, including the hostname, console security, enable passwords, password encryption, MOTD banner, and startup configuration. The activity was completed with a score of **72/72 (100%)**.

## Summary

This lab introduced the basic workflow for configuring a Cisco Catalyst 2960 switch using Cisco IOS. By the end of the exercise, I was able to:

- Navigate between User EXEC, Privileged EXEC, and Global Configuration modes.
- Configure hostnames and secure console access.
- Protect privileged mode using both `enable password` and `enable secret`.
- Encrypt stored passwords using `service password-encryption`.
- Configure a Message of the Day (MOTD) banner.
- Save the running configuration to NVRAM.
- Repeat the complete configuration process on a second switch without step-by-step guidance.

Although the commands themselves are relatively simple, this lab established the foundation for future Cisco IOS configuration and reinforced the importance of securing network devices before placing them into service.