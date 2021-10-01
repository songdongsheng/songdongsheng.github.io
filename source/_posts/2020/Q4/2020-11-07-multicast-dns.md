---
title: Multicast DNS (mDNS)
excerpt: Multicast DNS (mDNS)
date: 2020-11-07 13:10:21
tags:
    - Operating system
    - Linux
categories: [Operating system, Linux]
---

# Multicast DNS (mDNS)

## Overview

- [Multicast DNS (mDNS)](https://tools.ietf.org/html/rfc6762)
- [DNS-Based Service Discovery](https://tools.ietf.org/html/rfc6763)
- [Link-local Multicast Name Resolution (LLMNR)](https://tools.ietf.org/html/rfc4795)
- [Home Networking Control Protocol](https://tools.ietf.org/html/rfc7788)
- [Avahi](https://avahi.org/)
- [nss-mdns](https://github.com/lathiat/nss-mdns)
- [Download ISC's software](https://www.isc.org/download/)
- [Mdns daemon for OpenBSD](https://github.com/haesbaert/mdnsd)
- [Mdns daemon not support IPv6 yet](https://github.com/haesbaert/mdnsd/issues/4)
- [Local name resolution in windows networks#11](https://www.slideshare.net/MenandMice/part-2-local-name-resolution-in-windows-networks)
- [Local name resolution in Linux](https://www.slideshare.net/MenandMice/part-3-local-name-resolution-in-linux-freebsd-and-macosios)

Multicast DNS (mDNS) is a way of using familiar DNS programming interfaces, packet formats and operating semantics, in a small network where no conventional DNS server has been installed.

Multicast DNS (mDNS) is a joint effort by participants of the IETF Zero Configuration Networking (zeroconf) and DNS Extensions (dnsext) working groups. The requirements are driven by the Zeroconf working group; the implementation details are a chartered work item for the DNSEXT group. Most of the people working on mDNS are active participants of both working groups.

Multicast DNS (mDNS) provides the ability to perform DNS-like operations on the local link in the absence of any conventional Unicast DNS server. In addition, Multicast DNS designates a portion of the DNS namespace to be free for local use, without the need to pay any annual fee, and without the need to set up delegations or otherwise configure a conventional DNS server to answer for those names.

The primary benefits of Multicast DNS are that:

-   mDNS require little or no administration or configuration to set them up
-   mDNS work when no infrastructure is present
-   mDNS work during infrastructure failures

Avahi is a free Zero-configuration networking (zeroconf) implementation, including a system for multicast DNS/DNS-SD service discovery. It allows programs to publish and discover services and hosts running on a local network with no specific configuration.

### Protocol overview

When an mDNS client needs to resolve a hostname, it sends an **IP multicast** query message that asks the host having that name to identify itself. That target machine then multicasts a message that includes its IP address. All machines in that subnet can then use that information to update their mDNS caches. Any host can relinquish its claim to a name by sending a response packet with a **time to live** (TTL) equal to zero.

By default, mDNS exclusively resolves hostnames ending with the **.local** top-level domain. This can cause problems if **.local** includes hosts that do not implement mDNS but that can be found via a conventional unicast DNS server. Resolving such conflicts requires network-configuration changes that mDNS was designed to avoid.

### Packet structure

An mDNS message is a multicast UDP packet sent using the following addressing:

-   IPv4 address **224.0.0.251** or IPv6 address **ff02::fb**
-   **UDP** port **5353**
-   When using Ethernet frames, the standard IP multicast MAC address **01:00:5E:00:00:FB** (for IPv4) or **33:33:00:00:00:FB** (for IPv6)

The payload structure is based on the **unicast DNS packet format**, consisting of two parts — the header and the data.

The header is identical to that found in unicast DNS, as are the sub-sections in the data part: queries, answers, authoritative-nameservers, and additional records. The number of records in each sub-section matches the value of the corresponding COUNT field in the header.

## Install Avahi

```shell
$ sudo apt-get install -y --no-install-recommends avahi-daemon libnss-mdns

$ dig -p 5353 @ff02::fb OP-7020-01.local aaaa
$ dig -p 5353 @224.0.0.251 OP-7020-01.local
$ dig -p 5353 @224.0.0.251 OP-7020-01.local aaaa
```

## Using Avahi

### Hostname resolution

Avahi provides local hostname resolution using a "hostname.local" naming scheme. To enable it, install the **nss-mdns** package and start **avahi-daemon.service**.

Then, edit the file **/etc/nsswitch.conf** and change the **hosts** line to include **mdns_minimal [NOTFOUND=return]** before **dns**:

```shell
$ sudo vi /etc/nsswitch.conf

# mdns_minimal: IPv6 and IPv4, may slowdowns in resolving .local hosts
# mdns4_minimal: IPv4 only
# mdns6_minimal: IPv6 only
hosts:          files mdns_minimal [NOTFOUND=return] dns myhostname
```

### Configuring mDNS for custom TLD

The mdns_minimal module handles queries for the **.local** TLD only. Note the **[NOTFOUND=return]**, which specifies that if **mdns_minimal** cannot find **\*.local**, it will not continue to search for it in dns, myhostname, etc.

In case you want Avahi to support other TLDs, you should:

-   replace **mdns_minimal [NOTFOUND=return]** with the full **mdns** module. There also are IPv4-only and IPv6-only modules **mdns[46]\(\_minimal\)**
-   customize **/etc/avahi/avahi-daemon.conf** with the **domain-name** of your choice
-   whitelist Avahi custom TLDs in **/etc/mdns.allow**

### Avahi services discover tools

Avahi includes several utilities which help you discover the services running on a network. For example, run

```shell
$ sudo apt-get install -y --no-install-recommends avahi-utils
$ avahi-browse --all --resolve --terminate
$ avahi-browse --all --ignore-local --resolve --terminate
```

to discover services in your network.

### Adding services to Avahi

[The Avahi mDNS/DNS-SD daemon](https://www.freebsd.org/cgi/man.cgi?query=avahi-daemon) advertises the services whose **\*.service** files are found in **/etc/avahi/services/**. Files in this directory must be readable by the **avahi** user/group. See [avahi.service(5)](https://www.freebsd.org/cgi/man.cgi?query=avahi.service) for more details.

### Firewall

Be sure to open **UDP port 5353** if you're using a firewall.

### Windows

Windows 10 may not discover services by IPv4, but IPv6 works:

```powershell
PS C:\> ping -6 raspberrypi.local

Pinging raspberrypi.local [2409:8a55:8ba:fb20:e010:9723:cb71:a0cb] with 32 bytes of data:
Reply from 2409:8a55:8ba:fb20:e010:9723:cb71:a0cb: time=5ms
Reply from 2409:8a55:8ba:fb20:e010:9723:cb71:a0cb: time=9ms
Reply from 2409:8a55:8ba:fb20:e010:9723:cb71:a0cb: time=13ms
Reply from 2409:8a55:8ba:fb20:e010:9723:cb71:a0cb: time=4ms

Ping statistics for 2409:8a55:8ba:fb20:e010:9723:cb71:a0cb:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 4ms, Maximum = 13ms, Average = 7ms
```

Windows 10 handles both hostname and hostname.local identically: it simultaneously tries LLMNR for the bare hostname, NetBIOS for the bare hostname, and (optionally) mDNS for hostname.local.

To activate the mDNS support, you have to Turn off the Link Local Multicast Name Resolution (LLMNR), i.e. set the **EnableMulticast** registry value to 0 (0: Disable LLMNR, 1: Use LLMNR):

```powershell
PS C:\> netsh dnsclient show state
PS C:\> resolve-dnsname OP-7020-01.local -LlmnrOnly

PS C:\> reg delete "HKLM\Software\Policies\Microsoft\Windows NT\DNSClient" /v EnableMulticast /f
The operation completed successfully.

PS C:\> reg add "HKLM\Software\Policies\Microsoft\Windows NT\DNSClient" /v EnableMulticast /t REG_DWORD /d 0 /f
The operation completed successfully.

PS C:\> reg query "HKLM\Software\Policies\Microsoft\Windows NT\DNSClient"

HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows NT\DNSClient
    EnableMulticast    REG_DWORD    0x0
```
