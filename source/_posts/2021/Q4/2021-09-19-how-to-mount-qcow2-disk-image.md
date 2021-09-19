---
title: How to Mount QEMU Disk Image
description: How to Mount QEMU Disk Image
date: 2021-09-19 13:32:42
tags:
  - Operating system
  - Windows
categories:
  - Operating system
  - Windows
permalink: how-to-mount-qemu-disk-image
---

# How to Mount QEMU Disk Image

## Load NBD Kernel Module on The Host

```bash
$ sudo modprobe nbd max_part=16 nbds_max=16

$ ls /dev/nbd*
/dev/nbd0  /dev/nbd10  /dev/nbd12  /dev/nbd14  /dev/nbd2  /dev/nbd4  /dev/nbd6  /dev/nbd8
/dev/nbd1  /dev/nbd11  /dev/nbd13  /dev/nbd15  /dev/nbd3  /dev/nbd5  /dev/nbd7  /dev/nbd9
```

## Connect the QEMU Disk Image as NBD

```bash
$ qemu-img convert -f raw -O qcow2 src.raw dst.qcow2

$ qemu-img resize dst.qcow2 48G
Image resized.

$ sudo qemu-nbd --connect=/dev/nbd0 my.qcow2
```

## Find The Desired NBD Partition

```bash
$ dmesg | grep nbd
[92223.517473]  nbd0: p1 p12 p13 p14 p15
[92223.518662]  nbd0: p1 p12 p13 p14 p15

$ sudo fdisk /dev/nbd0 -l
```

## Resize or Fix The Partition and File System

```bash
$ sudo sgdisk -e /dev/nbd0

$ sudo e2fsck -f /dev/nbd0p1
$ sudo resize2fs /dev/nbd0p1
```

## Mount The Desired Partition

```bash
$ sudo mount /dev/nbd0p1 /mnt/

$ mount | grep nbd
/dev/nbd0p1 on /mnt type ext4 (rw,relatime)
```

If you encountered error `mount: special device /dev/nbd0p1 does not exist`, or similar partition table errors, please rescan partitions, tell the kernel about the latest information of partitions:

```bash
$ sudo partx -a /dev/nbd0

$ sudo partx -s /dev/nbd0

$ partx --help

Usage:
 partx [-a|-d|-s|-u] [--nr <n:m> | <partition>] <disk>

Tell the kernel about the presence and numbering of partitions.

Options:
 -a, --add            add specified partitions or all of them
 -d, --delete         delete specified partitions or all of them
 -u, --update         update specified partitions or all of them
 -s, --show           list partitions
...
```

## Do Something in The Desired Partition

```bash
$ ls /mnt/
bin  boot  dev  etc  home  lib  lost+found  media  mnt  opt  proc  root  run  sbin  snap  srv  sys  tmp  usr  var

$ sudo du -ms /mnt/
3127    /mnt/
```

## Unmount File System and Disconnect NBD

```bash
$ sudo umount /mnt

$ sudo qemu-nbd --disconnect /dev/nbd0
/dev/nbd0 disconnected

$ sudo rmmod nbd

$ dmesg | grep nbd
[92223.517473]  nbd0: p1 p12 p13 p14 p15
[92223.518662]  nbd0: p1 p12 p13 p14 p15
[92412.338838]  nbd0: p1 p12 p13 p14 p15
[92534.871085] EXT4-fs (nbd0p1): mounted filesystem with ordered data mode. Opts: (null)
[92794.269063] block nbd0: NBD_DISCONNECT
[92794.269077] block nbd0: Disconnected due to user request.
[92794.269078] block nbd0: shutting down sockets
```
