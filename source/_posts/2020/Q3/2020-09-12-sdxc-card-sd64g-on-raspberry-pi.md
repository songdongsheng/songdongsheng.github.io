---
title: SDXC card SD64G on Raspberry Pi
description: SDXC card SD64G on Raspberry Pi
date: 2020-09-12 17:12:41
tags:
    - Operating system
    - Linux
categories: [Operating system]
permalink: sdxc-card-sd64g-on-raspberry-pi
---

# SDXC card SD64G on Raspberry Pi

## dmesg - mmc

```shell
$ dmesg  | grep mmc
[    1.681001] sdhost-bcm2835 20202000.mmc: could not get clk, deferring probe
[    2.583924] mmc0: sdhost-bcm2835 loaded - DMA enabled (>1)
[    2.675452] mmc0: host does not support reading read-only switch, assuming write-enable
[    2.691849] mmc0: new high speed SDXC card at address 5048
[    2.701678] mmcblk0: mmc0:5048 SD64G 58.0 GiB
[    2.712564]  mmcblk0: p1 p2
[    2.737917] EXT4-fs (mmcblk0p2): INFO: recovery required on readonly filesystem
[    2.748359] EXT4-fs (mmcblk0p2): write access will be enabled during recovery
[    2.888278] EXT4-fs (mmcblk0p2): recovery complete
[    2.898848] EXT4-fs (mmcblk0p2): mounted filesystem with ordered data mode. Opts: (null)
[   12.148874] EXT4-fs (mmcblk0p2): re-mounted. Opts: (null)
```

## lsblk

```shell
$ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
mmcblk0     179:0    0   58G  0 disk
├─mmcblk0p1 179:1    0  256M  0 part /boot
└─mmcblk0p2 179:2    0 57.8G  0 part /
```

## fdisk

```shell
$ sudo fdisk -l /dev/mmcblk0
Disk /dev/mmcblk0: 58 GiB, 62264442880 bytes, 121610240 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x8babfcf3

Device         Boot  Start       End   Sectors  Size Id Type
/dev/mmcblk0p1        8192    532479    524288  256M  c W95 FAT32 (LBA)
/dev/mmcblk0p2      532480 121610239 121077760 57.8G 83 Linux
```

## parted

```shell
pi@raspberrypi:~ $ sudo parted -l /dev/mmcblk0
Model: SD SD64G (sd/mmc)
Disk /dev/mmcblk0: 62.3GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags:

Number  Start   End     Size    Type     File system  Flags
 1      4194kB  273MB   268MB   primary  fat32        lba
 2      273MB   62.3GB  62.0GB  primary  ext4
```
