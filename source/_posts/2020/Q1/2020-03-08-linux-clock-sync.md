---
title: Synchronization of Linux clocks
description: Synchronization of Linux clocks
date: 2020-03-08 23:41:10
tags:
  - Cloud
  - Linux
categories: [Operating system, Linux]
permalink: Linux-clock-sync
---

# Synchronization of computer clocks by NTP and PTP

There are two supported protocols for synchronization of computer clocks over a network. The older and more well-known protocol is the Network Time Protocol (NTP). In its fourth version, NTP is defined by IETF in [RFC 5905](https://tools.ietf.org/html/rfc5905). The newer protocol is the Precision Time Protocol (PTP), which is defined in the [IEEE 1588-2008](https://en.wikipedia.org/wiki/Precision_Time_Protocol) standard.

The reference implementation of NTP is provided in the [ntp](http://www.ntp.org/) package.
[chrony](https://chrony.tuxfamily.org/) is a more versatile NTP implementation, which can usually synchronize the clock with better accuracy and has other advantages over the reference implementation. PTP is implemented in the [linuxptp](http://linuxptp.sourceforge.net/) package.

PTP was designed for local networks with broadcast/multicast transmission and, in ideal conditions, the system clock can be synchronized with sub-microsecond accuracy to the reference time. NTP was primarily designed for synchronization over the Internet using unicast, where it can usually achieve accuracy in the single-digit millisecond range.

The basic principles of the two protocols are the same. Computers or other devices that have a clock are connected in a network and form a hierarchy of time sources in which time is distributed from top to bottom. The devices on top are normally synchronized to a reference time source (e.g. a timing signal from a GPS receiver). Devices "below" periodically exchange timestamps with their time sources in order to measure the offset of their clocks. The clocks are continuously adjusted to correct for random variations in their rate (due to effects like thermal changes) and to minimize the observed offset.

In NTP, one level of the hierarchy is called stratum. The devices on top are stratum 1 servers, below them are stratum 2 clients, which are servers to stratum 3 clients, and so on. In PTP there are slaves, which are synchronized to their masters. Each communication path has one master and its slaves can be masters on other communication paths. The master on top is called grandmaster (GM). A device that has ports in two or more communication paths (i.e. it can be a slave and also master of other slaves at the same time) is a boundary clock (BC). Clocks with one port are ordinary clocks (OC). The group of all clocks that are directly or indirectly synchronized to each other using the protocol is called a PTP domain.

## Combining PTP with NTP

In order to get both accuracy and resiliency at the same time, it would be useful if PTP and NTP could be combined. PTP would be the primary source for synchronization of the clock when everything is working as expected. NTP would keep the PTP sources in check and allow for fallback between different PTP sources, or to NTP servers when all PTP sources fail.

### /etc/linuxptp/timemaster.conf

```
[ptp_domain 0]
interfaces ens3
delay 10e-6

#[ptp_domain 1]
#interfaces eth1
#delay 10e-6

[ntp_server us.pool.ntp.org]
minpoll 3
maxpoll 15
iburst 1

[ntp_server ca.pool.ntp.org]
minpoll 3
maxpoll 15
iburst 1

[ntp_server de.pool.ntp.org]
minpoll 3
maxpoll 15
iburst 1

[timemaster]
ntp_program chronyd
```

### Verification

```shell
# apt-get install -y linuxptp
# dpkg -L linuxptp | sort | less
    /etc/linuxptp/ptp4l.conf
    /etc/linuxptp/timemaster.conf
    /lib/systemd/system/ptp4l.service
    /lib/systemd/system/timemaster.service
    /usr/sbin/ptp4l
    /usr/sbin/timemaster
    /var/run/timemaster

# ptp4l -i ens3 -m -H
# ptp4l -i ens3 -m -S

# phc_ctl /dev/ptp0
# phc_ctl /dev/ptp0 get
# phc_ctl /dev/ptp0 cmp

# /usr/sbin/timemaster -f /etc/linuxptp/timemaster.conf -m -l 7 -n

# systemctl start timemaster && systemctl status timemaster -l --no-pager

# pmc -u -b 0 'GET CURRENT_DATA_SET'
# pmc -u -b 0 'GET TIME_STATUS_NP'

# chronyc sources
# chronyc -n tracking

# ethtool -T ens3
Time stamping parameters for ens3:
Capabilities:
        software-transmit     (SOF_TIMESTAMPING_TX_SOFTWARE)
        software-receive      (SOF_TIMESTAMPING_RX_SOFTWARE)
        software-system-clock (SOF_TIMESTAMPING_SOFTWARE)
PTP Hardware Clock: none
Hardware Transmit Timestamp Modes: none
Hardware Receive Filter Modes: none
```

## Virtual PTP hardware clock (PHC)

Virtual network devices in KVM guests do not support hardware timestamping, which means it is difficult to synchronize the clocks of guests that use a network protocol like NTP or PTP with better accuracy than tens of microseconds.

When a more accurate synchronization of the guests is required, it is recommended to synchronize the clock of the host using NTP or PTP with hardware timestamping, and to synchronize the guests to the host directly. Linux KVM provide a virtual PTP hardware clock (PHC), which enables the guests to synchronize to the host with a sub-microsecond accuracy.

### ptp_kvm

```shell
# modinfo ptp_kvm
filename:       /lib/modules/5.4.0-4-amd64/kernel/drivers/ptp/ptp_kvm.ko
license:        GPL
description:    PTP clock using KVMCLOCK
author:         Marcelo Tosatti <mtosatti@redhat.com>
depends:        ptp

# modprobe ptp_kvm
[31918.515562] pps_core: LinuxPPS API ver. 1 registered
[31918.515575] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
[31918.516788] PTP clock support registered

# echo ptp_kvm > /etc/modules-load.d/ptp_kvm.conf
# echo "refclock PHC /dev/ptp0 poll 2" >> /etc/chrony.conf
# systemctl restart chronyd
# chronyc sources
```

### /etc/chrony/chrony.conf

```
keyfile /etc/chrony/chrony.keys
driftfile /var/lib/chrony/chrony.drift
logdir /var/log/chrony

maxupdateskew 100.0
makestep 1 3
rtcsync

leapsecmode slew
maxslewrate 1000
smoothtime 400 0.001 leaponly

# PTP hardware clock (PHC) driver
refclock PHC /dev/ptp0 poll 2 trust

pool de.pool.ntp.org iburst maxsources 2 minpoll 3 maxpoll 15
pool us.pool.ntp.org iburst maxsources 2 minpoll 3 maxpoll 15
pool fr.pool.ntp.org iburst maxsources 2 minpoll 3 maxpoll 15
pool uk.pool.ntp.org iburst maxsources 2 minpoll 3 maxpoll 15
pool nl.pool.ntp.org iburst maxsources 2 minpoll 3 maxpoll 15
pool ru.pool.ntp.org iburst maxsources 2 minpoll 3 maxpoll 15
pool ca.pool.ntp.org iburst maxsources 2 minpoll 3 maxpoll 15
```

### Verification

```shell
# apt-get install -y chrony

# chronyd -f /etc/chrony/chrony.conf -d -Q

# systemctl restart chronyd && systemctl status chronyd --no-pager

# chronyc -n tracking
Reference ID    : 50484330 (PHC0)
Stratum         : 1
Ref time (UTC)  : Tue Mar 17 05:01:01 2020
System time     : 0.000000001 seconds slow of NTP time
Last offset     : -0.000000001 seconds
RMS offset      : 0.000000024 seconds
Frequency       : 1.393 ppm slow
Residual freq   : -0.000 ppm
Skew            : 0.001 ppm
Root delay      : 0.000000001 seconds
Root dispersion : 0.000004307 seconds
Update interval : 4.0 seconds
Leap status     : Normal

# chronyc -n sources

210 Number of sources = 15
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
#* PHC0                          0   2   377     5     +1ns[   -1ns] +/-   23ns
^- 185.244.195.159               3   8   377   229  -1259us[-1257us] +/-  104ms
^- 193.30.35.11                  2   8   377   230  +2118us[+2120us] +/-  113ms
^- 216.229.0.50                  2   8   377   228  +1084us[+1086us] +/-   44ms
^- 96.245.170.99                 2   8   377    31   -159us[ -159us] +/-  132ms
^- 212.83.145.32                 3   7   377    53    -55us[  -55us] +/-   97ms
^- 37.187.2.230                  3   8   377   227   +793us[ +795us] +/-  101ms
^- 195.219.205.9                 2   8   377   227  +1867us[+1869us] +/-   93ms
^- 178.62.16.103                 2   8   377   230   +572us[ +574us] +/-   87ms
^- 5.200.6.34                    2   4   377     3    -62ms[  -62ms] +/-  184ms
^- 5.39.184.5                    2   8   377    99   -407us[ -407us] +/-   90ms
^- 85.21.78.91                   2   8   377   118   +290us[ +291us] +/-  112ms
^- 91.207.136.55                 2   8   377   229  -1403us[-1401us] +/-  143ms
^- 192.99.2.172                  2   8   377    29  +1033us[+1033us] +/-   37ms
^- 216.197.156.83                1   7   377    18    +11ms[  +11ms] +/-   51ms
```

## Protocol dependencies

### PTP

- UDP: Typically, PTP uses UDP as its transport protocol (although other transport protocols are possible). The well known UDP ports for PTP traffic are 319 (Event Message) and 320 (General Message).
- Ethernet: Starting with IEEE1588 Version2, a native Layer2 Ethernet implementation was designed. PTP can use Ethernet as its transport protocol. The well known Ethernet type for PTP traffic is 0x88F7.

```shell
tcpdump -vv -i ens3 "(ether proto 0x88F7) or (udp port 319 or udp port 320)"
```

### NTP

NTP is a UDP-based service. NTP servers use well-known port 123 to talk to each other and to NTP clients. NTP clients use random ports above 1023.

```shell
tcpdump -vv -i any "udp port 123"
```

## Summary

Here is an overview of main features that are currently specified in the protocols and that have an effect on accuracy, resiliency, or security:

features                        | NTP          | PTP
--------------------------------|--------------|-------------
Delay correction                | No           | Yes
Transmit timestamp correction   | No           | Yes
Client-side source selection    | Yes          | No
Multiple sources                | Yes          | No
Estimation of maximum error     | Yes          | No
Authentication                  | Yes          | Experimental

Both NTP and PTP have some strong advantages over the other. PTP in ideal conditions with HW timestamping and transparent clocks can effectively eliminate the effect of the network on the measurements and synchronize the system clock with sub-microsecond accuracy. NTP is highly resilient. It works with multiple sources, estimates their errors, and selects only good sources for synchronization.
