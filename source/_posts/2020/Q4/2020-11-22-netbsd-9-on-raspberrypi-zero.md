---
title: NetBSD 9 on Raspberry Pi Zero
description: NetBSD 9 on Raspberry Pi Zero
date: 2020-11-22 10:01:22
tags:
    - Operating system
    - NetBSD
categories: [Operating system, NetBSD]
permalink: netbsd-9-on-raspberrypi-zero
---

# NetBSD 9 on Raspberry Pi Zero

## Download NetBSD image

- http://netbsd.org/
- http://wiki.netbsd.org/ports/
- http://wiki.netbsd.org/ports/evbarm/
- http://wiki.netbsd.org/ports/aarch64/
- http://wiki.netbsd.org/ports/evbmips/
- https://www.netbsd.org/docs/guide/en/chap-boot.html
- http://netbsd.org/releases/formal-9/NetBSD-9.1.html
- https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-earm/INSTALL.html
- https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-aarch64/binary/gzimg/arm64.img.gz
- https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-earmv6hf/binary/gzimg/rpi.img.gz
- https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-earmv7hf/binary/gzimg/armv7.img.gz
- https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-amd64-install.img.gz
- https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-amd64.iso
- https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-evbarm-aarch64.iso
- https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-evbarm-earmv6hf.iso
- https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-evbarm-earmv7hf.iso
- https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-evbmips-mips64el.iso
- https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-evbmips-mipsel.iso

```shell
$ aria2c https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-earmv6hf/binary/gzimg/rpi.img.gz

$ aria2c https://nycdn.netbsd.org/pub/NetBSD-daily/netbsd-9/latest/evbarm-earmv6hf/binary/gzimg/rpi.img.gz
```

## Write image to SDXC card

```shell
$ cat rpi.img.gz | gzip -cd | sudo dd of=/dev/sdb bs=4M oflag=sync status=progress; time sync
```

## Booting Raspberry Pi

```shell
[     1.000000] Copyright (c) 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005,
[     1.000000]     2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017,
[     1.000000]     2018, 2019, 2020 The NetBSD Foundation, Inc.  All rights reserved.
[     1.000000] Copyright (c) 1982, 1986, 1989, 1991, 1993
[     1.000000]     The Regents of the University of California.  All rights reserved.

[     1.000000] NetBSD 9.1 (RPI) #0: Sun Oct 18 19:24:30 UTC 2020
[     1.000000]         mkrepro@mkrepro.NetBSD.org:/usr/src/sys/arch/evbarm/compile/RPI
[     1.000000] total memory = 448 MB
[     1.000000] avail memory = 434 MB
[     1.000000] timecounter: Timecounters tick every 10.000 msec
[     1.000000] running cgd selftest aes-xts-256 aes-xts-512 done
[     1.000000] armfdt0 (root)
[     1.000000] simplebus0 at armfdt0: Raspberry Pi Zero Rev 1.3
[     1.000000] simplebus1 at simplebus0
[     1.000000] simplebus2 at simplebus0
[     1.000000] simplebus3 at simplebus0
[     1.000000] cpus0 at simplebus0
[     1.000000] cpu0 at cpus0: 700 MHz ARM1176JZ-S r0p7 (ARM11J V6ZK core)
[     1.000000] cpu0: DC enabled IC enabled WB enabled LABT
[     1.000000] cpu0: 16KB/32B 4-way L1 VIPT Instruction cache
[     1.000000] cpu0: 16KB/32B 4-way write-back-locking-C L1 VIPT Data cache
[     1.000000] vfp0 at cpu0: VFP11, rounding, exceptions
[     1.000000] fclock0 at simplebus2: 19200000 Hz fixed clock (osc)
[     1.000000] fclock1 at simplebus2: 480000000 Hz fixed clock (otg)
[     1.000000] simplebus4 at simplebus1
[     1.000000] bcmicu0 at simplebus1
[     1.000000] bcmcprman0 at simplebus1: BCM283x Clock Controller
[     1.000000] bcmtmr0 at simplebus1: VC System Timer
[     1.000000] bcmtmr0: interrupting on icu irq 3
[     1.000000] bcmaux0 at simplebus1
[     1.000000] /soc/thermal@7e212000 at simplebus1 not configured
[     1.000000] /soc/dsi@7e209000 at simplebus1 not configured
[     1.000000] bcmgpio0 at simplebus1: GPIO controller
[     1.000000] bcmgpio0: pins 0..31 interrupting on icu irq 49
[     1.000000] bcmgpio0: pins 32..54 interrupting on icu irq 50
[     1.000000] gpio0 at bcmgpio0: 54 pins
[     1.000000] bcmdmac0 at simplebus1: DMA0 DMA2 DMA4 DMA5 DMA8 DMA9 DMA10
[     1.000000] /soc/power at simplebus1 not configured
[     1.000000] bsciic0 at simplebus1: Broadcom Serial Controller
[     1.000000] iic0 at bsciic0: I2C bus
[     1.000000] /phy at simplebus0 not configured
[     1.000000] bcmpmwdog0 at simplebus1: Power management, Reset and Watchdog controller
[     1.000000] bcmmbox0 at simplebus1: VC mailbox
[     1.000000] bcmmbox0: interrupting on icu irq 65
[     1.000000] vcmbox0 at bcmmbox0
[     1.000000] plcom0 at simplebus1: ARM PL011 UART
[     1.000000] plcom0: txfifo disabled
[     1.000000] plcom0: console
[     1.000000] plcom0: interrupting on icu irq 57
[     1.000000] bcmsdhost0 at simplebus1: SD HOST controller
[     1.000000] bcmsdhost0: interrupting on icu irq 56
[     1.000000] bsciic1 at simplebus1: Broadcom Serial Controller
[     1.000000] iic1 at bsciic1: I2C bus
[     1.000000] bsciic2 at simplebus1: Broadcom Serial Controller
[     1.000000] iic2 at bsciic2: I2C bus
[     1.000000] /soc/vec@7e806000 at simplebus1 not configured
[     1.000000] /soc/hdmi@7e902000 at simplebus1 not configured
[     1.000000] dwctwo0 at simplebus1: USB controller
[     1.000000] dwctwo0: interrupting on icu irq 9
[     1.000000] /soc/gpu at simplebus1 not configured
[     1.000000] genfb0 at simplebus1: switching to framebuffer console
[     1.000000] genfb0: framebuffer at 0x5e876000, size 1280x720, depth 32, stride 5120
[     1.000000] wsdisplay0 at genfb0 kbdmux 1: console (default, vt100 emulation)
[     1.000000] wsmux1: connecting to wsdisplay0
[     1.000000] wsdisplay0: screen 1-3 added (default, vt100 emulation)
[     1.000000] vchiq0 at simplebus1: BCM2835 VCHIQ
[     1.000000] /arm-pmu at simplebus0 not configured
[     1.000000] gpioleds0 at simplebus0: ACT
[     1.000000] /soc/txp@7e004000 at simplebus1 not configured
[     1.000000] bcmrng0 at simplebus1: RNG
[     1.000000] timecounter: Timecounter "clockinterrupt" frequency 100 Hz quality 0
[     1.000003] timecounter: Timecounter "bcmtmr0" frequency 1000000 Hz quality 100
[     1.050020] sdmmc0 at bcmsdhost0
[     1.050020] dwctwo0: Core Release: 2.80a (snpsid=4f54280a)
[     1.050020] usb0 at dwctwo0: USB revision 2.0
[     1.110148] uhub0 at usb0: NetBSD (0000) DWC2 root hub (0000), class 9/0, rev 2.00/1.00, addr 1
[     1.110148] uhub0: 1 port with 1 removable, self powered
[     1.120174] IPsec: Initialized Security Association Processing.
[     1.150235] sdmmc0: direct I/O error 5, r=6 p=0x9bd91f2c write
[     1.160249] sdmmc0: SD card status: 4-bit, C10, U1, V10, A1
[     1.160249] ld0 at sdmmc0: <0x27:0x5048:SD64G:0x60:0xda7f8e63:0x14a>
[     1.160249] ld0: 59616 MB, 7599 cyl, 255 head, 63 sec, 512 bytes/sect x 122093568 sectors
[     1.160249] ld0: 4-bit width, High-Speed/SDR25, 50.000 MHz
[     2.941461] uhub1 at uhub0 port 1: vendor 214b (0x214b) USB2.0 HUB (0x7250), class 9/0, rev 2.00/1.00, addr 2
[     2.941461] uhub1: single transaction translator
[     2.941461] uhub1: 4 ports with 4 removable, self powered
[     4.252284] ure0 at uhub1 port 1
[     4.252284] ure0: Realtek (0xbda) USB 10/100 LAN (0x8152), rev 2.10/20.00, addr 3
[     4.252284] ure0: RTL8152 ver 4c10
[     4.312354] rlphy0 at ure0 phy 0: RTL8201E 10/100 media interface, rev. 2
[     4.322369] rlphy0: 10baseT, 10baseT-FDX, 100baseTX, 100baseTX-FDX, auto
[     4.332562] ure0: Ethernet address 00:e0:4c:36:03:b4
[     4.332562] uhub0: autoconfiguration error: illegal enable change, port 1
[     4.332562] WARNING: 1 error while detecting hardware; check system log.
[     4.332562] boot device: ld0
[     4.332562] root on ld0a dumps on ld0b
[     4.342577] root file system type: ffs
[     4.342577] kern.module.path=/stand/evbarm/9.1/modules
[     4.362615] vchiq0: interrupting on icu irq 66
[     4.362615] vchiq: vchiq_init_state: slot_zero = 0xc97e0000, is_master = 0
[     4.362615] vchiq: local ver 8 (min 3), remote ver 8.
[     4.362615] vcaudio0 at vchiq0: auds
[     4.362615] WARNING: no TOD clock present
[     4.362615] WARNING: using filesystem time
[     4.369164] WARNING: CHECK AND RESET THE DATE!
[     4.372629] audio0 at vcaudio0: playback, capture, half duplex, independent
[     4.372629] audio0: slinear_le:16 -> slinear_le:16 2ch 48000Hz, blk 7680 bytes (40ms) for playback
[     4.372629] audio0: slinear_le:16 2ch 48000Hz, blk 7680 bytes (40ms) for recording
[     4.372629] spkr0 at audio0: PC Speaker (synthesized)
[     4.372629] wsbell at spkr0 not configured
[    12.868235] wsdisplay0: screen 4 added (default, vt100 emulation)
```

## Logging in to NetBSD

The default account for the images are **root** without password.

You must set the password and configure **ssh** to allow root login before you can log in remotely.

```shell
# vi /etc/ssh/sshd_config

# /etc/rc.d/sshd reload
Reloading sshd config files.
```

## System characteristics

### uname

```shell
# uname -snrmp
NetBSD rpi 9.1 evbarm earmv6hf

# uname -ap
NetBSD rpi 9.1 NetBSD 9.1 (RPI) #0: Sun Oct 18 19:24:30 UTC 2020  mkrepro@mkrepro.NetBSD.org:/usr/src/sys/arch/evbarm/compile/RPI evbarm earmv6hf
```

### cpu

```shell
# dmesg | grep cpu
[     1.000000] cpus0 at simplebus0
[     1.000000] cpu0 at cpus0: 700 MHz ARM1176JZ-S r0p7 (ARM11J V6ZK core)
[     1.000000] cpu0: DC enabled IC enabled WB enabled LABT
[     1.000000] cpu0: 16KB/32B 4-way L1 VIPT Instruction cache
[     1.000000] cpu0: 16KB/32B 4-way write-back-locking-C L1 VIPT Data cache
[     1.000000] vfp0 at cpu0: VFP11, rounding, exceptions
[     1.000000] cpus0 at simplebus0
[     1.000000] cpu0 at cpus0: 700 MHz ARM1176JZ-S r0p7 (ARM11J V6ZK core)
[     1.000000] cpu0: DC enabled IC enabled WB enabled LABT
[     1.000000] cpu0: 16KB/32B 4-way L1 VIPT Instruction cache
[     1.000000] cpu0: 16KB/32B 4-way write-back-locking-C L1 VIPT Data cache
[     1.000000] vfp0 at cpu0: VFP11, rounding, exceptions
```

### memory

```shell
# cat /proc/meminfo
        total:    used:    free:  shared: buffers: cached:
Mem:  456818688 108863488 347955200        0 41213952 72589312
Swap:        0        0        0
MemTotal:    446112 kB
MemFree:     339800 kB
MemShared:        0 kB
Buffers:      40248 kB
Cached:       70888 kB
SwapTotal:        0 kB
SwapFree:         0 kB
```

### ure

Realtek RTL8152 Based USB Ethernet Adapter

```shell
# dmesg | grep ure0
[     4.252284] ure0 at uhub1 port 1
[     4.252284] ure0: Realtek (0xbda) USB 10/100 LAN (0x8152), rev 2.10/20.00, addr 3
[     4.252284] ure0: RTL8152 ver 4c10
[     4.312354] rlphy0 at ure0 phy 0: RTL8201E 10/100 media interface, rev. 2
[     4.332562] ure0: Ethernet address 00:e0:4c:36:03:b4
```

### sysctl

```shell
hw.disknames = ld0
hw.machine = evbarm
hw.machine_arch = earmv6hf
hw.model = raspberrypi,model-zero
hw.ncpu = 1
hw.ncpuonline = 1
hw.pagesize = 8192
hw.physmem = 469762048
hw.physmem64 = 469762048
hw.usermem = 456302592
hw.usermem64 = 456302592
kern.aio_listio_max = 512
kern.aio_max = 8192
kern.argmax = 262144
kern.clockrate: tick = 10000, tickadj = 40, hz = 100, profhz = 100, stathz = 100
kern.configname = RPI
kern.consdev = ttyE0
kern.dump_on_panic = 1
kern.expose_address = 1
kern.forkfsleep = 0
kern.fscale = 2048
kern.fsync = 1
kern.hardclock_ticks = 294254
kern.iov_max = 1024
kern.mbuf.mblowat = 16
kern.mbuf.mclbytes = 2048
kern.mbuf.mcllowat = 8
kern.mbuf.msize = 256
kern.mbuf.nmbclusters = 14336
kern.osrelease = 9.1
kern.osrevision = 901000000
kern.ostype = NetBSD
kern.timecounter.choice = bcmtmr0(q=100, f=1000000 Hz) clockinterrupt(q=0, f=100 Hz) dummy(q=-1000000, f=1000000 Hz)
kern.timecounter.hardware = bcmtmr0
kern.timecounter.timestepwarnings = 0
kern.version = NetBSD 9.1 (RPI) #0: Sun Oct 18 19:24:30 UTC 2020
machdep.board_model = 0
machdep.board_revision = 9437331
machdep.booted_device = ld0
machdep.cpu.frequency.available = 700 1000
machdep.cpu.frequency.current = 1000
machdep.cpu.frequency.max = 1000
machdep.cpu.frequency.min = 700
machdep.cpu.frequency.target = 1000
machdep.cpu_arch = 6ZK
machdep.cpu_id = 1091549031
machdep.firmware_revision = 1536600398
machdep.fpu_id = 1090592949
machdep.fpu_present = 1
machdep.simd_present = 1
machdep.simdex_present = 1
machdep.synchprim_present = 32
```

### mmc

```shell
# dmesg | grep mmc
[     1.050020] sdmmc0 at bcmsdhost0
[     1.150235] sdmmc0: direct I/O error 5, r=6 p=0x9bd91f2c write
[     1.160249] sdmmc0: SD card status: 4-bit, C10, U1, V10, A1
[     1.160249] ld0 at sdmmc0: <0x27:0x5048:SD64G:0x60:0xda7f8e63:0x14a>
```

### mounts

```shell
# cat /proc/mounts
/dev/ld0a / ffs rw,noatime 0 0
/dev/ld0e /boot msdos rw 0 0
kernfs /kern kernfs rw 0 0
ptyfs /dev/pts ptyfs rw 0 0
procfs /proc proc rw 0 0
tmpfs /var/shm tmpfs rw 0 0
```

### fdisk

```shell
# fdisk /dev/rld0
Disk: /dev/rld0
NetBSD disklabel disk geometry:
cylinders: 7599, heads: 255, sectors/track: 63 (16065 sectors/cylinder)
total sectors: 122093568, bytes/sector: 512

BIOS disk geometry:
cylinders: 1023, heads: 255, sectors/track: 63 (16065 sectors/cylinder)
total sectors: 122093568

Partitions aligned to 2048 sector boundaries, offset 63

Partition table:
0: Primary DOS with 32 bit FAT - LBA (sysid 12)
    start 32768, size 163840 (80 MB, Cyls 2/10/9-12/60/48), Active
1: NetBSD (sysid 169)
    start 196608, size 121896960 (59520 MB, Cyls 12/60/49-7599/248/9)
        PBR is not bootable: All bytes are identical (0x00)
2: <UNUSED>
3: <UNUSED>
First active partition: 0
Drive serial number: 0 (0x00000000)
```

### disklabel

```shell
# disklabel /dev/ld0
# /dev/ld0:
type: SCSI
disk: STORAGE DEVICE
label: fictitious
flags: removable
bytes/sector: 512
sectors/track: 32
tracks/cylinder: 64
sectors/cylinder: 2048
cylinders: 1154
total sectors: 122093568
rpm: 3600
interleave: 1
trackskew: 0
cylinderskew: 0
headswitch: 0           # microseconds
track-to-track seek: 0  # microseconds
drivedata: 0

8 partitions:
#        size    offset     fstype [fsize bsize cpg/sgs]
 a: 121896960    196608     4.2BSD      0     0     0  # (Cyl.     96 -  59615)
 c: 122093568         0     unused      0     0        # (Cyl.      0 -  59615)
 d: 122093568         0     unused      0     0        # (Cyl.      0 -  59615)
 e:    163840     32768      MSDOS                     # (Cyl.     16 -     95)
```

## List of processes after startup

```shell
# ps -axdl
UID PID PPID   CPU PRI NI   VSZ   RSS WCHAN   STAT TTY      TIME COMMAND
  0   0    0     0 125  0     0  8232 uvm     DKl  ?     0:07.74 [system]
  0   1    0 18731  81  0  7496  1760 wait    Is   ?     0:00.05 - init
  0 145    1     0  85  0  8816  1752 kqueue  Is   ?     0:00.01 |-- dhcpcd: [master] [ip4] [ip6]
  0 196    1     0  85  0 12080  2680 kqueue  Ss   ?     0:00.18 |-- /usr/sbin/syslogd -s
  0 370    1  7829  84  0  7952  1080 devmon  Is   ?     0:00.00 |-- /sbin/devpubd
  0 403    1     0  85  0 11000 13152 pause   Ss   ?     0:00.36 |-- /usr/sbin/ntpd -p /var/run/ntpd.pid -g
  0 389  403     0  85  0 11200  2880 netio   I    ?     0:00.12 | `-- ntpd: asynchronous dns resolver
  0 474    1 21896  80  0 16504  3272 select  Is   ?     0:00.02 |-- /usr/sbin/sshd
  0 629    1 17225  81  0  5816  1712 kqueue  Is   ?     0:00.01 |-- /usr/sbin/inetd -l
  0 752    1     0  85  0 15112  3056 kqueue  Is   ?     0:00.08 |-- /usr/libexec/postfix/master -w
 12 515  752     0  85  0 15768  4288 kqueue  I    ?     0:00.09 | |-- pickup -l -t unix -u
 12 748  752     0  85  0 14168  4320 kqueue  I    ?     0:00.09 | `-- qmgr -l -t unix -u
  0 765    1     0  85  0  6864  1896 nanoslp Ss   ?     0:00.02 |-- /usr/sbin/cron
  0 831    1     0  85  0 14984  4920 wait    Is   ttyE0 0:00.25 `-- login
  0 760  831     0  85  0  8200  2464 wait    S    ttyE0 0:00.36   `-- -sh
  0 948  760     0  43  0  5944  1672 -       O+   ttyE0 0:00.01     `-- ps -axdl
```

## ifconfig

```shell
# ifconfig -am
ure0: flags=0x8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        capabilities=3ff00<IP4CSUM_Rx,IP4CSUM_Tx,TCP4CSUM_Rx,TCP4CSUM_Tx>
        capabilities=3ff00<UDP4CSUM_Rx,UDP4CSUM_Tx,TCP6CSUM_Rx,TCP6CSUM_Tx>
        capabilities=3ff00<UDP6CSUM_Rx,UDP6CSUM_Tx>
        enabled=0
        ec_capabilities=1<VLAN_MTU>
        ec_enabled=0
        address: 00:e0:4c:36:03:b4
        media: Ethernet autoselect (100baseTX full-duplex)
        status: active
        supported Ethernet media:
                media 10baseT
                media 10baseT mediaopt full-duplex
                media 100baseTX
                media 100baseTX mediaopt full-duplex
                media autoselect
        inet6 fe80::8b76:8c43:8609:1582%ure0/64 flags 0x0 scopeid 0x1
        inet6 2409:8a55:8bc:dda0:364e:fd36:9aea:b84b/64 flags 0x0
        inet 192.168.1.10/24 broadcast 192.168.1.255 flags 0x0
lo0: flags=0x8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 33176
        inet6 ::1/128 flags 0x20<NODAD>
        inet6 fe80::1%lo0/64 flags 0x0 scopeid 0x2
        inet 127.0.0.1/8 flags 0x0
```

## Compiler

```shell
# ld -v
GNU ld (NetBSD Binutils nb1) 2.31.1

# gcc -v
Using built-in specs.
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/libexec/lto-wrapper
Target: armv6--netbsdelf-eabihf
Configured with: /usr/src/tools/gcc/../../external/gpl3/gcc/dist/configure --target=armv6--netbsdelf-eabihf --enable-long-long --enable-threads --with-bugurl=http://www.NetBSD.org/Misc/send-pr.html --with-pkgversion='NetBSD nb4 20200810' --with-system-zlib --without-isl --enable-__cxa_atexit --enable-libstdcxx-time=rt --enable-libstdcxx-threads --with-diagnostics-color=auto-if-env --with-default-libstdcxx-abi=new --with-mpc-lib=/var/obj/mknative/evbarm-earmv6hf/usr/src/external/lgpl3/mpc/lib/libmpc --with-mpfr-lib=/var/obj/mknative/evbarm-earmv6hf/usr/src/external/lgpl3/mpfr/lib/libmpfr --with-gmp-lib=/var/obj/mknative/evbarm-earmv6hf/usr/src/external/lgpl3/gmp/lib/libgmp --with-mpc-include=/usr/src/external/lgpl3/mpc/dist/src --with-mpfr-include=/usr/src/external/lgpl3/mpfr/dist/src --with-gmp-include=/usr/src/external/lgpl3/gmp/lib/libgmp/arch/arm --enable-tls --enable-initfini-array --disable-multilib --disable-libstdcxx-pch --build=armv6--netbsdelf-eabihf --host=armv6--netbsdelf-eabihf --with-sysroot=/var/obj/mknative/evbarm-earmv6hf/usr/src/destdir.evbarm
Thread model: posix
gcc version 7.5.0 (nb4 20200810)
```

## NetBSD Packages Collection

- https://www.pkgsrc.org/
- https://www.netbsd.org/docs/pkgsrc/using.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/README-all.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/emulators/qemu/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/inputmethod/fcitx/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/inputmethod/ibus/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/clang/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/gcc10/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/ghc88/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/go/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/lua54/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/LuaJIT2/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/nodejs/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/openjdk11/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/openjdk8/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/python38/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/rust/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/zig/README.html
- http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/net/avahi/README.html

```shell
export PKG_PATH="http://cdn.NetBSD.org/pub/pkgsrc/packages/$(uname -s)/$(uname -m)/$(uname -r | cut -f '1 2' -d.)/All/"
export PKG_PATH="http://cdn.netbsd.org/pub/pkgsrc/packages/NetBSD/earmv6hf/9.1/All/;http://cdn.netbsd.org/pub/pkgsrc/packages/NetBSD/earmv6hf/9.0/All/"

pkg_admin fetch-pkg-vulnerabilities

pkg_add -uv tmux bash pkgin avahi clang python38

pkg_info tmux

pkg_delete -r jpeg

cat << EOF > /usr/pkg/etc/pkgin/repositories.conf
#
# Pkgin repositories list
#
# Simply add repositories URIs one below the other
#
# WARNING: order matters, duplicates will not be added, if two
# repositories hold the same package, it will be fetched from
# the first one listed in this file.
#

http://cdn.netbsd.org/pub/pkgsrc/packages/NetBSD/earmv6hf/9.1/All/
http://cdn.netbsd.org/pub/pkgsrc/packages/NetBSD/earmv6hf/9.0/All/
EOF

pkgin update
pkgin upgrade
pkgin avail
pkgin list
pkgin search foo.*bar
pkgin install 'foo<5.0' bar baz
pkgin show-full-deps foo
pkgin show-rev-deps foo
pkgin requires foo
pkgin remove foo
pkgin autoremove
pkgin clean
pkgin export > my-packages
pkgin import my-packages
pkgin provides foo
pkgin pkg-content foo
```

## Conclusion

NetBSD 9.1 can use my Realtek RTL8152 Based USB Ethernet Adapter, it's works very good.
