---
title: WireGuard - Next Generation Network Tunnel
description: WireGuard - Next Generation Network Tunnel
date: 2020-01-01 23:16:22
tags:
    - IP
categories: [Programming, IP]
permalink: wire-guard
---

# WireGuard

-   https://www.wireguard.com/
-   https://www.wireguard.com/install/
-   https://www.wireguard.com/quickstart/
-   https://www.wireguard.com/protocol/
-   https://www.wireguard.com/repositories/
-   https://www.wireguard.com/papers/wireguard.pdf

WireGuard® is an extremely simple yet fast and modern VPN that utilizes state-of-the-art cryptography. It aims to be faster, simpler, leaner, and more useful than IPsec, while avoiding the massive headache. It intends to be considerably more performant than OpenVPN. WireGuard is designed as a general purpose VPN for running on embedded interfaces and super computers alike, fit for many different circumstances. Initially released for the Linux kernel, it is now cross-platform (Windows, macOS, BSD, iOS, Android) and widely deployable. It is currently under heavy development, but already it might be regarded as the most secure, easiest to use, and simplest VPN solution in the industry.

-   Simple & Easy-to-use
-   Cryptographically Sound
-   Minimal Attack Surface
-   High Performance
-   Well Defined & Thoroughly Considered

## Conceptual Overview

WireGuard securely encapsulates IP packets over UDP. You add a WireGuard interface, configure it with your private key and your peers' public keys, and then you send packets across it. All issues of key distribution and pushed configurations are out of scope of WireGuard; these are issues much better left for other layers, lest we end up with the bloat of IKE or OpenVPN. In contrast, it more mimics the model of SSH and Mosh; both parties have each other's public keys, and then they're simply able to begin exchanging packets through the interface.

## Simple Network Interface

WireGuard works by adding a network interface (or multiple), like eth0 or wlan0, called wg0 (or wg1, wg2, wg3, etc). This network interface can then be configured normally using ifconfig(8) or ip-address(8), with routes for it added and removed using route(8) or ip-route(8), and so on with all the ordinary networking utilities. The specific WireGuard aspects of the interface are configured using the wg(8) tool. This interface acts as a tunnel interface. WireGuard associates tunnel IP addresses with public keys and remote endpoints.

## Cryptokey Routing

At the heart of WireGuard is a concept called Cryptokey Routing, which works by associating public keys with a list of tunnel IP addresses that are allowed inside the tunnel. Each network interface has a private key and a list of peers. Each peer has a public key. Public keys are short and simple, and are used by peers to authenticate each other. They can be passed around for use in configuration files by any out-of-band method, similar to how one might send their SSH public key to a friend for access to a shell server.

When sending packets, the list of allowed IPs behaves as a sort of routing table, and when receiving packets, the list of allowed IPs behaves as a sort of access control list.

This is what we call a Cryptokey Routing Table: the simple association of public keys and allowed IPs.

Any combination of IPv4 and IPv6 can be used, for any of the fields. WireGuard is fully capable of encapsulating one inside the other if necessary.

Because all packets sent on the WireGuard interface are encrypted and authenticated, and because there is such a tight coupling between the identity of a peer and the allowed IP address of a peer, system administrators do not need complicated firewall extensions, such as in the case of IPsec, but rather they can simply match on "is it from this IP? on this interface?", and be assured that it is a secure and authentic packet. This greatly simplifies network management and access control, and provides a great deal more assurance that your iptables rules are actually doing what you intended for them to do.

## Built-in Roaming

The client configuration contains an initial endpoint of its single peer (the server), so that it knows where to send encrypted data before it has received encrypted data. The server configuration doesn't have any initial endpoints of its peers (the clients). This is because the server discovers the endpoint of its peers by examining from where correctly authenticated data originates. If the server itself changes its own endpoint, and sends data to the clients, the clients will discover the new server endpoint and update the configuration just the same. Both client and server send encrypted data to the most recent IP endpoint for which they authentically decrypted data. Thus, there is full IP roaming on both ends.

## Ready for Containers

WireGuard sends and receives encrypted packets using the network namespace in which the WireGuard interface was originally created. This means that you can create the WireGuard interface in your main network namespace, which has access to the Internet, and then move it into a network namespace belonging to a Docker container as that container's only interface. This ensures that the only possible way that container is able to access the network is through a secure encrypted WireGuard tunnel.

## NAT and Firewall Traversal Persistence

By default, WireGuard tries to be as silent as possible when not being used; it is not a chatty protocol. For the most part, it only transmits data when a peer wishes to send packets. When it's not being asked to send packets, it stops sending packets until it is asked again. In the majority of configurations, this works well. However, when a peer is behind NAT or a firewall, it might wish to be able to receive incoming packets even when it is not sending any packets. Because NAT and stateful firewalls keep track of "connections", if a peer behind NAT or a firewall wishes to receive incoming packets, he must keep the NAT/firewall mapping valid, by periodically sending keepalive packets. This is called persistent keepalives. When this option is enabled, a keepalive packet is sent to the server endpoint once every interval seconds. A sensible interval that works with a wide variety of firewalls is 25 seconds. Setting it to 0 turns the feature off, which is the default, since most users will not need this, and it makes WireGuard slightly more chatty. This feature may be specified by adding the PersistentKeepalive = field to a peer in the configuration file, or setting persistent-keepalive at the command line. If you don't need this feature, don't enable it. But if you're behind NAT or a firewall and you want to receive incoming connections long after network traffic has gone silent, this option will keep the "connection" open in the eyes of NAT.

## Installation

-   https://www.wireguard.com/install/

### WireGuard for Windows

-   https://download.wireguard.com/windows-client/

### Ubuntu ≥ 19.10

```shell
sudo apt install wireguard
```

### Ubuntu ≤ 19.04

```shell
sudo add-apt-repository ppa:wireguard/wireguard
sudo apt-get update
sudo apt-get install wireguard
```

### Debian

```shell
echo "deb http://deb.debian.org/debian/ unstable main" | sudo tee /etc/apt/sources.list.d/unstable.list
printf 'Package: *\nPin: release a=unstable\nPin-Priority: 90\n' | sudo tee /etc/apt/preferences.d/limit-unstable
sudo apt update
sudo apt install wireguard

sudo apt dist-upgrade
sudo apt install linux-headers-amd64
sudo apt-get install --reinstall wireguard-dkms
```

## Quick Start

### Key Generation

```shell
umask 077

wg genkey > privatekey
wg pubkey < privatekey > publickey

#  do this all at once
wg genkey | tee privatekey | wg pubkey > publickey
```

### Server configuration

    # sudo sysctl net.ipv4.conf.all.forwarding=1
    # sudo sysctl net.ipv6.conf.all.forwarding=1
    # sudo vi /etc/wireguard/wg0.conf
    [Interface]
    PrivateKey = YMKtNQbqzIL37gFZPue1rxITRWfwb828bvFqpbiMZ00=
    ListenPort = 24618

    # OP-7020-01
    [Peer]
    PublicKey = YMKtNQbqzIL37gFZPue1rxITRWfwb828bvFqpbiMZ00=
    AllowedIPs = 192.168.20.2/32
    PersistentKeepalive = 25

### Client configuration

    # sudo sysctl net.ipv4.conf.all.forwarding=1
    # sudo sysctl net.ipv6.conf.all.forwarding=1
    # sudo vi /etc/wireguard/wg0.conf
    # sudo wg setconf wg0 /etc/wireguard/wg0.conf
    # qrencode -t png -o ipv4-tunnel-by-ipv4.png < ipv4-tunnel-by-ipv4.conf
    [Interface]
    PrivateKey = YMKtNQbqzIL37gFZPue1rxITRWfwb828bvFqpbiMZ00=
    ListenPort = 6474

    [Peer]
    PublicKey = 8IFM9l26YZF0Xs3QPK88j87iE/gUrTRyi2n9I5WjH2c=
    Endpoint = 26.73.12.47:24618
    AllowedIPs = 0.0.0.0/0
    PersistentKeepalive = 25

### Command-line Interface

#### Server side

##### Server route

```shell
iptables -L -n -v

iptables -t filter -P INPUT ACCEPT
iptables -t filter -P FORWARD ACCEPT
iptables -t filter -P OUTPUT ACCEPT
iptables -t filter -F
iptables -t filter -X
iptables -t nat -F
iptables -t nat -X

cat | iptables-restore << EOF
*filter
:INPUT ACCEPT
:FORWARD ACCEPT
:OUTPUT ACCEPT
-A FORWARD -i wg0 -j ACCEPT
COMMIT
*nat
:PREROUTING ACCEPT
:INPUT ACCEPT
:OUTPUT ACCEPT
:POSTROUTING ACCEPT
-A POSTROUTING -p esp -j RETURN
-A POSTROUTING -s 10.20.30.0/24  -o ens3 -j MASQUERADE
-A POSTROUTING -s 10.20.40.0/24  -o ens3 -j MASQUERADE
-A POSTROUTING -s 192.168.0.0/16 -o ens3 -j MASQUERADE
COMMIT
EOF
```

##### wg-quick

```shell
# sudo wg setconf wg0 /etc/wireguard/wg0.conf
# sudo systemctl enable wg-quick@wg0

sudo ip link add dev wg0 type wireguard
sudo ip addr add dev wg0 192.168.20.1/24
sudo ip link set dev wg0 up mtu 1420
sudo ip link set up dev wg0

sudo wg setconf wg0 /etc/wireguard/wg0.conf

sudo wg show
sudo wg showconf wg0
```

#### Client side

```shell
# sudo wg setconf wg0 /etc/wireguard/wg0.conf
# sudo systemctl enable wg-quick@wg0

sudo ip link add dev wg0 type wireguard
sudo ip addr add dev wg0 192.168.20.2/24
sudo ip link set dev wg0 up mtu 1420
sudo ip link set up dev wg0

sudo wg setconf wg0 /etc/wireguard/wg0.conf

sudo wg show
sudo wg showconf wg0
```

```shell
$ sudo wg-quick up wg0
[#] ip link add wg0 type wireguard
[#] wg setconf wg0 /dev/fd/63
[#] ip -4 address add 192.168.20.3/32 dev wg0
[#] ip link set mtu 1420 up dev wg0
[#] wg set wg0 fwmark 51820
[#] ip -4 route add 0.0.0.0/0 dev wg0 table 51820
[#] ip -4 rule add not fwmark 51820 table 51820
[#] ip -4 rule add table main suppress_prefixlength 0
[#] sysctl -q net.ipv4.conf.all.src_valid_mark=1
[#] nft -f /dev/fd/63
```

```shell
$ ping -OD 192.168.20.1
PING 192.168.20.1 (192.168.20.1) 56(84) bytes of data.
[1584754074.675552] 64 bytes from 192.168.20.1: icmp_seq=1 ttl=64 time=176 ms
[1584754075.677335] 64 bytes from 192.168.20.1: icmp_seq=2 ttl=64 time=177 ms
[1584754076.680005] 64 bytes from 192.168.20.1: icmp_seq=3 ttl=64 time=178 ms
[1584754077.679841] 64 bytes from 192.168.20.1: icmp_seq=4 ttl=64 time=176 ms
[1584754078.682989] 64 bytes from 192.168.20.1: icmp_seq=5 ttl=64 time=177 ms
[1584754079.683023] 64 bytes from 192.168.20.1: icmp_seq=6 ttl=64 time=176 ms
[1584754080.696269] 64 bytes from 192.168.20.1: icmp_seq=7 ttl=64 time=188 ms
[1584754081.684687] 64 bytes from 192.168.20.1: icmp_seq=8 ttl=64 time=176 ms
[1584754082.687258] 64 bytes from 192.168.20.1: icmp_seq=9 ttl=64 time=178 ms
^C
--- 192.168.20.1 ping statistics ---
9 packets transmitted, 9 received, 0% packet loss, time 17ms
rtt min/avg/max/mdev = 175.802/177.997/188.254/3.769 ms
```

#### Client route

```shell
sudo ip route add 192.168.20.0/24 via 192.168.20.2 dev wg0

sudo ip route add 26.73.12.47/32 via 192.168.1.1
sudo ip route add 8.8.8.8/32 via 192.168.20.2 dev wg0
sudo ip route add 8.8.4.4/32 via 192.168.20.2 dev wg0
sudo ip route add 128.0.0.0/1 via 192.168.20.2 dev wg0
sudo ip route add 0.0.0.0/1 via 192.168.20.2 dev wg0

sudo ip route del 26.73.12.47/32
sudo ip route del 8.8.8.8/32
sudo ip route del 8.8.4.4/32
sudo ip route del 0.0.0.0/1
sudo ip route del 128.0.0.0/1
```
