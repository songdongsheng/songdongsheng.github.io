---
title: CentOS 8 Stream
excerpt: CentOS 8 Stream
date: 2020-02-08 12:31:5
tags:
  - Linux
categories: [Operating system, Linux]
---

## Using AppStream

    dnf module install nodejs:12/s2i

    module – They are set of packages
    nodejs – One of the module
    :12 – One of the Stream = Version
    /s2i – One of the profile.

## Listing all available modules in AppStream

    dnf module list

## Gathering module Information

    dnf module info rust-toolset
    dnf module info nodejs
    dnf module info --profile nodejs
    dnf module info --profile nodejs:10
    dnf module info --profile nodejs:12

## Selecting a stream before installation of packages

    dnf module enable nodejs
    dnf module enable python36
    dnf module disable nodejs

## Installing a module stream

    dnf install @nodejs:12/s2i
    dnf module install nodejs:12/s2i
    dnf module remove nodejs:12/s2i

## Installing a non-default stream of an application

    dnf module list postgresql
    dnf install @postgresql:12/server

## Switching to a later stream

    dnf distro-sync
    dnf upgrade
    dnf module reset module-name
    dnf module enable module-name:new-stream

## CentOS 8 stream repository

- https://mirrors.163.com/centos/8/BaseOS/x86_64/os/Packages/
- https://mirrors.163.com/centos/8/AppStream/x86_64/os/Packages/
- https://mirrors.163.com/centos/8/PowerTools/x86_64/os/Packages/
- https://mirrors.163.com/centos/8/extras/x86_64/os/Packages/
- https://download.fedoraproject.org/pub/epel/$releasever/Everything/$basearch

### /etc/yum.repos.d

    # dnf clean all
    # dnf makecache

    # dnf distro-sync
    # dnf upgrade
    # rpm -qa | grep ^centos             
    centos-release-8.1-1.1911.0.8.el8.x86_64
    centos-gpg-keys-8.1-1.1911.0.8.el8.noarch
    centos-repos-8.1-1.1911.0.8.el8.x86_64

    # vi /etc/yum.repos.d/CentOS-Base.repo
    [Base]
    name=CentOS-$releasever - Base
    baseurl=http://mirrors.aliyun.com/centos/8/BaseOS/$basearch/os/
    #baseurl=http://mirrors.163.com/centos/8/BaseOS/$basearch/os/
    #baseurl=http://mirror.centos.org/centos/8/BaseOS/$basearch/os/
    gpgcheck=1
    enabled=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial

    # vi /etc/yum.repos.d/CentOS-AppStream.repo
    [AppStream]
    name=CentOS-$releasever - AppStream
    baseurl=http://mirrors.aliyun.com/centos/8/AppStream/$basearch/os/
    #baseurl=http://mirrors.163.com/centos/8/AppStream/$basearch/os/
    #baseurl=https://mirror.centos.org/centos/8/AppStream/$basearch/os/
    gpgcheck=1
    enabled=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial

    # vi /etc/yum.repos.d/CentOS-PowerTools.repo
    [PowerTools]
    name=CentOS-$releasever - PowerTools
    baseurl=http://mirrors.aliyun.com/centos/8/PowerTools/$basearch/os/
    #baseurl=http://mirrors.163.com/centos/8/PowerTools/$basearch/os/
    #baseurl=http://mirror.centos.org/centos/8/PowerTools/$basearch/os/
    gpgcheck=1
    enabled=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial

    # vi /etc/yum.repos.d/CentOS-Extras.repo
    [Extra]
    name=CentOS-$releasever - Extra
    baseurl=http://mirrors.aliyun.com/centos/8/extras/$basearch/os/
    #baseurl=http://mirrors.163.com/centos/8/extras/$basearch/os/
    #baseurl=http://mirror.centos.org/centos/8/extras/$basearch/os/
    gpgcheck=1
    enabled=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial

    # vi /etc/yum.repos.d/CentOS-Epel.repo
    [EPEL]
    name=CentOS-$releasever - EPEL
    baseurl=http://mirrors.aliyun.com/epel/$releasever/Everything/
    #baseurl=http://download.fedoraproject.org/pub/epel/$releasever/Everything/$basearch
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-8
