---
title: APT configuration and Preference
description: APT configuration and Preference
date: 2020-02-29 14:23:31
tags:
  - Linux
categories: [Operating system, Linux]
permalink: apt-config-pref
---

# APT configuration and Preference

## APT configuration file

- /etc/apt/apt.conf
- /etc/apt/apt.conf.d/

```shell
cat << EOF | sudo tee /etc/apt/apt.conf.d/zz-10-install
APT::Install-Recommends "0";
APT::Install-Suggests "0";

#Acquire::http { Proxy "http://www.example.com:8080"; };
#Acquire::http { Proxy "socks5h://127.0.0.1:10080"; };

Acquire::Languages "none";
Acquire::GzipIndexes "true";
Acquire::CompressionTypes::Order { "xz"; "gz"; };
EOF
```

## APT Configuration Query program

```shell
apt-config dump
```

##  Preference control file for APT

- /etc/apt/preferences
- /etc/apt/preferences.d/

### stable

```shell
cat << EOF | sudo tee /etc/apt/preferences.d/10-stable
Package: *
Pin: release a=stable
Pin-Priority: 500
EOF
```

### testing

```shell
cat << EOF | tee /etc/apt/preferences.d/20-testing
Package: *
Pin: release a=testing
Pin-Priority: 400
EOF
```

### unstable

```shell
cat << EOF | tee /etc/apt/preferences.d/30-unstable
Package: *
Pin: release a=unstable
Pin-Priority: 90
EOF
```

### experimental

```shell
cat << EOF | tee /etc/apt/preferences.d/40-experimental
Package: *
Pin: release a=experimental
Pin-Priority: 1
EOF
```

## Shows available versions of installed packages

```shell
apt-get install -y --no-install-recommends apt-show-versions

apt-show-versions -u -b
```

## Install/Upgrading Testing Packages

```shell
apt-get -t testing install nginx-full

apt-get install -y --no-install-recommends `apt-show-versions -u -b | grep testing`
```

## Show policy settings

```shell
apt-cache policy wireguard-dkms

apt-get -t unstable install wireguard
```

## List of configured APT data sources

- /etc/apt/sources.list
- /etc/apt/sources.list.d

```
deb http://deb.debian.org/debian/ stable main contrib non-free
deb http://deb.debian.org/debian/ stable-updates main contrib non-free
deb http://deb.debian.org/debian/ stable-proposed-updates main contrib non-free
deb http://deb.debian.org/debian/ stable-backports main contrib non-free

deb http://deb.debian.org/debian/ testing main contrib non-free
deb http://deb.debian.org/debian/ testing-updates main contrib non-free
deb http://deb.debian.org/debian/ testing-proposed-updates main contrib non-free

deb http://deb.debian.org/debian/ unstable main contrib non-free
```
