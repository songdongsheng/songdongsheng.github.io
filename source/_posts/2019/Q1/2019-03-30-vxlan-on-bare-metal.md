---
title: VXLAN on Bare Metal
excerpt: VXLAN on Bare Metal
date: 2019-03-30 13:20:10
tags:
  - Programming
  - IP
categories: [Programming, IP]
---

# VXLAN on Bare Metal

## Host way and sun

    127.0.0.1                       localhost
    23.239.7.231                    way.songdongsheng.info          way
    172.104.155.135                 sun.songdongsheng.info          sun

    ::1                             localhost
    2600:3c01::f03c:91ff:fed5:e79a  way.songdongsheng.info          way
    2a01:7e01::f03c:91ff:fe60:3c56  sun.songdongsheng.info          sun

## Enable IP Forwarding

    net.ipv4.ip_forward = 1

## Open UDP/4789 port

Since VXLAN uses UDP packet to forward encapsulated the L2 frames, UDP/4789 port must be opened.

    iptables -t filter -I INPUT -p udp -m udp --dport 4789 -j ACCEPT


## Increase maximum number of IGMP memberships

    # cat /proc/sys/net/ipv4/igmp_max_memberships
    20

    echo 256 >/proc/sys/net/ipv4/igmp_max_memberships

## VXLAN and route on way

    ip link add vxlan.1 type vxlan id 1 remote 172.104.155.135 dev eth0 dstport 4789

    ip link add vxlan.1 type vxlan id 1 dstport 4789
    bridge fdb append 00:00:00:00:00:00 dev vxlan.1 dst 172.104.155.135
    bridge fdb append 00:00:00:00:00:00 dev vxlan.1 dst 23.239.7.231

    ip address add 192.168.101.1 dev vxlan.1
    ip link set up vxlan.1 mtu 1450
    route add -net 192.168.0.0/16 gw 192.168.101.1

    bridge fdb show dev vxlan.1

## VXLAN and route on sun

    ip link add vxlan.1 type vxlan id 1 remote 23.239.7.231 dev eth0 dstport 4789

    ip link add vxlan.1 type vxlan id 1 dstport 4789
    bridge fdb append 00:00:00:00:00:00 dev vxlan.1 dst 172.104.155.135
    bridge fdb append 00:00:00:00:00:00 dev vxlan.1 dst 23.239.7.231

    ip address add 192.168.102.1 dev vxlan.1
    ip link set up vxlan.1 mtu 1450
    route add -net 192.168.0.0/16 gw 192.168.102.1

    bridge fdb show dev vxlan.1

## MTU of VXLAN

步骤|  操作/封包   |   协议   |                长度                |MTU
----|--------------|----------|------------------------------------|---------------
  1 | ping -s 1422 | ICMP     | 1430 = 1422 + 8 (ICMP header)      |
  2 | L3           | IP       | 1450 = 1430 + 20 (IP header)       | VxLAN Interface 的 MTU
  3 | L2           | Ethernet | 1464 = 1450 + 14 (Ethernet header) |
  4 | VxLAN        | UDP      | 1480 = 1464 + 8 (VxLAN header) + 8 (UDP header) |
  5 | L3           | IP       | 1500 = 1480 + 20 (IP header)       | 物理网卡的(IP)MTU，它不包括 Ethernet header 的长度
  6 | L2           | Ethernet | 1514 = 1500 + 14 (Ethernet header) | 最大可传输帧大小

因此，VxLAN 的 overhead 是 1514 - 1464 = 50 byte。

## VXLAN verify

    ethtool vxlan.1
    ethtool -i vxlan.1
    ethtool -k vxlan.1

    bridge fdb show vxlan.1
    ip -d link show vxlan.1
    ip monitor neigh dev vxlan.1

    ethtool -i docker0
