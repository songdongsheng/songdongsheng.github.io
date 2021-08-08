---
title: SCTP over UDP in the Linux kernel
description: SCTP over UDP in the Linux kernel
date: 2021-08-08 10:36:55
tags:
    - Programming
    - Python
categories: [Programming, Git]
permalink: sctp-over-udp-in-the-linux-kernel
---

# SCTP over UDP in the Linux kernel

## Introduction

Stream Control Transmission Protocol over User Datagram Protocol (SCTP over UDP, also known as UDP encapsulation of SCTP) is a feature defined in RFC6951 and implemented in the Linux kernel space since 5.11.

## Why we need SCTP over UDP

As the [author said](https://lore.kernel.org/netdev/cover.1603110316.git.lucien.xin@gmail.com/T/):

>   The Main Reasons:
>
>   1. To allow SCTP traffic to pass through legacy NATs, which do not
>      provide native SCTP support as specified in [BEHAVE] and
>      [NATSUPP].
>
>   2. To allow SCTP to be implemented on hosts that do not provide
>      direct access to the IP layer.  In particular, applications can
>      use their own SCTP implementation if the operating system does not
>      provide one.

The first reason will solve the middlebox issues that have brought many troubles to users and prevented SCTP’s wide use. The second reason is to allow user space applications to develop their own SCTP implementation based on the UDP protocol.

## How SCTP over UDP works

With this feature enabled, all SCTP packets are encapsulated into UDP packets. SCTP over UDP is implemented with kernel UDP tunnel APIs that have previously been used by the VXLAN, GENEVE, and TIPC protocols.

UDP-encapsulated SCTP is normally communicated between SCTP stacks using the IANA-assigned UDP port number 9899 (sctp-tunneling) on both ends.

There are circumstances where other ports may be used on either end, and it might be required to use ports other than the registered port, implementations need to allow other port numbers to be specified as a local or remote UDP encapsulation port number through APIs.

## How to use SCTP over UDP

When programming, you don't need to do anything different: All the standard SCTP features still apply, and all the APIs are available to use as before. Old applications will work well without any changes or recompilation. The only adjustment is to set up a UDP port (a local listening port or src port) and an encapsulation port (a remote listening or dest port), which could be done globally for the network namespace by sysctl:

```bash
# sysctl -w net.sctp.encap_port=9899
# sysctl -w net.sctp.udp_port=9899
```

Alternatively, you could set the encapsulation port per socket, association, or transport, using sockopt:

```c
setsockopt(SCTP_REMOTE_UDP_ENCAPS_PORT, port);
```

On the server side, the encapsulation port normally doesn’t need to be set explicitly, as detailed in the next section.

## The UDP encapsulation port

The UDP encapsulation port allows for very flexible usage. On the sender side, the global encapsulation port only provides a default value:

+ The per-socket encapsulation port can be used when another socket on one host connects to a different host on which a different UDP port is used.
+ The per-association encapsulation port can be used when the same socket connects to a different host on which a different UDP port is used.
+ The per-transport encapsulation port can be used when the same association wants to send UDP-encapsulated SCTP packets on one transport.

On the receiver side, the encapsulation port normally doesn’t need to be set:

+ The encapsulation port of one association would be learned from the first INIT packet. Other INITs with different UDP src ports would then be discarded.
+ The encapsulation port of each transport would be learned from the incoming packets on the corresponding path, and can be updated anytime.
+ Plain SCTP packets can still be processed even if the encapsulation ports of the association and its transports are set.

## Conclusion

If you’re using SCTP and enjoying its features, like multi-homing, multi-streaming, and partial-reliability, but having issues with middleboxes, the Linux kernel now provides an easier way to get around them.

## Documentation

+ [RFC 6951: UDP Encapsulation of SCTP Packets](https://datatracker.ietf.org/doc/html/rfc6951)
+ [PATCH v4 net-next 00/16 sctp: Implement RFC6951: UDP Encapsulation of SCTP](https://lore.kernel.org/netdev/cover.1603110316.git.lucien.xin@gmail.com/T/)
+ [SCTP of Linux Kernel IP Sysctl](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/tree/Documentation/networking/ip-sysctl.rst?h=linux-5.13.y#n2723)
