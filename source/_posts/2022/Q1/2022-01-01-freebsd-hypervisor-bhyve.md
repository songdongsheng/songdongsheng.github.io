---
title: 'FreeBSD Hypervisor: bhyve'
excerpt: 'FreeBSD Hypervisor: bhyve'
date: 2022-01-01 22:03:27
tags:
  - Operating system
  - FreeBSD
categories:
  - Operating system
  - FreeBSD
---

# FreeBSD Hypervisor: bhyve

## Introduction

The bhyve BSD-licensed hypervisor became part of the base system with FreeBSD 10.0-RELEASE. This hypervisor supports a number of guests, including FreeBSD, OpenBSD, and many Linux® distributions.

## Preparing the Host

The first step to creating a virtual machine in bhyve is configuring the host system. First, load the bhyve kernel module:

```shell
# kldload vmm
```

Then, create a tap interface for the network device in the virtual machine to attach to. In order for the network device to participate in the network, also create a bridge interface containing the tap interface and the physical interface as members. In this example, the physical interface is _igb0_:

```shell
# ifconfig tap0 create
# sysctl net.link.tap.up_on_open=1
net.link.tap.up_on_open: 0 -> 1
# ifconfig bridge0 create
# ifconfig bridge0 addm ue0 addm tap0
# ifconfig bridge0 up

service bhyve start
service bhyve status
service bhyve stop
```

## Creating a FreeBSD Guest

Download an installation image of FreeBSD to install:

```shell
# pkg install -y aria2 lzma vm-bhyve bhyve-rc
# pkg info -l aria2 | grep 'bin/'
    /usr/local/bin/aria2c
# aria2c -c -m 0 -t 5 -j 8 -s 8 -x 8 -k 1m --file-allocation=falloc https://download.freebsd.org/ftp/releases/ISO-IMAGES/13.1/FreeBSD-13.1-RELEASE-amd64-disc1.iso
    Download Results:
    gid   |stat|avg speed  |path/URI
    ======+====+===========+=======================================================
    a31284|OK  |   699KiB/s|/root/vm/FreeBSD-13.1-RELEASE-amd64-disc1.iso

# aria2c -c -m 0 -t 5 -j 8 -s 8 -x 8 -k 1m --file-allocation=falloc https://download.freebsd.org/ftp/releases/VM-IMAGES/13.1-RELEASE/amd64/Latest/FreeBSD-13.1-RELEASE-amd64.qcow2.xz
    Download Results:
    gid   |stat|avg speed  |path/URI
    ======+====+===========+=======================================================
    0ed032|OK  |   1.2MiB/s|/root/vm/FreeBSD-13.1-RELEASE-amd64.qcow2.xz

# xz -d -k -v FreeBSD-13.1-RELEASE-amd64.qcow2.xz

# fetch https://download.freebsd.org/ftp/releases/ISO-IMAGES/13.1/FreeBSD-13.1-RELEASE-amd64-disc1.iso
# fetch https://download.freebsd.org/ftp/releases/VM-IMAGES/13.1-RELEASE/amd64/Latest/FreeBSD-13.1-RELEASE-amd64.qcow2.xz
```

Create a file to use as the virtual disk for the guest machine. Specify the size and name of the virtual disk:

```shell
# truncate -s 16G guest.img
```

FreeBSD comes with an example script for running a virtual machine in bhyve. The script will start the virtual machine and run it in a loop, so it will automatically restart if it crashes. The script takes a number of options to control the configuration of the machine

This example starts the virtual machine in installation mode:

```shell
# sh /usr/share/examples/bhyve/vmrun.sh -c 2 -m 1024M -t tap0 -d guest.img -i -I FreeBSD-13.1-RELEASE-amd64-disc1.iso guestname
```

The virtual machine will boot and start the installer. After installing a system in the virtual machine, when the system asks about dropping in to a shell at the end of the installation, choose **Yes**.

Reboot the virtual machine. While rebooting the virtual machine causes bhyve to exit, the vmrun.sh script runs `bhyve` in a loop and will automatically restart it. When this happens, choose the reboot option from the boot loader menu in order to escape the loop. Now the guest can be started from the virtual disk:

```shell
# sh /usr/share/examples/bhyve/vmrun.sh -c 2 -m 1024M -t tap0 -d guest.img guestname
```

## Managing Virtual Machines

A device node is created in /dev/vmm for each virtual machine. This allows the administrator to easily see a list of the running virtual machines:

```shell
# ls -al /dev/vmm
```

A specified virtual machine can be destroyed using `bhyvectl`:

```shell
# bhyvectl --destroy --vm=guestname
```

## Persistent Configuration

In order to configure the system to start bhyve guests at boot time, the following configurations must be made in the specified files:

1. /etc/sysctl.conf
    ```shell
    net.link.tap.up_on_open=1
    ```

2. /etc/rc.conf
    ```shell
    hostname="freebsd-amd64-01"
    ifconfig_DEFAULT="DHCP"
    ipv6_enable="YES"
    ipv6_activate_all_interfaces="YES"
    sshd_enable="YES"
    ntpdate_enable="YES"
    sendmail_enable="NONE"
    sendmail_submit_enable="NO"
    sendmail_outbound_enable="NO"
    sendmail_msp_queue_enable="NO"
    growfs_enable="YES"

    cloned_interfaces="tap0 tap1 bridge0"
    ifconfig_bridge0="addm ue0 addm tap0 addm tap1"

    kld_list="nmdm vmm"

    bhyve_enable="YES"
    bhyve_profiles="virt1 virt2"
    bhyve_virt1_diskdev="/dev/zvol/tank/bhyve/virt1"
    bhyve_virt2_tapdev="tap1"
    bhyve_virt2_diskdev="/dev/zvol/tank/bhyve/virt2"
    bhyve_virt2_memsize="8192"
    bhyve_virt2_ncpu="4"

    vm_enable="YES"
    #vm_dir="zfs:pool/bhyve/virt"
    vm_dir="/var/bhyve/virt"
    ```

## Reference

+ [bhyve - FreeBSD Wiki](https://wiki.freebsd.org/bhyve)
+ [FreeBSD as a Host with bhyve](https://docs.freebsd.org/en/books/handbook/virtualization/#virtualization-host-bhyve)
+ [FreeBSD 13.1 Release Checksum Signatures](https://www.freebsd.org/releases/13.1R/signatures/)
+ [FreeBSD 13.1 ISO Images](https://download.freebsd.org/ftp/releases/ISO-IMAGES/13.1/)
+ [FreeBSD 13.1 VM Images](https://download.freebsd.org/ftp/releases/VM-IMAGES/13.1-RELEASE/)
+ [FreeBSD Manual Pages - rc](https://www.freebsd.org/cgi/man.cgi?query=rc)
+ [FreeBSD Manual Pages - rc.conf](https://www.freebsd.org/cgi/man.cgi?query=rc.conf)
+ [FreeBSD Manual Pages - sysctl.conf](https://www.freebsd.org/cgi/man.cgi?query=sysctl.conf)
+ [FreeBSD Manual Pages - bhyve](https://www.freebsd.org/cgi/man.cgi?query=bhyve)
+ [FreeBSD Manual Pages - bhyve_config](https://www.freebsd.org/cgi/man.cgi?query=bhyve_config)
