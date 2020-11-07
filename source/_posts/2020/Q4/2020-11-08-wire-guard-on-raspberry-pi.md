---
title: WireGuard on Raspberry Pi
description: WireGuard on Raspberry Pi
date: 2020-11-08 22:41:09
tags:
    - Operating system
    - Linux
categories: [Operating system, Linux]
permalink: wire-guard-on-raspberry-pi
---

# WireGuard on Raspberry Pi

## Kernel Requirements

WireGuard requires Linux kernel version ≥3.10, with the following configuration options, which are likely already configured in your kernel, especially if you're installing via distribution packages.

-   **CONFIG_NET** for basic networking support
-   **CONFIG_INET** for basic IP support
-   **CONFIG_NET_UDP_TUNNEL** for sending and receiving UDP packets
-   **CONFIG_CRYPTO_ALGAPI** for crypto_xor

Raspberry Pi use Linux kernel version 5.4:

```shell
# cat /proc/version
Linux version 5.4.72+ (dom@buildbot) (gcc version 9.3.0 (Ubuntu 9.3.0-17ubuntu1~20.04)) #1356 Thu Oct 22 13:56:00 BST 2020

# dpkg -l | grep raspberrypi-kernel
ii  raspberrypi-kernel          1.20201022-1    armhf   Raspberry Pi bootloader
ii  raspberrypi-kernel-headers  1.20201022-1    armhf   Header files for the Raspberry Pi Linux kernel
```

## Step 1: Install the toolchain

```shell
$ sudo apt-get install -y raspberrypi-kernel raspberrypi-kernel-headers \
    iptables nftables \
    libelf-dev build-essential pkg-config
```

## Step 2: Grab the code

```shell
$ git clone https://git.zx2c4.com/wireguard-linux-compat
$ git clone https://git.zx2c4.com/wireguard-tools
```

## Step 3: Compile and install the module

```shell
$ make -C wireguard-linux-compat/src -j$(nproc)
$ sudo make -C wireguard-linux-compat/src install

make: Entering directory '/tmp/wireguard-linux-compat/src'
  INSTALL /tmp/wireguard-linux-compat/src/wireguard.ko
  DEPMOD  5.4.72+
depmod -b "/" -a 5.4.72+

$ sudo modprobe wireguard
$ dmesg

[38626.469261] wireguard: WireGuard 1.0.20200908-3-gda5646f loaded. See www.wireguard.com for information.
[38626.469283] wireguard: Copyright (C) 2015-2019 Jason A. Donenfeld <Jason@zx2c4.com>. All Rights Reserved.
```

## Step 4: Compile and install the wg(8) tool

```shell
$ make -C wireguard-tools/src -j$(nproc)
$ sudo make -C wireguard-tools/src install

'wg' -> '/usr/bin/wg'
'man/wg.8' -> '/usr/share/man/man8/wg.8'
'completion/wg.bash-completion' -> '/usr/share/bash-completion/completions/wg'
'wg-quick/linux.bash' -> '/usr/bin/wg-quick'
'man/wg-quick.8' -> '/usr/share/man/man8/wg-quick.8'
'completion/wg-quick.bash-completion' -> '/usr/share/bash-completion/completions/wg-quick'
'systemd/wg-quick.target' -> '/lib/systemd/system/wg-quick.target'
'systemd/wg-quick@.service' -> '/lib/systemd/system/wg-quick@.service'
```

## Step 5: Key generation

```shell
$ wg genkey | tee wg-privatekey | wg pubkey > wg-publickey
```

## Step 6: Local configuration

```shell
$ sudo vi /etc/wireguard/wg0.conf

[Interface]
PrivateKey = 4PlDEwG+60kuuEfDguXS/r7k+wUP4gQzeVCgJXLUOlI=
Address = 192.168.20.5/24
ListenPort = 64741
DNS = 8.8.8.8,8.8.4.4

[Peer]
PublicKey = r0SXjeVxqcTy7p1Ikau5XGd5eGPHciUejrTGtUL4Vh0=
AllowedIPs = 0.0.0.0/0
Endpoint = 1.2.3.4:64741
PersistentKeepalive = 25
```

## Step 7: Peer configuration

```shell
$ sudo vi /etc/wireguard/wg0.conf

[Interface]
PrivateKey = uC40eSTd0GgKN9KWN4FEMlIDqx08hvf3m4HkPm7yNU0=
ListenPort = 64741
Address = 192.168.20.1/24

# Raspberry Pi Zero
[Peer]
PublicKey = RC/jDAB2ZkOon76ItDgYYBcFEhWByvjENk71cg8LoiM=
AllowedIPs = 192.168.20.5/32
PersistentKeepalive = 25
```

## Step 8: Set up the peer interface

```shell
# wg-quick up wg0

sysctl net.ipv4.tcp_fin_timeout=15
sysctl net.ipv4.tcp_keepalive_intvl=75
sysctl net.ipv4.ip_forward=1
sysctl net.ipv6.conf.all.forwarding=1
sysctl net.ipv4.conf.ens3.forwarding=1
sysctl net.ipv6.conf.ens3.forwarding=1

iptables -t filter -P INPUT ACCEPT
iptables -t filter -P FORWARD ACCEPT
iptables -t filter -P OUTPUT ACCEPT
iptables -t filter -F
iptables -t filter -X
iptables -t nat -F
iptables -t nat -X

$ cat << EOF | sudo iptables-restore
*filter
:INPUT ACCEPT
:FORWARD ACCEPT
:OUTPUT ACCEPT
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

## Step 9: Connect to peers

```shell
$ sudo wg-quick up wg0
$ sudo wg

$ sudo sysctl -w net.ipv4.ip_forward=1
$ sudo sysctl -w net.ipv6.conf.all.forwarding=1
$ sudo ip rule list
$ sudo ip route show table 0
$ sudo ip route show table all
```
