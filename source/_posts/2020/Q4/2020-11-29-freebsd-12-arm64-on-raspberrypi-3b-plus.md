---
title: FreeBSD 12 arm64 on Raspberry Pi 3B+
excerpt: FreeBSD 12 arm64 on Raspberry Pi 3B+
date: 2020-11-29 12:23:41
tags:
    - Operating system
    - FreeBSD
categories: [Operating system, FreeBSD]
---

# FreeBSD 12 arm64 on Raspberry Pi 3B+

## Download FreeBSD image

-   https://www.freebsd.org/platforms/index.html
-   https://www.freebsd.org/platforms/arm.html
-   https://wiki.freebsd.org/arm64
-   https://wiki.freebsd.org/arm64/QEMU
-   https://wiki.freebsd.org/arm
-   https://wiki.freebsd.org/arm/Raspberry%20Pi
-   https://wiki.freebsd.org/arm/RockChip
-   https://wiki.freebsd.org/arm/Allwinner
-   https://www.freebsd.org/cgi/man.cgi?query=muge
-   https://www.freebsd.org/cgi/man.cgi?query=bwn
-   https://wiki.freebsd.org/DeviceDrivers
-   https://wiki.freebsd.org/SDIO
-   https://download.freebsd.org/ftp/releases/arm/armv6/ISO-IMAGES/12.2/FreeBSD-12.2-RELEASE-arm-armv6-RPI-B.img.xz
-   https://download.freebsd.org/ftp/releases/arm/armv7/ISO-IMAGES/12.2/FreeBSD-12.2-RELEASE-arm-armv7-RPI2.img.xz
-   https://download.freebsd.org/ftp/releases/arm64/aarch64/ISO-IMAGES/12.2/FreeBSD-12.2-RELEASE-arm64-aarch64-RPI3.img.xz
-   https://download.freebsd.org/ftp/snapshots/ISO-IMAGES/13.0/
-   https://download.freebsd.org/ftp/snapshots/ISO-IMAGES/13.0/FreeBSD-13.0-CURRENT-arm-armv6-RPI-B-20201203-f659bf1d31c.img.xz
-   https://download.freebsd.org/ftp/snapshots/ISO-IMAGES/13.0/FreeBSD-13.0-CURRENT-arm-armv7-RPI2-20201119-f2ea0734875.img.xz
-   https://download.freebsd.org/ftp/snapshots/ISO-IMAGES/13.0/FreeBSD-13.0-CURRENT-arm64-aarch64-RPI3-20201203-f659bf1d31c.img.xz

```shell
$ aria2c https://download.freebsd.org/ftp/releases/arm64/aarch64/ISO-IMAGES/12.2/FreeBSD-12.2-RELEASE-arm64-aarch64-RPI3.img.xz

FREEBSD_STABLE_ROOT=https://download.freebsd.org/ftp/snapshots/arm64/aarch64/ISO-IMAGES/12.2
FREEBSD_STABLE_FILE=`curl -sS ${FREEBSD_STABLE_ROOT}/ | grep -oP "(FreeBSD-.*?-STABLE-arm64-aarch64-RPI3-.*?xz)" | tail -1`
aria2c ${FREEBSD_STABLE_ROOT}/${FREEBSD_STABLE_FILE}
```

## Write image to SDXC card

```shell
$ cat FreeBSD-12.2-RELEASE-arm64-aarch64-RPI3.img.xz | xz -cd | sudo dd of=/dev/sdb bs=4M oflag=sync status=progress; time sync
3205439488 bytes (3.2 GB, 3.0 GiB) copied, 185 s, 17.3 MB/s
0+49215 records in
0+49215 records out
3221225472 bytes (3.2 GB, 3.0 GiB) copied, 185.924 s, 17.3 MB/s

real	0m0.005s
user	0m0.001s
sys	0m0.000s
```

## Booting Raspberry Pi

```shell
$ sudo minicom -b 115200 -8 -D /dev/ttyUSB0
```

```shell
U-Boot 2020.07 (Oct 23 2020 - 01:37:07 +0000)

DRAM:  948 MiB
RPI 3 Model B+ (0xa020d3)
MMC:   mmc@7e300000: 0
Loading Environment from FAT... In:    serial
Out:   vidconsole
Err:   vidconsole
Net:   No ethernet found.
starting USB...
Bus usb@7e980000: USB DWC2
scanning bus usb@7e980000 for devices... 4 USB Device(s) found
       scanning usb for storage devices... 0 Storage Device(s) found
Hit any key to stop autoboot:  0
switch to partitions #0, OK
mmc0 is current device
Scanning mmc 0:1...
Found EFI removable media binary efi/boot/bootaa64.efi
libfdt fdt_check_header(): FDT_ERR_BADMAGIC
Scanning disk mmc@7e300000.blk...
** Unrecognized filesystem type **
Found 3 disks
BootOrder not defined
EFI boot manager: Cannot load any image
640224 bytes read in 62 ms (9.8 MiB/s)
libfdt fdt_check_header(): FDT_ERR_BADMAGIC
Consoles: EFI console
    Reading loader env vars from /efi/freebsd/loader.env
Setting currdev to disk0p1:
FreeBSD/arm64 EFI loader, Revision 1.1

   Command line arguments: loader.efi
   EFI version: 2.80
   EFI Firmware: Das U-Boot (rev 8224.1792)
   Console: efi (0)
   Load Path: /efi\boot\bootaa64.efi
   Load Device: /VenHw(e61d73b9-a384-4acc-aeab-82e828f3628b)/SD(0)/SD(0)/HD(1,0x01,0,0x81f,0x18fa8)
Trying ESP: /VenHw(e61d73b9-a384-4acc-aeab-82e828f3628b)/SD(0)/SD(0)/HD(1,0x01,0,0x81f,0x18fa8)
Setting currdev to disk0p1:
Trying: /VenHw(e61d73b9-a384-4acc-aeab-82e828f3628b)/SD(0)/SD(0)/HD(2,0x01,0,0x197c7,0x5e6821)
Setting currdev to disk0p2:
Loading /boot/defaults/loader.conf
Loading /boot/defaults/loader.conf
Loading /boot/device.hints
Loading /boot/loader.conf
Loading /boot/loader.conf.local
Loading kernel...
/boot/kernel/kernel text=0x9081b4 data=0x185f50 data=0x0+0x2d0a3e syms=[0x8+0x134550+0x8+0x12137a]
Loading configured modules...
can't find '/etc/hostid'
/boot/kernel/umodem.ko text=0x1ba0 text=0xf60 data=0x610+0x8 syms=[0x8+0xe28+0x8+0xa7e]
can't find '/boot/entropy'

Hit [Enter] to boot immediately, or any other key for command prompt.
Booting [/boot/kernel/kernel]...
Using DTB provided by EFI at 0x7ef6000.
EFI framebuffer information:
addr, size     0x3e513000, 0x6d8c00
dimensions     1824 x 984
stride         1824
masks          0x00ff0000, 0x0000ff00, 0x000000ff, 0xff000000
---<<BOOT>>---
Copyright (c) 1992-2020 The FreeBSD Project.
Copyright (c) 1979, 1980, 1983, 1986, 1988, 1989, 1991, 1992, 1993, 1994
        The Regents of the University of California. All rights reserved.
FreeBSD is a registered trademark of The FreeBSD Foundation.
FreeBSD 12.2-RELEASE r366954 GENERIC arm64
FreeBSD clang version 10.0.1 (git@github.com:llvm/llvm-project.git llvmorg-10.0.1-0-gef32c611aa2)
VT(efifb): resolution 1824x984
KLD file umodem.ko is missing dependencies
real memory  = 993849344 (947 MB)
avail memory = 950001664 (905 MB)
Starting CPU 1 (1)
Starting CPU 2 (2)
Starting CPU 3 (3)
FreeBSD/SMP: Multiprocessor System Detected: 4 CPUs
arc4random: no preloaded entropy cache
random: entropy device external interface
MAP 39f3e000 mode 2 pages 1
MAP 39f44000 mode 2 pages 2
MAP 3b350000 mode 2 pages 16
MAP 3f100000 mode 0 pages 1
kbd0 at kbdmux0
ofwbus0: <Open Firmware Device Tree>
simplebus0: <Flattened device tree simple bus> on ofwbus0
ofw_clkbus0: <OFW clocks bus> on ofwbus0
clk_fixed0: <Fixed clock> on ofw_clkbus0
clk_fixed1: <Fixed clock> on ofw_clkbus0
regfix0: <Fixed Regulator> on ofwbus0
regfix1: <Fixed Regulator> on ofwbus0
psci0: <ARM Power State Co-ordination Interface Driver> on ofwbus0
lintc0: <BCM2836 Interrupt Controller> mem 0x40000000-0x400000ff on simplebus0
intc0: <BCM2835 Interrupt Controller> mem 0x7e00b200-0x7e00b3ff irq 48 on simplebus0
gpio0: <BCM2708/2835 GPIO controller> mem 0x7e200000-0x7e2000b3 irq 24,25 on simplebus0
gpiobus0: <OFW GPIO bus> on gpio0
generic_timer0: <ARMv7 Generic Timer> irq 1,2,3,4 on ofwbus0
Timecounter "ARM MPCore Timecounter" frequency 19200000 Hz quality 1000
Event timer "ARM MPCore Eventtimer" frequency 19200000 Hz quality 1000
usb_nop_xceiv0: <USB NOP PHY> on ofwbus0
bcm_dma0: <BCM2835 DMA Controller> mem 0x7e007000-0x7e007eff irq 6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21 on simplebus0
bcmwd0: <BCM2708/2835 Watchdog> mem 0x7e100000-0x7e100113,0x7e00a000-0x7e00a023 on simplebus0
bcm2835_clkman0: <BCM283x Clock Manager> mem 0x7e101000-0x7e102fff on simplebus0
bcmrng0: <Broadcom BCM2835 RNG> mem 0x7e104000-0x7e10400f irq 22 on simplebus0
mbox0: <BCM2835 VideoCore Mailbox> mem 0x7e00b880-0x7e00b8bf irq 23 on simplebus0
gpioc0: <GPIO controller> on gpio0
uart0: <PrimeCell UART (PL011)> mem 0x7e201000-0x7e2011ff irq 26 on simplebus0
uart0: console (115200,n,8,1)
spi0: <BCM2708/2835 SPI controller> mem 0x7e204000-0x7e2041ff irq 28 on simplebus0
spibus0: <OFW SPI bus> on spi0
spibus0: <unknown card> at cs 0 mode 0
spibus0: <unknown card> at cs 1 mode 0
iichb0: <BCM2708/2835 BSC controller> mem 0x7e804000-0x7e804fff irq 40 on simplebus0
bcm283x_dwcotg0: <DWC OTG 2.0 integrated USB controller (bcm283x)> mem 0x7e980000-0x7e98ffff,0x7e006000-0x7e006fff irq 46,47 on simplebus0
usbus0 on bcm283x_dwcotg0
sdhci_bcm0: <Broadcom 2708 SDHCI controller> mem 0x7e300000-0x7e3000ff irq 50 on simplebus0
mmc0: <MMC/SD bus> on sdhci_bcm0
fb0: <BCM2835 VT framebuffer driver> on simplebus0
fb0: keeping existing fb bpp of 32
fbd0 on fb0
VT: Replacing driver "efifb" with new "fb".
fb0: 1824x984(1824x984@0,0) 32bpp
fb0: fbswap: 1, pitch 7296, base 0x3e513000, screen_size 7237632
pmu0: <Performance Monitoring Unit> irq 0 on ofwbus0
cpulist0: <Open Firmware CPU Group> on ofwbus0
cpu0: <Open Firmware CPU> on cpulist0
bcm2835_cpufreq0: <CPU Frequency Control> on cpu0
cpu1: <Open Firmware CPU> on cpulist0
cpu2: <Open Firmware CPU> on cpulist0
cpu3: <Open Firmware CPU> on cpulist0
gpioled0: <GPIO LEDs> on ofwbus0
gpioled0: <led1> failed to map pin
cryptosoft0: <software crypto>
Timecounters tick every 1.000 msec
iicbus0: <OFW I2C bus> on iichb0
iic0: <I2C generic I/O> on iicbus0
usbus0: 480Mbps High Speed USB v2.0
ugen0.1: <DWCOTG OTG Root HUB> at usbus0
uhub0: <DWCOTG OTG Root HUB, class 9/0, rev 2.00/1.00, addr 1> on usbus0
mmcsd0: 63GB <SDHC SD64G 6.0 SN DA7F8E63 MFG 10/2020 by 39 PH> at mmc0 50.0MHz/4bit/65535-block
mbox0: mbox response error
bcm2835_cpufreq0: can't get clock rate (id=8)
bcm2835_cpufreq0: ARM 600MHz, Core 250MHz, SDRAM -999MHz, Turbo OFF
Release APs...done
CPU  0: ARM Cortex-A53 r0p4 affinity:  0
arc4random: no preloaded entropy cache
 Instruction Set Attributes 0 = <CRC32>
Trying to mount root from ufs:/dev/ufs/rootfs [rw]...
 Instruction Set Attributes 1 = <>
         Processor Features 0 = <AdvSIMD,Float,EL3 32,EL2 32,EL1 32,EL0 32>
         Processor Features 1 = <0>
      Memory Model Features 0 = <4k Granule,64k Granule,S/NS Mem,MixedEndian,16bit ASID,1TB PA>
      Memory Model Features 1 = <>
      Memory Model Features 2 = <32b CCIDX,48b VA>
             Debug Features 0 = <2 CTX Breakpoints,4 Watchpoints,6 Breakpoints,PMUv3,Debug v8>
             Debug Features 1 = <0>
         Auxiliary Features 0 = <0>
         Auxiliary Features 1 = <0>
CPU  1: ARM Cortex-A53 r0p4 affinity:  1
CPU  2: ARM Cortex-A53 r0p4 affinity:  2
CPU  3: ARM Cortex-A53 r0p4 affinity:  3
arc4random: no preloaded entropy cache
Warning: no time-of-day clock registered, system time will not be set accurately
uhub0: 1 port with 1 removable, self powered
arc4random: no preloaded entropy cache
Growing root partition to fill device
random: read_random_uio unblock wait
ugen0.2: <vendor 0x0424 product 0x2514> at usbus0
uhub1 on uhub0
uhub1: <vendor 0x0424 product 0x2514, class 9/0, rev 2.00/b.b3, addr 2> on usbus0
uhub1: MTT enabled
uhub1: 4 ports with 3 removable, self powered
random: unblocking device.
ugen0.3: <vendor 0x0424 product 0x2514> at usbus0
uhub2 on uhub1
uhub2: <vendor 0x0424 product 0x2514, class 9/0, rev 2.00/b.b3, addr 3> on usbus0
uhub2: MTT enabled
GEOM_PART: mmcsd0s2 was automatically resized.
  Use `gpart commit mmcsd0s2` to save changes or `gpart undo mmcsd0s2` to revert them.
mmcsd0s2 resized
mmcsd0s2a resized
gpart: arg0 'ufs/rootfs': Invalid argument
super-block backups (for fsck_ffs -b #) at:
 6411392, 7693632, 8975872, 10258112, 11540352, 12822592, 14104832, 15387072,
 16669312, 17951552, 19233792, 20516032, 21798272, 23080512, 24362752,
 25644992, 26927232, 28209472, 29491712, 30773952, 32056192, 33338432,
 34620672, 35902912, 37185152, 38467392, 39749632, 41031872, 42314112,
 43596352, 44878592, 46160832, 47443072, 48725312, 50007552, 51289792,
 52572032, 53854272, 55136512, 56418752, 57700992, 58983232,uhub2: 3 ports with 2 removable, self powered
 60265472,
 61547712, 62829952, 64112192, 65394432, 66676672, 67958912, 69241152,
 70523392, 71805632, 73087872, 74370112, 75652352, 76934592, 78216832,
 79499072, 80781312, 82063552, 83345792, 84628032, 85910272, 87192512,
 88474752, 89756992, 91039232, 92321472, 93603712, 94885952, 96168192,
 97450432, 98732672, 100014912, 101297152, 102579392, 103861632, 105143872,
 106426112, 107708352, 108990592, 110272832, 111555072, 112837312, 114119552,
 115401792, 116684032, 117966272, 119248512, 120530752, 121812992
ugen0.4: <vendor 0x0424 product 0x7800> at usbus0
muge0 on uhub2
muge0: <vendor 0x0424 product 0x7800, rev 2.10/3.00, addr 4> on usbus0
muge0: Chip ID 0x7800 rev 0002
Setting hostuuid: 30303030-3030-3030-3636-363631333338.
Setting hostid: 0x816ce8bf.
Starting file system checks:
miibus0: <MII bus> on muge0
ukphy0: <Generic IEEE 802.3u media interface> PHY 1 on miibus0
ukphy0:  none, 10baseT, 10baseT-FDX, 100baseTX, 100baseTX-FDX, 1000baseT, 1000baseT-master, 1000baseT-FDX, 1000baseT-FDX-master, auto
ue0: <USB Ethernet> on muge0
ue0: Ethernet address: b8:27:eb:66:13:38
/dev/ufs/rootfs: FILE SYSTEM CLEAN; SKIPPING CHECKS
/dev/ufs/rootfs: clean, 14112954 free (18 frags, 1764117 blocks, 0.0% fragmentation)
Mounting local filesystems:.
ELF ldconfig path: /lib /usr/lib /usr/lib/compat
Building /boot/kernel/linker.hints
Setting hostname: generic.
Setting up harvesting: [UMA],[FS_ATIME],SWI,INTERRUPT,NET_NG,NET_ETHER,NET_TUN,MOUSE,KEYBOARD,ATTACH,CACHED
Feeding entropy: .
lo0: link state changed to UP
muge0: Chip ID 0x7800 rev 0002
ue0: link state changed to DOWN
ue0: link state changed to UP
Starting Network: lo0 ue0.
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> metric 0 mtu 16384
        options=680003<RXCSUM,TXCSUM,LINKSTATE,RXCSUM_IPV6,TXCSUM_IPV6>
        inet6 ::1 prefixlen 128
        inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1
        inet 127.0.0.1 netmask 0xff000000
        groups: lo
        nd6 options=21<PERFORMNUD,AUTO_LINKLOCAL>
ue0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500
        options=80009<RXCSUM,VLAN_MTU,LINKSTATE>
        ether b8:27:eb:66:13:38
        media: Ethernet autoselect (100baseTX <full-duplex>)
        status: active
        nd6 options=29<PERFORMNUD,IFDISABLED,AUTO_LINKLOCAL>
Starting devd.
Starting dhclient.
DHCPDISCOVER on ue0 to 255.255.255.255 port 67 interval 6
DHCPOFFER from 192.168.1.1
unknown dhcp option value 0x7d
DHCPREQUEST on ue0 to 255.255.255.255 port 67
DHCPACK from 192.168.1.1
unknown dhcp option value 0x7d
bound to 192.168.1.8 -- renewal in 43200 seconds.
add host 127.0.0.1: gateway lo0 fib 0: route already in table
add host ::1: gateway lo0 fib 0: route already in table
add net fe80::: gateway ::1
add net ff02::: gateway ::1
add net ::ffff:0.0.0.0: gateway ::1
add net ::0.0.0.0: gateway ::1
Generating host.conf.
Creating and/or trimming log files.
Starting syslogd.
Clearing /tmp (X related).
Updating motd:.
Mounting late filesystems:.
Updating /var/run/os-release done.
Configuring vt: blanktime.
Generating RSA host key.
2048 SHA256:D48allvap5AHqcmhkVQQUiy0OgS/E6XmH2ioPMW3HWc root@generic (RSA)
Generating ECDSA host key.
256 SHA256:Ct89xJM8EqyHyxO6SBfI3/SoLYnnr/WE+4t+IvzTels root@generic (ECDSA)
Generating ED25519 host key.
256 SHA256:TINaQKiQ9MbblbWDBmIhAzzyYd78+cLjWYZXHuUj2xo root@generic (ED25519)
Performing sanity check on sshd configuration.
Starting sshd.
Starting cron.
Starting background file system checks in 60 seconds.

Fri Oct 23 03:50:22 UTC 2020

FreeBSD/arm64 (generic) (ttyu0)

login:
```

## Logging in to FreeBSD

The default passwords for the images are **freebsd/freebsd** and **root/root** .

If you logging remotely, **freebsd/freebsd** is mandatory, since **root** remote login is disabled by default.

```shell
# vi /etc/ssh/sshd_config
    PermitRootLogin yes
    #PermitRootLogin prohibit-password
    PasswordAuthentication yes
    UseDNS no

# /etc/rc.d/sshd reload
Performing sanity check on sshd configuration.
```

## cat /etc/os-release

```shell
# ls -l /etc/os-release
lrwxr-xr-x  1 root  wheel  21 Oct 23 03:44 /etc/os-release -> ../var/run/os-release

# cat /etc/os-release
NAME=FreeBSD
VERSION=12.2-RELEASE
VERSION_ID=12.2
ID=freebsd
ANSI_COLOR="0;31"
PRETTY_NAME="FreeBSD 12.2-RELEASE"
CPE_NAME=cpe:/o:freebsd:freebsd:12.2
HOME_URL=https://FreeBSD.org/
BUG_REPORT_URL=https://bugs.FreeBSD.org/
```

## System characteristics

### uname

```shell
root@generic:~ # uname -snrmp
FreeBSD generic 12.2-RELEASE arm64 aarch64

root@generic:~ # uname -ap
FreeBSD generic 12.2-RELEASE FreeBSD 12.2-RELEASE r366954 GENERIC  arm64 aarch64
```

### cpu

```shell
root@generic:~ # dmesg | grep -i cpu
Starting CPU 1 (1)
Starting CPU 2 (2)
Starting CPU 3 (3)
FreeBSD/SMP: Multiprocessor System Detected: 4 CPUs
cpulist0: <Open Firmware CPU Group> on ofwbus0
cpu0: <Open Firmware CPU> on cpulist0
bcm2835_cpufreq0: <CPU Frequency Control> on cpu0
cpu1: <Open Firmware CPU> on cpulist0
cpu2: <Open Firmware CPU> on cpulist0
cpu3: <Open Firmware CPU> on cpulist0
bcm2835_cpufreq0: can't get clock rate (id=8)
bcm2835_cpufreq0: ARM 600MHz, Core 250MHz, SDRAM -999MHz, Turbo OFF
CPU  0: ARM Cortex-A53 r0p4 affinity:  0
CPU  1: ARM Cortex-A53 r0p4 affinity:  1
CPU  2: ARM Cortex-A53 r0p4 affinity:  2
CPU  3: ARM Cortex-A53 r0p4 affinity:  3
```

### ure

Realtek RTL8152 Based USB Ethernet Adapter

```shell
root@generic:~ # dmesg | grep -i -E "ure0|realtek"
ugen0.6: <Realtek USB 10/100 LAN> at usbus0
ure0 on uhub3
ure0: <Realtek USB 10/100 LAN, class 0/0, rev 2.10/20.00, addr 6> on usbus0
miibus1: <MII bus> on ure0
ue1: <USB Ethernet> on ure0

# dmesg | grep -i -E "Ethernet|ue0|muge"
muge0 on uhub2
muge0: <vendor 0x0424 product 0x7800, rev 2.10/3.00, addr 4> on usbus0
muge0: Chip ID 0x7800 rev 0002
miibus0: <MII bus> on muge0
ue0: <USB Ethernet> on muge0
ue0: Ethernet address: b8:27:eb:66:13:38
```

### sysctl

```shell
hw.machine: arm64
hw.model: ARM Cortex-A53 r0p4
hw.ncpu: 4
hw.byteorder: 1234
hw.physmem: 974901248
hw.usermem: 856944640
hw.pagesize: 4096
hw.machine_arch: aarch64
hw.realmem: 993849344
hw.cpufreq.temperature: 43470
hw.cpufreq.voltage_sdram_p: 1200000
hw.cpufreq.voltage_sdram_i: 1250000
hw.cpufreq.voltage_sdram_c: 1250000
hw.cpufreq.voltage_core: 1200000
hw.cpufreq.turbo: 0
hw.cpufreq.sdram_freq: 400000000
hw.cpufreq.core_freq: 250000000
hw.cpufreq.arm_freq: 600000000
kern.argmax: 524288
kern.bootfile: /boot/kernel/kernel
kern.clockrate: { hz = 1000, tick = 1000, profhz = 8129, stathz = 127 }
kern.eventtimer.choice: ARM MPCore Eventtimer(1000)
kern.eventtimer.et.ARM MPCore Eventtimer.flags: 6
kern.eventtimer.et.ARM MPCore Eventtimer.frequency: 19200000
kern.eventtimer.et.ARM MPCore Eventtimer.quality: 1000
kern.eventtimer.idletick: 0
kern.eventtimer.periodic: 0
kern.eventtimer.singlemul: 2
kern.eventtimer.timer: ARM MPCore Eventtimer
kern.features.compat_freebsd_32bit: 1
kern.hwpmc.softevents: 16
kern.hz: 1000
kern.ident: GENERIC
kern.module_path: /boot/kernel;/boot/modules;/boot/dtb;/boot/dtb/overlays
kern.osreldate: 1202000
kern.osrelease: 12.2-RELEASE
kern.osrevision: 199506
kern.ostype: FreeBSD
kern.panic_reboot_wait_time: 15
kern.pid_max: 99999
kern.racct.enable: 0
kern.racct.pcpu_threshold: 1
kern.racct.rctl.devctl_rate_limit: 10
kern.racct.rctl.log_rate_limit: 10
kern.racct.rctl.maxbufsize: 16777216
kern.racct.rctl.throttle_max: 4294967295
kern.racct.rctl.throttle_min: 4294967295
kern.racct.rctl.throttle_pct2: 4294967295
kern.racct.rctl.throttle_pct: 4294967295
kern.random.fortuna.minpoolsize: 64
kern.random.harvest.mask: 511
kern.random.harvest.mask_bin: 000000000000000111111111
kern.random.harvest.mask_symbolic: [UMA],[FS_ATIME],SWI,INTERRUPT,NET_NG,NET_ETHER,NET_TUN,MOUSE,KEYBOARD,ATTACH,CACHED
kern.supported_archs: aarch64 armv7
kern.timecounter.alloweddeviation: 5
kern.timecounter.choice: ARM MPCore Timecounter(1000) dummy(-1000000)
kern.timecounter.fast_gettime: 1
kern.timecounter.hardware: ARM MPCore Timecounter
kern.timecounter.stepwarnings: 0
kern.timecounter.tc.ARM MPCore Timecounter.counter: 3051977945
kern.timecounter.tc.ARM MPCore Timecounter.frequency: 19200000
kern.timecounter.tc.ARM MPCore Timecounter.mask: 4294967295
kern.timecounter.tc.ARM MPCore Timecounter.quality: 1000
kern.timecounter.tick: 1
kern.timecounter.timehands_count: 2
kern.version: FreeBSD 12.2-RELEASE r366954 GENERIC
vm.max_kernel_address: 18446463148488654848
vm.min_kernel_address: 18446462598732840960
net.inet.ip.portrange.first: 10000
net.inet.ip.portrange.hifirst: 49152
net.inet.ip.portrange.hilast: 65535
net.inet.ip.portrange.last: 65535
net.inet.ip.portrange.lowfirst: 1023
net.inet.ip.portrange.lowlast: 600
```

### mmc

```shell
# dmesg | grep mmc
sdhci_bcm0: <Broadcom 2708 SDHCI controller> mem 0x7e300000-0x7e3000ff irq 50 on simplebus0
mmc0: <MMC/SD bus> on sdhci_bcm0
mmcsd0: 63GB <SDHC SD64G 6.0 SN DA7F8E63 MFG 10/2020 by 39 PH> at mmc0 50.0MHz/4bit/65535-block
GEOM_PART: mmcsd0s2 was automatically resized.
  Use `gpart commit mmcsd0s2` to save changes or `gpart undo mmcsd0s2` to revert them.
```

### mounts

```shell
root@generic:~ # mount
/dev/ufs/rootfs on / (ufs, local, soft-updates)
devfs on /dev (devfs, local, multilabel)
/dev/msdosfs/MSDOSBOOT on /boot/msdos (msdosfs, local, noatime)
tmpfs on /tmp (tmpfs, local)
procfs on /proc (procfs, local)

root@generic:~ # df -h
Filesystem                Size    Used   Avail Capacity  Mounted on
/dev/ufs/rootfs            56G    2.8G     49G     5%    /
devfs                     1.0K    1.0K      0B   100%    /dev
/dev/msdosfs/MSDOSBOOT     50M     14M     36M    28%    /boot/msdos
tmpfs                      50M    4.0K     50M     0%    /tmp
procfs                    4.0K    4.0K      0B   100%    /proc
```

### fdisk

```shell
root@generic:~ # diskinfo -v /dev/mmcsd0
/dev/mmcsd0
        512             # sectorsize
        62511906816     # mediasize in bytes (58G)
        122093568       # mediasize in sectors
        4194304         # stripesize
        0               # stripeoffset
        SDHC SD64G 6.0 SN DA7F8E63 MFG 10/2020 by 39 PH # Disk descr.
        DA7F8E63        # Disk ident.
        Yes             # TRIM/UNMAP support
        0               # Rotation rate in RPM

root@generic:~ # gpart show
=>       63  122093505  mmcsd0  MBR  (58G)
         63       2016          - free -  (1.0M)
       2079     102312       1  fat32lba  [active]  (50M)
     104391  121989177       2  freebsd  (58G)

=>        0  121989177  mmcsd0s2  BSD  (58G)
          0         57            - free -  (29K)
         57  121989120         1  freebsd-ufs  (58G)
```

## List of processes after startup

```shell
# ps -wxdlww
UID  PID PPID C PRI NI   VSZ  RSS MWCHAN   STAT TT      TIME COMMAND
  0    0    0 1 -16  0     0  384 swapin   DLs   -   0:00.20 [kernel]
  0    1    0 1  20  0  9868  564 wait     ILs   -   0:00.07 - /sbin/init --
  0  614    1 1  24  0 11248 2456 select   Is    -   0:00.01 |-- dhclient: system.syslog (dhclient)
  0  617    1 0  52  0 11456 2600 select   Is    -   0:00.01 |-- dhclient: ue0 [priv] (dhclient)
  0  683    1 1  20  0 10340 1092 select   Is    -   0:00.00 |-- /sbin/devd
  0  754    1 3  20  0 11252 2512 select   Ss    -   0:00.02 |-- /usr/sbin/syslogd -s
  0  957    1 3  20  0 19132 7800 select   Is    -   0:00.06 |-- /usr/sbin/sshd
  0  961    1 1  34  0 11268 2528 nanslp   Is    -   0:00.01 |-- /usr/sbin/cron -s
  0 1034    1 2  20  0 11984 3156 wait     Is   u0   0:00.05 |-- login [pam] (login)
  0 1035 1034 1  20  0 13012 3620 pause    S    u0   0:00.06 | `-- -csh (csh)
  0 1057 1035 3  20  0 11644 2692 -        R+   u0   0:00.00 |   `-- ps -wxdlww
  0 1008    1 1  52  0 10732 2160 ttyin    Is+  v0   0:00.01 |-- /usr/libexec/getty Pc ttyv0
  0 1009    1 0  52  0 10732 2160 ttyin    Is+  v1   0:00.01 |-- /usr/libexec/getty Pc ttyv1
  0 1010    1 3  52  0 10732 2160 ttyin    Is+  v2   0:00.01 |-- /usr/libexec/getty Pc ttyv2
  0 1011    1 2  52  0 10732 2160 ttyin    Is+  v3   0:00.01 |-- /usr/libexec/getty Pc ttyv3
  0 1012    1 2  52  0 10732 2160 ttyin    Is+  v4   0:00.01 |-- /usr/libexec/getty Pc ttyv4
  0 1013    1 2  52  0 10732 2160 ttyin    Is+  v5   0:00.01 |-- /usr/libexec/getty Pc ttyv5
  0 1014    1 1  52  0 10732 2160 ttyin    Is+  v6   0:00.01 |-- /usr/libexec/getty Pc ttyv6
  0 1015    1 1  52  0 10732 2160 ttyin    Is+  v7   0:00.01 `-- /usr/libexec/getty Pc ttyv7
  0    2    0 0 -16  0     0   16 crypto_w DL    -   0:00.00 - [crypto]
  0    3    0 0 -16  0     0   16 crypto_r DL    -   0:00.00 - [crypto returns 0]
  0    4    0 0 -16  0     0   16 crypto_r DL    -   0:00.00 - [crypto returns 1]
  0    5    0 0 -16  0     0   16 crypto_r DL    -   0:00.00 - [crypto returns 2]
  0    6    0 0 -16  0     0   16 crypto_r DL    -   0:00.00 - [crypto returns 3]
  0    7    0 0 -16  0     0   32 -        DL    -   0:00.00 - [cam]
  0    8    0 1 -16  0     0   16 waiting_ DL    -   0:00.00 - [sctp_iterator]
  0    9    0 0 -16  0     0   16 -        DL    -   0:00.04 - [rand_harvestq]
  0   10    0 0 -16  0     0   16 audit_wo DL    -   0:00.00 - [audit]
  0   11    0 0 155  0     0   64 -        RNL   -  23:34.32 - [idle]
  0   12    0 0 -52  0     0  336 -        WL    -   0:02.72 - [intr]
  0   13    0 3  -8  0     0   48 -        DL    -   0:00.13 - [geom]
  0   14    0 0 -16  0     0   16 seqstate DL    -   0:00.00 - [sequencer 00]
  0   15    0 0 -68  0     0   96 -        DL    -   0:00.64 - [usb]
  0   16    0 0 -16  0     0   16 -        DL    -   0:00.00 - [soaiod1]
  0   17    0 0 -16  0     0   16 -        DL    -   0:00.00 - [soaiod2]
  0   18    0 1 -16  0     0   16 -        DL    -   0:00.00 - [soaiod3]
  0   19    0 0 -16  0     0   16 -        DL    -   0:00.00 - [soaiod4]
  0   20    0 2 -16  0     0   16 mmcsd di DL    -   0:00.10 - [mmcsd0: mmc/sd card]
  0   21    0 0 -16  0     0   48 psleep   DL    -   0:00.03 - [pagedaemon]
  0   22    0 2 -16  0     0   16 psleep   DL    -   0:00.00 - [vmdaemon]
  0   23    0 0 -16  0     0   48 qsleep   DL    -   0:00.03 - [bufdaemon]
  0   24    0 0 -16  0     0   16 vlruwt   DL    -   0:00.00 - [vnlru]
  0   25    0 0  16  0     0   16 syncer   DL    -   0:00.05 - [syncer]
```

## ifconfig

```shell
# ifconfig -am
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> metric 0 mtu 16384
        options=680003<RXCSUM,TXCSUM,LINKSTATE,RXCSUM_IPV6,TXCSUM_IPV6>
        capabilities=680003<RXCSUM,TXCSUM,LINKSTATE,RXCSUM_IPV6,TXCSUM_IPV6>
        inet6 ::1 prefixlen 128
        inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1
        inet 127.0.0.1 netmask 0xff000000
        groups: lo
        nd6 options=21<PERFORMNUD,AUTO_LINKLOCAL>
ue0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500
        options=80009<RXCSUM,VLAN_MTU,LINKSTATE>
        capabilities=80009<RXCSUM,VLAN_MTU,LINKSTATE>
        ether b8:27:eb:66:13:38
        inet 192.168.1.8 netmask 0xffffff00 broadcast 192.168.1.255
        media: Ethernet autoselect (none)
        status: no carrier
        supported media:
                media autoselect
                media 1000baseT mediaopt full-duplex,master
                media 1000baseT mediaopt full-duplex
                media 1000baseT mediaopt master
                media 1000baseT
                media 100baseTX mediaopt full-duplex
                media 100baseTX
                media 10baseT/UTP mediaopt full-duplex
                media 10baseT/UTP
                media none
        nd6 options=29<PERFORMNUD,IFDISABLED,AUTO_LINKLOCAL>
ue1: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500
        options=80000<LINKSTATE>
        capabilities=80000<LINKSTATE>
        ether 00:e0:4c:36:03:b4
        inet 192.168.1.10 netmask 0xffffff00 broadcast 192.168.1.255
        media: Ethernet autoselect (100baseTX <full-duplex>)
        status: active
        supported media:
                media autoselect
                media 100baseTX mediaopt full-duplex
                media 100baseTX
                media 10baseT/UTP mediaopt full-duplex
                media 10baseT/UTP
        nd6 options=29<PERFORMNUD,IFDISABLED,AUTO_LINKLOCAL>
```

## Compiler

```shell
root@generic:~ # file /bin/sh
/bin/sh: ELF 64-bit LSB executable, ARM aarch64, version 1 (FreeBSD), dynamically linked, interpreter /libexec/ld-elf.so.1, for FreeBSD 12.2, FreeBSD-style, stripped

root@generic:~ # ld -v
LLD 10.0.1 (FreeBSD llvmorg-10.0.1-0-gef32c611aa2-1200012) (compatible with GNU linkers)

root@generic:~ # cc -v
FreeBSD clang version 10.0.1 (git@github.com:llvm/llvm-project.git llvmorg-10.0.1-0-gef32c611aa2)
Target: aarch64-unknown-freebsd12.2
Thread model: posix
InstalledDir: /usr/bin

root@generic:~ # c++ -v
FreeBSD clang version 10.0.1 (git@github.com:llvm/llvm-project.git llvmorg-10.0.1-0-gef32c611aa2)
Target: aarch64-unknown-freebsd12.2
Thread model: posix
InstalledDir: /usr/bin
```

## Using pkg for Binary Package Management

```shell
root@generic:~ # pkg update
Updating FreeBSD repository catalogue...
Fetching meta.conf: 100%    163 B   0.2kB/s    00:01
Fetching packagesite.txz: 100%    6 MiB   2.1MB/s    00:03
Processing entries: 100%
FreeBSD repository update completed. 30270 packages processed.
All repositories are up to date.

root@generic:~ # pkg upgrade
Updating FreeBSD repository catalogue...
FreeBSD repository is up to date.
All repositories are up to date.
Updating database digests format: 100%
Checking for upgrades (1 candidates): 100%
Processing candidates (1 candidates): 100%
Checking integrity... done (0 conflicting)
Your packages are up to date.

root@generic:~ # pkg version
Updating FreeBSD repository catalogue...
FreeBSD repository is up to date.
All repositories are up to date.
pkg-1.14.6

root@generic:~ # pkg install tmux bash python38 minicom
Updating FreeBSD repository catalogue...
FreeBSD repository is up to date.
All repositories are up to date.
The following 10 package(s) will be affected (of 0 checked):

New packages to be INSTALLED:
        bash: 5.0.17
        gettext-runtime: 0.20.2
        indexinfo: 0.3.1
        libevent: 2.1.12
        libffi: 3.2.1_3
        lrzsz: 0.12.20_4
        minicom: 2.7.1_2
        python38: 3.8.5
        readline: 8.0.4
        tmux: 3.1b

Number of packages to be installed: 10

The process will require 148 MiB more space.
19 MiB to be downloaded.

Proceed with this action? [y/N]: y
[1/10] Fetching tmux-3.1b.txz: 100%  276 KiB 282.3kB/s    00:01
[2/10] Fetching bash-5.0.17.txz: 100%    1 MiB   1.5MB/s    00:01
[3/10] Fetching python38-3.8.5.txz: 100%   16 MiB   2.5MB/s    00:07
[4/10] Fetching minicom-2.7.1_2.txz: 100%  227 KiB 232.8kB/s    00:01
[5/10] Fetching libevent-2.1.12.txz: 100%  266 KiB 272.6kB/s    00:01
[6/10] Fetching indexinfo-0.3.1.txz: 100%    5 KiB   5.6kB/s    00:01
[7/10] Fetching gettext-runtime-0.20.2.txz: 100%  151 KiB 155.0kB/s    00:01
[8/10] Fetching readline-8.0.4.txz: 100%  316 KiB 323.7kB/s    00:01
[9/10] Fetching libffi-3.2.1_3.txz: 100%   32 KiB  32.7kB/s    00:01
[10/10] Fetching lrzsz-0.12.20_4.txz: 100%   65 KiB  67.0kB/s    00:01
Checking integrity... done (0 conflicting)
[1/10] Installing indexinfo-0.3.1...
[1/10] Extracting indexinfo-0.3.1: 100%
[2/10] Installing gettext-runtime-0.20.2...
[2/10] Extracting gettext-runtime-0.20.2: 100%
[3/10] Installing libevent-2.1.12...
[3/10] Extracting libevent-2.1.12: 100%
[4/10] Installing readline-8.0.4...
[4/10] Extracting readline-8.0.4: 100%
[5/10] Installing libffi-3.2.1_3...
[5/10] Extracting libffi-3.2.1_3: 100%
[6/10] Installing lrzsz-0.12.20_4...
[6/10] Extracting lrzsz-0.12.20_4: 100%
[7/10] Installing tmux-3.1b...
[7/10] Extracting tmux-3.1b: 100%
[8/10] Installing bash-5.0.17...
[8/10] Extracting bash-5.0.17: 100%
[9/10] Installing python38-3.8.5...
[9/10] Extracting python38-3.8.5: 100%
[10/10] Installing minicom-2.7.1_2...
[10/10] Extracting minicom-2.7.1_2: 100%
```

## Conclusion

FreeBSD 12.2 arm64 can use my Realtek RTL8152 Based USB Ethernet Adapter, it’s works very good. But is can’t found WiFi Adapter in Raspberry Pi 3 Model B Plus Rev 1.3, it’s really disappointing.
