---
title: GPT Partition
excerpt: GPT Partition
date: 2020-01-29 20:24:27
tags:
  - Linux
categories: [Operating system, Linux]
---

# GPT Partition

## Debian 10 on GPT Partition

```bash
$ sudo fdisk -l /dev/sdb
Disk /dev/sdb: 465.8 GiB, 500107862016 bytes, 976773168 sectors
Disk model: ST500DM002-1BD14
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 3E509285-A069-48F4-AF14-0A38B272E3D6

Device         Start       End   Sectors   Size Type
/dev/sdb1       2048   1050623   1048576   512M EFI System
/dev/sdb2    1050624   2097151   1046528   511M Linux filesystem
/dev/sdb3    2097152 538968063 536870912   256G Linux LVM
/dev/sdb4  538968064 976773134 437805071 208.8G Microsoft basic data
```

## RHEL 8 on GPT Partition

```bash
# fdisk -l /dev/sda
Disk /dev/sda: 4 TiB, 4398046511104 bytes, 8589934592 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 8F849392-8906-4965-AA07-306C3E1702D8

Device       Start        End    Sectors  Size Type
/dev/sda1     2048    1230847    1228800  600M EFI System
/dev/sda2  1230848    3327999    2097152    1G Linux filesystem
/dev/sda3  3328000 8589932543 8586604544    4T Linux LVM
```

## Windows 10 on GPT Partition

```bash
$ sudo fdisk -l /dev/sda
Disk /dev/sda: 3.7 TiB, 4000787030016 bytes, 7814037168 sectors
Disk model: ST4000DM004-2CV1
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: EE646948-38BD-4F52-A564-72B94D20B500

Device          Start        End    Sectors   Size Type
/dev/sda1        2048    1023999    1021952   499M Windows recovery environment
/dev/sda2     1024000    1228799     204800   100M EFI System
/dev/sda3     1228800    1261567      32768    16M Microsoft reserved
/dev/sda4     1261568 1072511585 1071250018 510.8G Microsoft basic data
/dev/sda5  1072513024 1073741823    1228800   600M Windows recovery environment
/dev/sda6  1073743872 5368711167 4294967296     2T Microsoft basic data
```

## Disable UTC and use Local Time in Linux

```bash
sudo timedatectl set-local-rtc 1 --adjust-system-clock
timedatectl
```
