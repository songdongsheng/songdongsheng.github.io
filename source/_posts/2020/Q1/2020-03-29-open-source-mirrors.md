---
title: Open source mirrors
description: Open source mirrors
date: 2020-03-29 16:30:07
tags:
  - Programming
  - HTTP
categories: [Programming, HTTP]
permalink: open-source-mirrors
---

# Open source mirrors

## Mirror lists

- https://developer.aliyun.com/mirror/
- https://mirrors.huaweicloud.com/
- https://mirrors.163.com/
- https://mirrors.ustc.edu.cn/
- https://gems.ruby-china.com/

## Operating System

### Alpine

```bash
# https://mirrors.huaweicloud.com/alpine/v3.11/releases/x86_64/alpine-standard-3.11.5-x86_64.iso

cp -a /etc/apk/repositories /etc/apk/repositories.bak
sed -i "s@http://dl-cdn.alpinelinux.org/@https://mirrors.huaweicloud.com/@g" /etc/apk/repositories
```

### Debian

```ini
# https://mirrors.huaweicloud.com/debian-cd/current/amd64/iso-cd/debian-10.3.0-amd64-netinst.iso
# https://mirrors.huaweicloud.com/debian-cd/current/arm64/iso-cd/debian-10.3.0-arm64-netinst.iso
# https://mirrors.huaweicloud.com/debian-cd/current/s390x/iso-cd/debian-10.3.0-s390x-netinst.iso

deb https://mirrors.huaweicloud.com/debian/ buster main contrib non-free
deb https://mirrors.huaweicloud.com/debian/ buster-updates main contrib non-free
deb https://mirrors.huaweicloud.com/debian/ buster-proposed-updates main contrib non-free
deb https://mirrors.huaweicloud.com/debian/ buster-backports main contrib non-free
deb https://mirrors.huaweicloud.com/debian-security/ buster/updates main contrib non-free

#deb http://mirrors.163.com/debian/ buster main contrib non-free
#deb http://mirrors.163.com/debian/ buster-updates main contrib non-free
#deb http://mirrors.163.com/debian/ buster-proposed-updates main contrib non-free
#deb http://mirrors.163.com/debian/ buster-backports main contrib non-free
#deb http://mirrors.163.com/debian-security/ buster-backports main contrib non-free

#deb http://deb.debian.org/debian/ buster main contrib non-free
#deb http://deb.debian.org/debian/ buster-updates main contrib non-free
#deb http://deb.debian.org/debian/ buster-proposed-updates main contrib non-free
#deb http://deb.debian.org/debian/ buster-backports main contrib non-free
#deb http://security.debian.org/debian-security/ buster/updates main contrib non-free

#deb http://deb.debian.org/debian/ testing main contrib non-free
#deb http://deb.debian.org/debian/ testing-updates main contrib non-free
#deb http://deb.debian.org/debian/ testing-proposed-updates main contrib non-free

#deb http://deb.debian.org/debian/ unstable main contrib non-free
```


### Ubuntu

```ini
# https://mirrors.huaweicloud.com/ubuntu-cdimage/releases/18.04/release/ubuntu-18.04.4-server-amd64.iso
# https://mirrors.huaweicloud.com/ubuntu-cdimage/releases/18.04/release/ubuntu-18.04.4-server-arm64.iso
# https://mirrors.huaweicloud.com/ubuntu-cdimage/releases/18.04/release/ubuntu-18.04.4-server-s390x.iso
```

## Languages

### Java & maven

```xml
<mirror>
    <id>huaweicloud</id>
    <mirrorOf>*</mirrorOf>
    <url>https://mirrors.huaweicloud.com/repository/maven/</url>
</mirror>
```

### Node.js & npm

```bash
# https://mirrors.huaweicloud.com/nodejs/latest-v12.x/
# https://mirrors.huaweicloud.com/node-sass/?C=M&O=D

npm config set registry https://mirrors.huaweicloud.com/repository/npm/
npm config set disturl https://mirrors.huaweicloud.com/nodejs
npm config set sass_binary_site https://mirrors.huaweicloud.com/node-sass
npm cache clean -f
```

### Python & PyPI

```bash
pip install \
    --trusted-host https://mirrors.huaweicloud.com \
    -i https://mirrors.huaweicloud.com/repository/pypi/simple \
    <packages>
```

```ini
# ~/.pip/pip.conf
# C:\Users\<UserName>\pip\pip.ini
[global]
index-url = https://mirrors.huaweicloud.com/repository/pypi/simple
trusted-host = mirrors.huaweicloud.com
timeout = 120
```

### Ruby & RubyGems

```bash
gem update --system
gem sources -l

gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
gem sources -l
```


### Go & Modules

```bash
# https://proxy.golang.org
export GOPROXY=https://mirrors.aliyun.com/goproxy/
```
