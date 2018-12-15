---
title: Setup SSH SOCKS5 proxy for Chrome
description: Setup SSH SOCKS5 proxy for Chrome
date: 2020-01-28 22:37:32
tags:
  - Linux
categories: [Operating system, Linux]
permalink: setup-ssh-socks5-proxy-for-chrome
---

# Setup SSH SOCKS5 proxy for Chrome

## SSH server config

```bash
vi /etc/ssh/sshd_config
    PermitRootLogin yes
    Compression no
    TCPKeepAlive no
    ClientAliveInterval 15
    ClientAliveCountMax 3
```

## SSH client config

```bash
vi /etc/ssh/ssh_config
    ConnectTimeout 5
    Compression no
    ExitOnForwardFailure yes
    TCPKeepAlive no
    ServerAliveInterval 12
    ServerAliveCountMax 3
```

## Setup SSH SOCKS5 proxy

```bash
while true; do
    date
    ssh -4 -D 10080 -N -p 22 -o "ServerAliveInterval 12" root@example.com
  # ssh -6 -D 10080 -N -p 22 -o "ServerAliveInterval 12" root@example.com
    echo
done

# -4      Forces ssh to use IPv4 addresses only.
# -6      Forces ssh to use IPv6 addresses only.
# -C      Requests compression of all data
# -p port Port to connect to on the remote host.
# -f      Requests ssh to go to background just before command execution.
# -q      Quiet mode. Causes most warning and diagnostic messages to be suppressed.
# -N      Do not execute a remote command. This is useful for just forwarding ports.
# -D [bind_address:]port Specifies a local “dynamic” application-level port forwarding.
# -L [bind_address:]port:host:hostport Specifies that connections to the given local host are to be forwarded to the given host and hostport.
```

## Verify socks5 proxy

```bash
curl --socks5 127.0.0.1:10080 https://www.google.com
```

## Running apt-get with proxy

sudo vi /etc/apt/apt.conf.d/zz-10-install

```perl
APT::Install-Recommends "0";
APT::Install-Suggests "0";

#Acquire::http { Proxy "http://www.example.com:8080"; };
Acquire::http { Proxy "socks5h://127.0.0.1:10080"; };
```

## Check Chrome
```bash
$ google-chrome --version
Google Chrome 79.0.3945.130
```

## Running Chrome with proxy

```bash
google-chrome --proxy-server="socks5://localhost:10080"
```

## Running Chrome stay incognito

```bash
google-chrome --incognito --proxy-server="socks5://localhost:10080"
```
