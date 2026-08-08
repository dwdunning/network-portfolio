---
layout: post
title: "Packet Tracer Lab 34 - Use FTP Services"
date: 2026-08-08
categories: [Networking, Cisco, Packet Tracer, Module-17-Basic-Network-Administration]
tags: [networking, cisco, packet tracer, ftp, file transfer, network services, troubleshooting, Network+]
---

## Overview

In this lab, I used File Transfer Protocol (FTP) to transfer files between a client PC and an FTP server. The activity demonstrated how to connect to an FTP server, authenticate with a username and password, upload a file, rename a remote file, download it back to the client, and remove a file from the server.

FTP uses TCP port 21 for commands and connection management and TCP port 20 for data transfer in traditional active FTP.

## Objectives

- Locate a file on the client PC.
- Connect and authenticate to an FTP server.
- Upload a file to the FTP server.
- Verify that the uploaded file exists on the server.
- Rename a file stored on the FTP server.
- Download a file from the FTP server.
- Verify the downloaded file on the client PC.
- Delete a file from the FTP server.

## Network Topology

The topology consisted of a PC connected through a wireless router and network infrastructure to a remote FTP server.

![Packet Tracer Lab 34 network topology]({{ '/assets/images/lab34/topology.png' | relative_url }})

## Addressing Information

| Device | Interface | IP Address | Subnet Mask |
|---|---|---|---|
| FTP Server (`ftp.pka`) | NIC | `209.165.200.226` | `255.255.255.224` |

## Part 1 - Upload a File to the FTP Server

### Locate the File

I opened the command prompt on the PC and used the `dir` command to examine the contents of the local `C:\` directory.

```text
C:\>dir
```

The directory listing showed the file required for the activity:

```text
sampleFile.txt
```

The file was 26 bytes in size.

![Local directory showing sampleFile.txt]({{ '/assets/images/lab34/samplefile.png' | relative_url }})

### Connect to the FTP Server

I connected to the FTP server using its IP address:

```text
C:\>ftp 209.165.200.226
```

The server responded and established the FTP connection.

I authenticated using the credentials provided by the lab:

```text
Username: student
Password: class
```

The server confirmed the successful login:

```text
230- Logged in
(passive mode On)
ftp>
```

![Successful FTP connection and login]({{ '/assets/images/lab34/ftplogin.png' | relative_url }})

### Upload the File

From the FTP prompt, I first used `dir` to view the files already stored on the server.

```text
ftp>dir
```

I then uploaded the local `sampleFile.txt` file with:

```text
ftp>put sampleFile.txt
```

The server reported a successful transfer:

```text
Writing file sampleFile.txt to 209.165.200.226:
File transfer in progress...

[Transfer complete - 26 bytes]
```

I ran `dir` again to verify that the uploaded file was present on the FTP server.

```text
ftp>dir
```

The server directory now contained:

```text
sampleFile.txt    26
```

![FTP server directory verifying the uploaded sampleFile.txt]({{ '/assets/images/lab34/verify.png' | relative_url }})

## Part 2 - Download a File from the FTP Server

### Rename the Remote File

Before downloading the file, I renamed it on the FTP server:

```text
ftp>rename sampleFile.txt sampleFile_FTP.txt
```

The FTP server confirmed the operation:

```text
[OK Renamed file successfully from sampleFile.txt to sampleFile_FTP.txt]
```

I used `dir` again to confirm that the server now contained:

```text
sampleFile_FTP.txt
```

### Download the Renamed File

I downloaded the renamed file from the FTP server using:

```text
ftp>get sampleFile_FTP.txt
```

The transfer completed successfully:

```text
Reading file sampleFile_FTP.txt from 209.165.200.226:
File transfer in progress...

[Transfer complete - 26 bytes]
```

I then exited the FTP client:

```text
ftp>quit
```

The server closed the FTP control connection:

```text
221- Service closing control connection.
```

Back at the local command prompt, I used `dir` to verify the downloaded file:

```text
C:\>dir
```

Both files were now present on the PC:

```text
sampleFile.txt
sampleFile_FTP.txt
```

Each file was 26 bytes, giving a total of 52 bytes.

![Successful FTP download and local verification]({{ '/assets/images/lab34/ftpandback.png' | relative_url }})

## Delete the File from the FTP Server

The final task was to reconnect to the FTP server and remove `sampleFile_FTP.txt`.

After reconnecting and authenticating, I used:

```text
ftp>delete sampleFile_FTP.txt
```

I verified the server directory afterward to ensure that `sampleFile_FTP.txt` was no longer present.

### Lab Question

**What command did you use to remove the file from the FTP server?**

```text
delete sampleFile_FTP.txt
```

It is important to distinguish between deleting the remote file while at the `ftp>` prompt and deleting a local copy from the `C:\>` prompt. The FTP `delete` command removes the file stored on the remote FTP server.

## FTP Commands Used

| Command | Purpose |
|---|---|
| `dir` | Lists files in the current directory |
| `ftp 209.165.200.226` | Connects to the FTP server |
| `put sampleFile.txt` | Uploads a file to the FTP server |
| `rename sampleFile.txt sampleFile_FTP.txt` | Renames a file on the FTP server |
| `get sampleFile_FTP.txt` | Downloads a file from the FTP server |
| `delete sampleFile_FTP.txt` | Deletes a file from the FTP server |
| `quit` | Closes the FTP session |

## Verification and Results

Packet Tracer reported successful completion of the activity with a score of **1/1**. The assessment specifically verified that `sampleFile_FTP.txt` had been successfully transferred to the PC.

![Packet Tracer Lab 34 completion results showing 1 out of 1]({{ '/assets/images/lab34/results.png' | relative_url }})

## Troubleshooting and Verification

No significant connectivity problems occurred during this activity. I verified each stage of the file-transfer process rather than relying only on the FTP client's success messages.

The `dir` command was especially useful because it allowed me to confirm the state of the file at multiple points:

1. `sampleFile.txt` existed on the local PC before the transfer.
2. `sampleFile.txt` appeared on the FTP server after the upload.
3. The server showed `sampleFile_FTP.txt` after the rename.
4. The local PC showed both files after the download.
5. The remote file was removed from the FTP server during cleanup.

This demonstrated the importance of verifying the actual result of a network operation instead of assuming that a command completed as intended.

## What I Learned

This lab provided hands-on experience with basic FTP client operations and demonstrated the client-server nature of network file transfers. I practiced establishing an FTP session, authenticating to a remote service, and managing files across the network.

The lab also reinforced the difference between **local and remote file operations**. Commands entered at the `ftp>` prompt operate on or through the FTP session, while commands entered at the `C:\>` prompt operate on the local PC.

Most importantly, the activity demonstrated a practical method for verifying network services. Successful connectivity alone does not prove that a service is functioning correctly. Authentication, uploading, downloading, renaming, deleting, and verifying the resulting files provided evidence that the FTP service was operating as expected.
