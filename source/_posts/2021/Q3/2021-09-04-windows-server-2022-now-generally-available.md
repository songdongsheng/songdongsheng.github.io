---
title: Windows Server 2022 generally available
excerpt: Windows Server 2022 generally available
date: 2021-09-04 22:48:51
tags:
  - Operating system
  - Windows
categories:
  - Operating system
  - Windows
---

# Windows Server 2022 generally available

## Introduction

Windows Server 2022 now generally available — Microsoft rolled out Windows Server 2022 earlier this week in an unusually understated fashion, delivers innovation in security, hybrid, and containers.

According to the [Windows Server 2022 life cycle support page](https://docs.microsoft.com/en-us/lifecycle/products/windows-server-2022), Windows Server 2022 reached general availability on Aug 18, 2021.

##  Windows Server 2022 Lifecycle

Windows Server 2022 follows the Fixed Lifecycle Policy. Windows Server LTSCs Get 10 Years of Support, i.e. "5 years of mainstream support and 5 years of extended support".

This applies to the following editions: Datacenter, Datacenter: Azure Edition, Standard

| Start Date    | Mainstream End Date   | Extended End Date |
|---------------|-----------------------|-------------------|
| Aug 18, 2021  | Oct 13, 2026          | Oct 14, 2031      |

## Windows Server 2022 Features

### HTTPS and TLS 1.3 Enabled by Default

Windows Server 2022 use the latest security protocols, including HTTPS and TLS 1.3 by default. The server have TLS 1.0 and TLS 1.1 turned off by default.

### Encrypted DNS queries with DoH

DNS Client in Windows Server 2022 now supports DNS-over-HTTPS (DoH) which encrypts DNS queries using the HTTPS protocol. This helps keep your traffic as private as possible by preventing eavesdropping and your DNS data being manipulated. Learn more about [configuring the DNS client to use DoH](https://docs.microsoft.com/en-us/windows-server/networking/dns/doh-client-support).

### SMB AES-256 Encryption

Windows Server now supports AES-256-GCM and AES-256-CCM cryptographic suites for SMB encryption and signing. Windows will automatically negotiate this more advanced cipher method when connecting to another computer that also supports it, and it can also be mandated through Group Policy. Windows Server still supports AES-128 for down-level compatibility.

### SMB Direct and RDMA Encryption

SMB Direct and RDMA supply high bandwidth, low latency networking fabric for workloads like Storage Spaces Direct, Storage Replica, Hyper-V, Scale-out File Server, and SQL Server.

> In the past, if you were doing SMB direct and using SMB as a fabric, we did not let you encrypt. If you wanted to use encryption we'd let you turn it on and then we would turn off RDMA. Your performance would be really really terrible. Now, you're going to have the best of both worlds.

### Server Message Block over QUIC

SMB over QUIC updates the SMB 3.1.1 protocol in Windows Server 2022, it relies on the User Datagram Protocol (UDP) and the Transport Layer Security (TLS) 1.3 protocols. Mobile and telecommuter users no longer need a VPN to access their file servers over SMB when on Windows. More information can be found at the [SMB over QUIC documentation](https://docs.microsoft.com/en-us/windows-server/storage/file-server/smb-over-quic).

### SMB Compression

Windows Server 2022 has an SMB compression capability that can optionally compress files to speed up file transfers.

Per demonstrated how SMB compression handled a 20GB file during a [robocopy](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/robocopy) operation, it took almost three minutes to compress the 20GB file during the robocopy operation without SMB compression. With SMB compression turned on, the compression time was reduced to about 30 seconds. These compression benefits extend to end users accessing a file share through Windows Explorer, as well.

### UDP performance improvements

UDP is becoming a very popular protocol carrying more and more network traffic. The increasing popularity of RTP and custom (UDP) streaming and gaming protocols The QUIC protocol, built on top of UDP, brings the performance of UDP to a level on par with TCP. Significantly, Windows Server 2022 includes UDP Segmentation Offload (USO). USO moves most of the work required to send UDP packets from the CPU to the network adapter's specialized hardware. Complimenting USO is UDP Receive Side Coalescing (UDP RSC), which coalesces packets and reduces CPU usage for UDP processing. In addition, we have also made hundreds of improvements to the UDP data path both transmit and receive. Windows Server 2022 and Windows 11 both have this new capability.

### TCP performance improvements

Windows Server 2022 uses TCP [HyStart++](https://datatracker.ietf.org/doc/html/draft-ietf-tcpm-hystartplusplus-03) to reduce packet loss during connection start-up (especially in high-speed networks) and [RACK-TLP Loss Detection Algorithm](https://datatracker.ietf.org/doc/html/rfc8985) to reduce Retransmit TimeOuts (RTO). These features are enabled in the transport stack by default and provide a smoother network data flow with better performance at high speeds. Windows Server 2022 and Windows 11 both have this new capability.

### Application platform

There are several platform improvements for Windows Containers, including application compatibility and the Windows Container experience with Kubernetes. A major improvement includes reducing the Windows Container image size by up to 40%, which leads to a 30% faster startup time and better performance.

You can now also run applications that depend on Azure Active Directory with group Managed Services Accounts (gMSA) [without domain joining the container host](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/manage-serviceaccounts), and Windows Containers now support Microsoft Distributed Transaction Control (MSDTC) and Microsoft Message Queuing (MSMQ).

There are several other enhancements that simplify the Windows Container experience with Kubernetes. These enhancements include support for host-process containers for node configuration, IPv6, and consistent network policy implementation with Calico.

## Conclusion

Windows Server 2022 has some notable new features. On the security side, it has Secured Core boot protection, TLS 1.3 protocol use by default and Domain Name System over HTTPS encryption. Communications will be better protected from viewing with Server Message Block (SMB) over QUIC capability. The server also will have SMB compression for speedier file access.

## Reference

+ [Windows Server 2022 now generally available](https://cloudblogs.microsoft.com/windowsserver/2021/09/01/windows-server-2022-now-generally-available-delivers-innovation-in-security-hybrid-and-containers/)
+ [Windows Server 2022 life cycle support page](https://docs.microsoft.com/en-us/lifecycle/products/windows-server-2022)
+ [SMB over QUIC](https://docs.microsoft.com/en-us/windows-server/storage/file-server/smb-over-quic)
+ [SMB compression](https://docs.microsoft.com/en-us/windows-server/storage/file-server/smb-compression)
+ [Configuring the DNS client to use DoH](https://docs.microsoft.com/en-us/windows-server/networking/dns/doh-client-support)
+ [Create gMSAs for Windows containers](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/manage-serviceaccounts)
+ [robocopy](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/robocopy)
