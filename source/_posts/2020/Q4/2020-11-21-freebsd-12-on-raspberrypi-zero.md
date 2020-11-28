---
title: FreeBSD 12 on Raspberry Pi Zero
description: FreeBSD 12 on Raspberry Pi Zero
date: 2020-11-21 23:19:03
tags:
    - Operating system
    - FreeBSD
categories: [Operating system, FreeBSD]
permalink: freebsd-12-on-raspberrypi-zero
---

# FreeBSD 12 on Raspberry Pi Zero

## Download FreeBSD image

```shell
$ aria2c https://download.freebsd.org/ftp/releases/ISO-IMAGES/12.2/FreeBSD-12.2-RELEASE-arm-armv6-RPI-B.img.xz

FREEBSD_STABLE_ROOT=https://download.freebsd.org/ftp/snapshots/arm/armv6/ISO-IMAGES/12.2
FREEBSD_STABLE_FILE=`curl -sS ${FREEBSD_STABLE_ROOT}/ | grep -oP "(FreeBSD-.*?-STABLE-arm-armv6-RPI-B-.*?xz)" | tail -1`
aria2c ${FREEBSD_STABLE_ROOT}/${FREEBSD_STABLE_FILE}
```

## Write image to SDXC card

```shell
$ cat FreeBSD-12.2-STABLE-arm-armv6-RPI-B-20201119-r367792.img.xz | xz -cd | sudo dd of=/dev/sdb bs=4M oflag=sync status=progress; time sync
```

## Booting Raspberry Pi

```shell
U-Boot 2020.10 (Nov 21 2020 - 14:02:49 +0000)

DRAM:  448 MiB
RPI Zero (0x900093)
MMC:   mmc@7e300000: 1
Loading Environment from FAT... In:    serial
Out:   vidconsole
Err:   vidconsole
Net:   No ethernet found.
starting USB...
Bus usb@7e980000: USB DWC2
scanning bus usb@7e980000 for devices...
      USB device not accepting new address (error=0)
2 USB Device(s) found
       scanning usb for storage devices... 0 Storage Device(s) found
Hit any key to stop autoboot:  0
MMC Device 0 not found
no mmc device at slot 0
switch to partitions #0, OK
mmc1 is current device
Scanning mmc 1:1...
Found EFI removable media binary efi/boot/bootarm.efi
libfdt fdt_check_header(): FDT_ERR_BADMAGIC
Scanning disk mmc@7e300000.blk...
** Unrecognized filesystem type **
Found 3 disks
No EFI system partition
BootOrder not defined
EFI boot manager: Cannot load any image
625488 bytes read in 76 ms (7.8 MiB/s)
libfdt fdt_check_header(): FDT_ERR_BADMAGIC
Booting /efi\boot\bootarm.efi
Consoles: EFI console
    Reading loader env vars from /efi/freebsd/loader.env
Setting currdev to disk0p1:
FreeBSD/arm EFI loader, Revision 1.1

   Command line arguments: l
   EFI version: 2.80
   EFI Firmware: Das U-Boot (rev 8224.4096)
   Console: efi (0)
   Load Path: /efi\boot\bootarm.efi
   Load Device: /VenHw(e61d73b9-a384-4acc-aeab-82e828f3628b)/SD(1)/SD(0)/HD(1,0x01,0,0x42f,0x18fa8)
Trying ESP: /VenHw(e61d73b9-a384-4acc-aeab-82e828f3628b)/SD(1)/SD(0)/HD(1,0x01,0,0x42f,0x18fa8)
Setting currdev to disk0p1:
Trying: /VenHw(e61d73b9-a384-4acc-aeab-82e828f3628b)/SD(1)/SD(0)/HD(2,0x01,0,0x193d7,0x1d88c29)
Setting currdev to disk0p2:
Loading /boot/defaults/loader.conf
Loading /boot/defaults/loader.conf
Loading /boot/device.hints
Loading /boot/loader.conf
Loading /boot/loader.conf.local
Loading kernel...
/boot/kernel/kernel text=0x6f1708 data=0x83710 data=0x0+0x1f0000 syms=[0x4+0xc37f0+0x4+0xcd395]
Loading configured modules...
/etc/hostid size=0x25
/boot/entropy size=0x1000
/boot/kernel/umodem.ko text=0x1540 text=0xf40 data=0x234+0x4 syms=[0x4+0xe70+0x4+0xa74]
loading required module 'ucom'
/boot/kernel/ucom.ko text=0x1714 text=0x2d3c data=0x3c4+0x838 syms=[0x4+0x13e0+0x4+0xbac]

Hit [Enter] to boot immediately, or any other key for command prompt.
Booting [/boot/kernel/kernel]...
Using DTB provided by EFI at 0x7ef6000.
Kernel entry at 0x14c00180...
Kernel args: (null)
---<<BOOT>>---
Copyright (c) 1992-2020 The FreeBSD Project.
Copyright (c) 1979, 1980, 1983, 1986, 1988, 1989, 1991, 1992, 1993, 1994
        The Regents of the University of California. All rights reserved.
FreeBSD is a registered trademark of The FreeBSD Foundation.
FreeBSD 12.2-STABLE r367792 RPI-B arm
FreeBSD clang version 10.0.1 (git@github.com:llvm/llvm-project.git llvmorg-10.0.1-0-gef32c611aa2)
VT: init without driver.
CPU: ARM ARM1176 r0p7 (ECO: 0x00000000)
CPU Features:
  Thumb, Security, VMSAv7
Optional instructions:
  UMULL, SMULL, MLA, SIMD(ext)
  16KB/32B 4-way instruction cache
  16KB/32B 4-way WB data cache
real memory  = 469757952 (447 MB)
avail memory = 446181376 (425 MB)
random: unblocking device.
random: entropy device external interface
kbd0 at kbdmux0
ofwbus0: <Open Firmware Device Tree>
simplebus0: <Flattened device tree simple bus> on ofwbus0
intc0: <BCM2835 Interrupt Controller> mem 0x7e00b200-0x7e00b3ff on simplebus0
gpio0: <BCM2708/2835 GPIO controller> mem 0x7e200000-0x7e2000b3 irq 19,20 on simplebus0
gpiobus0: <OFW GPIO bus> on gpio0
bcm_dma0: <BCM2835 DMA Controller> mem 0x7e007000-0x7e007eff irq 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16 on simplebus0
bcmwd0: <BCM2708/2835 Watchdog> mem 0x7e100000-0x7e100113,0x7e00a000-0x7e00a023 on simplebus0
bcmrng0: <Broadcom BCM2835 RNG> mem 0x7e104000-0x7e10400f irq 17 on simplebus0
mbox0: <BCM2835 VideoCore Mailbox> mem 0x7e00b880-0x7e00b8bf irq 18 on simplebus0
gpioc0: <GPIO controller> on gpio0
uart0: <PrimeCell UART (PL011)> mem 0x7e201000-0x7e2011ff irq 21 on simplebus0
uart0: console (115200,n,8,1)
bcm283x_dwcotg0: <DWC OTG 2.0 integrated USB controller (bcm283x)> mem 0x7e980000-0x7e98ffff,0x7e006000-0x7e006fff irq 41,42 on simplebus0
usbus0 on bcm283x_dwcotg0
systimer0 mem 0x7e003000-0x7e003fff irq 43,44,45,46 on simplebus0
Event timer "BCM2835-3" frequency 1000000 Hz quality 1000
Timecounter "BCM2835-3" frequency 1000000 Hz quality 1000
sdhci_bcm0: <Broadcom 2708 SDHCI controller> mem 0x7e300000-0x7e3000ff irq 48 on simplebus0
mmc0: <MMC/SD bus> on sdhci_bcm0
vchiq0: <BCM2835 VCHIQ> mem 0x7e00b840-0x7e00b87b irq 52 on simplebus0
vchiq: local ver 8 (min 3), remote ver 8.
pcm0: <VCHIQ audio> on vchiq0
fb0: <BCM2835 VT framebuffer driver> on simplebus0
fb0: keeping existing fb bpp of 32
fbd0 on fb0
VT: initialize with new VT driver "fb".
fb0: 656x416(656x416@0,0) 32bpp
fb0: fbswap: 1, pitch 2624, base 0x1eaf0000, screen_size 1091584
gpioled0: <GPIO LEDs> on ofwbus0
cryptosoft0: <software crypto>
Timecounters tick every 1.000 msec
usbus0: 480Mbps High Speed USB v2.0
ugen0.1: <DWCOTG OTG Root HUB> at usbus0
uhub0: <DWCOTG OTG Root HUB, class 9/0, rev 2.00/1.00, addr 1> on usbus0
mmcsd0: 16GB <SDHC NCard 1.0 SN 229045F3 MFG 02/2012 by 115 BG> at mmc0 50.0MHz/4bit/65535-block
Trying to mount root from ufs:/dev/ufs/rootfs [rw]...
WARNING:  was not properly dismounted
Warning: no time-of-day clock registered, system time will not be set accurately
uhub0: 1 port with 1 removable, self powered
ugen0.2: <vendor 0x214b USB2.0 HUB> at usbus0
uhub1 on uhub0
uhub1: <vendor 0x214b USB2.0 HUB, class 9/0, rev 2.00/1.00, addr 2> on usbus0
Setting hostuuid: 617d555b-2a1c-11eb-ad9d-75bde1a855ce.
Setting hostid: 0xdaae4663.
Starting file system checks:
uhub1: 4 ports with 4 removable, self powered
/dev/ufs/rootfs: 24059 files, 503425 used, 3241854 free (214 frags, 405205 blocks, 0.0% fragmentation)
Mounting local filesystems:.
ELF ldconfig path: /lib /usr/lib /usr/lib/compat
Soft Float compatibility ldconfig path:
Setting hostname: rpi-b.
Setting up harvesting: [UMA],[FS_ATIME],SWI,INTERRUPT,NET_NG,NET_ETHER,NET_TUN,MOUSE,KEYBOARD,ATTACH,CACHED
Feeding entropy: .
lo0: link state changed to UP
Starting Network: lo0.
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> metric 0 mtu 16384
        options=680003<RXCSUM,TXCSUM,LINKSTATE,RXCSUM_IPV6,TXCSUM_IPV6>
        inet6 ::1 prefixlen 128
        inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1
        inet 127.0.0.1 netmask 0xff000000
        groups: lo
        nd6 options=21<PERFORMNUD,AUTO_LINKLOCAL>
ugen0.3: <Unknown > at usbus0 (disconnected)
Starting devd.
add host 127.0.0.1: gateway lo0 fib 0: route already in table
add host ::1: gateway lo0 fib 0: route already in table
add net fe80::: gateway ::1
add net ff02::: gateway ::1
add net ::ffff:0.0.0.0: gateway ::1
add net ::0.0.0.0: gateway ::1
Creating and/or trimming log files.
Updating motd:.
Updating /var/run/os-release done.
Starting syslogd.
Clearing /tmp (X related).
Mounting late filesystems:.
Performing sanity check on sshd configuration.
Starting sshd.
Configuring vt: blanktime.
Starting cron.
Starting background file system checks in 60 seconds.

Thu Nov 21 14:24:18 UTC 2020

FreeBSD/arm (rpi-b) (ttyu0)

login:
Password:
```

## Logging in to FreeBSD

The default passwords for the images are **freebsd/freebsd** and **root/root** .

If you logging remotely, **freebsd/freebsd** is mandatory, since **root** remote login is disabled by default.

## cat /etc/os-release

```shell
root@rpi-b:~ # ls -l /etc/os-release
lrwxr-xr-x  1 root  wheel  21 Nov 19 04:03 /etc/os-release -> ../var/run/os-release

root@rpi-b:~ # cat /etc/os-release
NAME=FreeBSD
VERSION=12.2-STABLE
VERSION_ID=12.2
ID=freebsd
ANSI_COLOR="0;31"
PRETTY_NAME="FreeBSD 12.2-STABLE"
CPE_NAME=cpe:/o:freebsd:freebsd:12.2
HOME_URL=https://FreeBSD.org/
BUG_REPORT_URL=https://bugs.FreeBSD.org/
```

## List of processes after startup

```shell
root@rpi-b:~ # ps ax
PID TT  STAT     TIME COMMAND
  0  -  DLs   0:00.01 [kernel]
  1  -  ILs   0:00.06 /sbin/init --
  2  -  DL    0:00.00 [crypto]
  3  -  DL    0:00.00 [crypto returns 0]
  4  -  DL    0:00.00 [cam]
  5  -  IL    0:00.00 [VCHIQ-0]
  6  -  IL    0:00.00 [VCHIQr-0]
  7  -  IL    0:00.00 [VCHIQs-0]
  8  -  DL    0:00.00 [sctp_iterator]
  9  -  DL    0:00.78 [rand_harvestq]
 10  -  RNL  30:45.95 [idle]
 11  -  WL    0:10.31 [intr]
 12  -  DL    0:00.53 [geom]
 13  -  DL    0:00.00 [sequencer 00]
 14  -  DL    0:00.27 [usb]
 15  -  DL    0:00.00 [soaiod1]
 16  -  DL    0:00.00 [soaiod2]
 17  -  DL    0:00.00 [soaiod3]
 18  -  DL    0:00.00 [soaiod4]
 19  -  DL    0:00.47 [mmcsd0: mmc/sd card]
 20  -  IL    0:00.00 [VCHIQka-0]
 21  -  IL    0:00.00 [bcm2835_audio_worke]
 22  -  DL    0:00.59 [pagedaemon]
 23  -  DL    0:00.00 [vmdaemon]
 24  -  DL    0:00.51 [bufdaemon]
 25  -  DL    0:00.04 [vnlru]
 26  -  DL    0:00.16 [syncer]
 27  -  DL    0:00.29 [schedcpu]
555  -  Ss    0:00.00 /sbin/devd
728  -  Is    0:00.06 /usr/sbin/syslogd -s
819  -  Is    0:00.01 /usr/sbin/sshd
828  -  Ss    0:00.08 /usr/sbin/cron -s
852 u0  Is    0:00.21 login [pam] (login)
853 u0  S     0:00.58 -csh (csh)
937 u0  R+    0:00.02 ps ax
844 v0  Is+   0:00.03 /usr/libexec/getty Pc ttyv0
845 v1  Is+   0:00.03 /usr/libexec/getty Pc ttyv1
846 v2  Is+   0:00.03 /usr/libexec/getty Pc ttyv2
847 v3  Is+   0:00.03 /usr/libexec/getty Pc ttyv3
848 v4  Is+   0:00.03 /usr/libexec/getty Pc ttyv4
849 v5  Is+   0:00.03 /usr/libexec/getty Pc ttyv5
850 v6  Is+   0:00.03 /usr/libexec/getty Pc ttyv6
851 v7  Is+   0:00.03 /usr/libexec/getty Pc ttyv7
root@rpi-b:~ #
```

## Conclusion

FreeBSD 12.2 can't found Realtek RTL8152 Based USB Ethernet Adapter, it's really disappointing. FreeBSD 13.0 snapshot have boot errors, it's even more disappointing.
