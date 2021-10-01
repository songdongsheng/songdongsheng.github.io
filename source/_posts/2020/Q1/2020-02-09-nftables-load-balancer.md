---
title: nftables load balancer
excerpt: nftables load balancer
date: 2020-02-09 18:15:39
tags:
  - Linux
categories: [Operating system, Linux]
---

# nftables load balancer

- https://netfilter.org/projects/nftables/index.html
- https://wiki.nftables.org/wiki-nftables/
- https://wiki.nftables.org/wiki-nftables/index.php/Scripting
- https://wiki.nftables.org/wiki-nftables/index.php/Main_differences_with_iptables
- https://wiki.nftables.org/wiki-nftables/index.php/Moving_from_iptables_to_nftables
- https://wiki.nftables.org/wiki-nftables/index.php/Quick_reference-nftables_in_10_minutes
- https://wiki.archlinux.org/index.php/Nftables
- https://wiki.gentoo.org/wiki/Nftables
- https://wiki.gentoo.org/wiki/Nftables/Examples
- https://github.com/zevenet/nftlb
- https://www.zevenet.com/knowledge-base/nftlb/what-is-nftlb/
- https://www.zevenet.com/blog/nftables-load-balancing-10x-faster-lvs/

## What is nftables?

**nftables** replaces the popular **{ip,ip6,arp,eb}**tables. This software provides a new in-kernel packet classification framework that is based on a network-specific Virtual Machine (VM) and a new **nft** userspace command line tool. **nftables** reuses the existing Netfilter subsystems such as the existing hook infrastructure, the connection tracking system, NAT, userspace queueing and logging subsystem.

## What is the status of nftables?

Available upstream since **Linux kernel 3.13**. It supports 3/4 of the existing iptables features, although it provides new features that you cannot find in iptables.

## Running nftables

You require the following software in order to run the nft command line tool:

- Linux kernel since 3.13, although newer kernel versions are recommended.
- libmnl: the minimalistic Netlink library
- libnftnl: low level netlink userspace library
- nft: command line tool

**nft** syntax differs from {ip,ip6,eb,arp}tables. Moreover, there is a backward compatibility layer that allows you run iptables/ip6tables, using the same syntax, over the nftables infrastructure.

    nft list ruleset > /etc/nftables.rules
    nft flush ruleset
    nft -f /etc/nftables.rules

## What is nftables load balancer?

nftlb stands for nftables load balancer, the next generation linux firewall that will replace iptables is adapted to behave as a complete load balancer and traffic distributor.

nftlb is a nftables rules manager to create virtual services for load balancing at layer 2, layer 3 and layer 4, minimizing the number of rules and using structures to match efficiently the packets. It’s also provided with an easy JSON API service to have the flexibility to interact with nftlb programmatically and to meet automation. So you can use your preferred health checker to be integrated with nftlb very easily.

The philosophy of nftlb is to maintain the data path into the kernel, in order to achieve the most performance possible, but the control plane and heath checks into user space to have the flexibility to change the behavior easily but also to be compatible with the rest of the linux stack.

At Zevenet, we’ve been using iptables and the netfilter infrastructure for years to create a full featured load balancer, hence we know very well the limitations of such approach that we’re saving with nftlb.


## Why is nftlb needed

The linux kernel already counts of an internal load balancer called IPVS, or also known as LVS (Linux Virtual Server), which is a complete piece of software and very stable that has been used for years. But such load balancer has some limitations: the kernel side is used for tasks that should be performed by the user space, for certain capabilities it duplicates the infrastructure that currently netfilter provides, and it relies on iptables and other pieces of software if is required something more complex (like transparent proxy, multiport or multiprotocol). It provides SNAT and DSR topologies, but not DNAT.

With the iptables approach the main limitations are the number of rules to be created by virtual service and number of backends (a minimum of ~2 rules per backend with several matches included in it) and an increasing linear complexity according to the number of backends added. The sequentially processing of rules also slows down the performance if too much rules are included, and this is even worse due to the classic iptables locking problem. In order to provide IPv6 load balancing it has the inconvenience of the use a different command, ip6tables. In addition, this approach is able to provide DNAT (Destination NAT for transparency) and SNAT (Source NAT) load balancing but not able to work in DSR (Direct Server Return) topologies.

With nftlb based on nftables we save all of these issues:
- It can build all these topologies and change from one to another very easily.
- It can handle multiport and multiprotocol natively.
- It can manage IPv4 and IPv6 traffic seamlessly.
- It’s used just one interface to provide all the required capabilities for load balancing.
- nftables provides a more expressive language so we can use 2 rules to build a complete load balancer with constant complexity!
 - The matches are indexed per virtual service, so we don’t need to sequentially process all of them.
 - It is provided of the RCU subsystem so there is no locking problem when updating rules.
 - The data path remains in the kernel space but providing the flexibility in the user space for the control plane.
 - It has been proven that it can perform 10x faster than LVS.

## nftlb features

Currently, nftlb provides the following capabilities:
- Topologies supported: Destination NAT, Source NAT, Direct Server Return and Stateless DNAT. This enables the use of the load balancer in one-armed and two-armed network architectures.
- Support for both IPv4 and IPv6 families.
- Multilayer load balancer: DSR in layer 2, IP based load balancing with protocol agnostic at layer 3, and support of load balancing of UDP, TCP and SCTP at layer 4.
- Multiport support for ranges and lists of ports.
- Multiple virtual services (or farms) support.
- Schedulers available: weight, round robin, configurable hash (per IP, port, MAC or combination of them) and symmetric hash.
- Support of configurable persistence or client-backend affinity with a timeout (per IP, port, MAC or combination of them).
- Support of security policies per service: white and blacklists (from ingress), queuing to user space filter, filtering of bogus TCP frames, maximum number of established connections, limit TCP RST per second, limit new connections per second and more.
- Priority support per backend.
- Live management of virtual services and backends programmatically through a JSON API.
- Web service authentication with a security Key.
-  Automated testbed included.
