---
layout: post
title: "Lab 1 - Navigate the IOS"
date: 2026-06-15
categories: network-plus labs
---


## Objective

The purpose of this lab was to learn how to access and navigate the Cisco IOS command-line interface. I explored different IOS operating modes, practiced using built-in help features, and configured the switch clock while becoming familiar with common administrative commands.


## Tools Used

* Cisco Packet Tracer

## Lab Walkthrough

### Step 1 - Establishing a Console Connection

The first task was to connect PC1 to switch S1 using a console cable. After selecting the console cable in Packet Tracer, I connected the RS-232 port on the PC to the Console port on the switch. Once the connection was established, I opened the Terminal application on PC1 and accessed the Cisco IOS command-line interface.

![Initial Network Topology](/assets/images/lab01/initial-topology.png)

*Figure 1. Initial Packet Tracer topology showing PC1 and switch S1.*

![Console Connection Established](/assets/images/lab01/console-s1.png)

*Figure 2. Terminal session established with switch S1 through the console connection.*

### Step 2 - Exploring IOS Commands and EXEC Modes

After accessing the CLI, I explored the Cisco IOS help system using the question mark (`?`) command. This displayed a list of available commands and demonstrated how context-sensitive help can be used to discover command options.

I also practiced navigating between User EXEC mode (`S1>`) and Privileged EXEC mode (`S1#`). Using the `enable` command granted access to additional administrative commands and system information.

![User EXEC Mode](/assets/images/lab01/user-exec.png)

*Figure 3. User EXEC mode prompt displayed after connecting to the switch.*

![Context-Sensitive Help](/assets/images/lab01/user-exec-help.png)

*Figure 4. Using the question mark command to display available commands in User EXEC mode.*

![Privileged EXEC Mode](/assets/images/lab01/privileged-help.png)

*Figure 5. Privileged EXEC mode provides access to additional administrative commands.*

### Step 3 - Setting and Verifying the System Clock

The final portion of the lab focused on using context-sensitive help to configure the system clock. Initially, entering the `clock set` command without all required parameters resulted in an incomplete command error. Using the help system revealed the additional information required to successfully complete the command.

After providing the correct time, day, month, and year values, I verified the configuration using the `show clock` command.

![Clock Configuration](/assets/images/lab01/clock-set.png)

*Figure 6. Setting and verifying the switch clock using IOS commands.*

## Challenges Encountered

One challenge during the lab was understanding the required syntax for the `clock set` command. Entering only the time value resulted in an incomplete command error because additional date information was required. It was also necessary to understand the difference between User EXEC mode and Privileged EXEC mode in order to access certain commands.

## Troubleshooting

To resolve command errors, I used Cisco IOS context-sensitive help by entering a question mark (`?`) after commands. This feature displayed the parameters required to complete commands correctly. I also used command completion and prompt changes to confirm that I was operating in the correct IOS mode before entering commands.

## Results

The lab was completed successfully. I established a console connection to the switch, navigated the Cisco IOS command-line interface, explored User EXEC and Privileged EXEC modes, used context-sensitive help and tab completion, configured the system clock, and verified the configuration using the `show clock` command.

![Assessment Results](/assets/images/lab01/assessment.png)

*Figure 7. Packet Tracer assessment confirming successful completion of the activity.*


## What I Learned

I learned how to access and navigate the Cisco IOS command-line interface, use context-sensitive help, move between IOS operating modes, and configure basic system settings such as the device clock.