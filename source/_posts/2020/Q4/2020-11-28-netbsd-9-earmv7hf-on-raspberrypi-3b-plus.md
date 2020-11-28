---
title: NetBSD 9 earmv7hf on Raspberry Pi 3 Model B Plus Rev 1.3
description: NetBSD 9 earmv7hf on Raspberry Pi 3 Model B Plus Rev 1.3
date: 2020-11-28 17:32:04
tags:
    - Operating system
    - NetBSD
categories: [Operating system, NetBSD]
permalink: netbsd-9-earmv7hf-on-raspberrypi-3b-plus
---

# NetBSD 9.1 earmv7hf on Raspberry Pi 3 Model B Plus Rev 1.3

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
$ aria2c https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-earmv7hf/binary/gzimg/armv7.img.gz

$ aria2c https://nycdn.netbsd.org/pub/NetBSD-daily/netbsd-9/latest/evbarm-earmv7hf/binary/gzimg/rpi.img.gz
```

## Write image to SDXC card

```shell
$ cat armv7.img.gz | gzip -cd | sudo dd of=/dev/sdb bs=4M oflag=sync status=progress; time sync
1219461120 bytes (1.2 GB, 1.1 GiB) copied, 71 s, 17.2 MB/s
0+18614 records in
0+18614 records out
1219756032 bytes (1.2 GB, 1.1 GiB) copied, 71.0196 s, 17.2 MB/s

real	0m0.004s
user	0m0.000s
sys	0m0.002s
```

## Booting Raspberry Pi

```shell
armv7# dmesg
[     1.000000] Copyright (c) 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005,
[     1.000000]     2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017,
[     1.000000]     2018, 2019, 2020 The NetBSD Foundation, Inc.  All rights reserved.
[     1.000000] Copyright (c) 1982, 1986, 1989, 1991, 1993
[     1.000000]     The Regents of the University of California.  All rights reserved.

[     1.000000] NetBSD 9.1 (GENERIC) #0: Sun Oct 18 19:24:30 UTC 2020
[     1.000000]         mkrepro@mkrepro.NetBSD.org:/usr/src/sys/arch/evbarm/compile/GENERIC
[     1.000000] total memory = 948 MB
[     1.000000] avail memory = 928 MB
[     1.000000] timecounter: Timecounters tick every 10.000 msec
[     1.000000] running cgd selftest aes-xts-256 aes-xts-512 done
[     1.000000] armfdt0 (root)
[     1.000000] simplebus0 at armfdt0: Raspberry Pi 3 Model B Plus Rev 1.3
[     1.000000] simplebus1 at simplebus0
[     1.000000] simplebus2 at simplebus0
[     1.000000] simplebus3 at simplebus1
[     1.000000] cpus0 at simplebus0
[     1.000000] simplebus4 at simplebus0
[     1.000000] cpu0 at cpus0: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[     1.000000] cpu0: DC enabled IC enabled WB enabled EABT branch prediction enabled
[     1.000000] cpu0: 32KB/64B 2-way L1 VIPT Instruction cache
[     1.000000] cpu0: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[     1.000000] cpu0: 512KB/64B 16-way write-through L2 PIPT Unified cache
[     1.000000] vfp0 at cpu0: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[     1.000000] cpu1 at cpus0
[     1.000000] cpu2 at cpus0
[     1.000000] cpu3 at cpus0
[     1.000000] bcmicu0 at simplebus1
[     1.000000] bcmicu1 at simplebus1: Multiprocessor
[     1.000000] bcmcprman0 at simplebus1: BCM283x Clock Controller
[     1.000000] fclock0 at simplebus2: 19200000 Hz fixed clock (osc)
[     1.000000] bcmaux0 at simplebus1
[     1.000000] fclock1 at simplebus2: 480000000 Hz fixed clock (otg)
[     1.000000] gtmr0 at simplebus0: Generic Timer
[     1.000000] gtmr0: interrupting on local_intc irq 3
[     1.000000] armgtmr0 at gtmr0: Generic Timer (19200 kHz, virtual)
[     1.000000] timecounter: Timecounter "armgtmr0" frequency 19200000 Hz quality 500
[     1.000003] plcom0 at simplebus1: ARM PL011 UART
[     1.000003] plcom0: txfifo disabled
[     1.000003] plcom0: interrupting on icu irq 185
[     1.000003] com0 at simplebus1: BCM AUX UART, working fifo
[     1.000003] com0: console
[     1.000003] com0: interrupting on icu irq 157
[     1.000003] usbnopphy0 at simplebus0: USB PHY
[     1.000003] /soc/thermal@7e212000 at simplebus1 not configured
[     1.000003] /soc/dsi@7e209000 at simplebus1 not configured
[     1.000003] bcmgpio0 at simplebus1: GPIO controller
[     1.000003] bcmgpio0: pins 0..31 interrupting on icu irq 177
[     1.000003] bcmgpio0: pins 32..54 interrupting on icu irq 178
[     1.000003] gpio0 at bcmgpio0: 54 pins
[     1.000003] /soc/firmware/gpio at simplebus3 not configured
[     1.000003] bcmdmac0 at simplebus1: DMA0 DMA2 DMA4 DMA5 DMA8 DMA9 DMA10
[     1.000003] /soc/power at simplebus1 not configured
[     1.000003] mmcpwrseq0 at simplebus0autoconfiguration error: : couldn't get reset GPIOs
[     1.000003] bsciic0 at simplebus1: Broadcom Serial Controller
[     1.000003] iic0 at bsciic0: I2C bus
[     1.000003] bcmpmwdog0 at simplebus1: Power management, Reset and Watchdog controller
[     1.000003] bcmmbox0 at simplebus1: VC mailbox
[     1.000003] bcmmbox0: interrupting on icu irq 193
[     1.000003] vcmbox0 at bcmmbox0
[     1.000003] bcmsdhost0 at simplebus1: SD HOST controller
[     1.000003] bcmsdhost0: interrupting on icu irq 184
[     1.000003] bsciic1 at simplebus1: Broadcom Serial Controller
[     1.000003] iic1 at bsciic1: I2C bus
[     1.000003] /soc/pwm@7e20c000 at simplebus1 not configured
[     1.000003] sdhc0 at simplebus1: SDHC controller
[     1.000003] sdhc0: interrupting on icu irq 190
[     1.000003] bsciic2 at simplebus1: Broadcom Serial Controller
[     1.000003] iic2 at bsciic2: I2C bus
[     1.000003] /soc/vec@7e806000 at simplebus1 not configured
[     1.000003] /soc/hdmi@7e902000 at simplebus1 not configured
[     1.000003] dwctwo0 at simplebus1: USB controller
[     1.000003] dwctwo0: interrupting on icu irq 137
[     1.000003] /soc/gpu at simplebus1 not configured
[     1.000003] genfb0 at simplebus1: switching to framebuffer console
[     1.000003] genfb0: framebuffer at 0xfe402000, size 1920x1080, depth 32, stride 7680
[     1.000003] wsdisplay0 at genfb0 kbdmux 1: console (default, vt100 emulation)
[     1.000003] wsmux1: connecting to wsdisplay0
[     1.000003] wsdisplay0: screen 1-3 added (default, vt100 emulation)
[     1.000003] vchiq0 at simplebus1: BCM2835 VCHIQ
[     1.000003] armpmu0 at simplebus0: Performance Monitor Unit
[     1.000003] gpioleds0 at simplebus0: ACT
[     1.000003] /soc/timer@7e003000 at simplebus1 not configured
[     1.000003] /soc/txp@7e004000 at simplebus1 not configured
[     1.000003] bcmrng0 at simplebus1: RNG
[     1.000003] timecounter: Timecounter "clockinterrupt" frequency 100 Hz quality 0
[     1.000003] cpu3: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[     1.690633] cpu3: DC enabled IC enabled WB enabled EABT branch prediction enabled
[     1.730641] cpu3: 32KB/64B 2-way L1 VIPT Instruction cache
[     1.760639] cpu3: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[     1.800646] cpu3: 512KB/64B 16-way write-through L2 PIPT Unified cache
[     1.830647] vfp3 at cpu3: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[     1.870654] cpu2: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[     1.900657] cpu2: DC enabled IC enabled WB enabled EABT branch prediction enabled
[     1.940661] cpu2: 32KB/64B 2-way L1 VIPT Instruction cache
[     1.970664] cpu2: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[     2.010668] cpu2: 512KB/64B 16-way write-through L2 PIPT Unified cache
[     2.050672] vfp2 at cpu2: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[     2.090677] cpu1: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[     2.120683] cpu1: DC enabled IC enabled WB enabled EABT branch prediction enabled
[     2.160687] cpu1: 32KB/64B 2-way L1 VIPT Instruction cache
[     2.200690] cpu1: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[     2.240695] cpu1: 512KB/64B 16-way write-through L2 PIPT Unified cache
[     2.270698] vfp1 at cpu1: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[     2.430717] sdmmc0 at bcmsdhost0
[     2.430717] sdhc0: SDHC 3.0, rev 153, platform DMA, 200000 kHz, HS 3.3V, re-tuning mode 1, 1024 byte blocks
[     2.441378] sdmmc1 at sdhc0 slot 0
[     2.441378] dwctwo0: Core Release: 2.80a (snpsid=4f54280a)
[     2.441378] usb0 at dwctwo0: USB revision 2.0
[     2.480812] armpmu0: interrupting on local_intc irq 9
[     2.490811] uhub0 at usb0: NetBSD (0000) DWC2 root hub (0000), class 9/0, rev 2.00/1.00, addr 1
[     2.490811] uhub0: 1 port with 1 removable, self powered
[     2.520818] IPsec: Initialized Security Association Processing.
[     2.570820] sdmmc0: direct I/O error 5, r=6 p=0xba649f2c write
[     2.610826] sdmmc0: SD card status: 4-bit, C10, U1, V10, A1
[     2.610826] ld0 at sdmmc0: <0x27:0x5048:SD64G:0x60:0xda7f8e63:0x14a>
[     2.622107] ld0: 59616 MB, 7599 cyl, 255 head, 63 sec, 512 bytes/sect x 122093568 sectors
[     2.630830] ld0: 4-bit width, High-Speed/SDR25, 50.000 MHz
[     2.710838] sdmmc1: 4-bit width, 50.000 MHz
[     2.710838] sdmmc1: MAX_BLK_SIZE1 = 64
[     2.740844] sdmmc1: MAX_BLK_SIZE2 = 512
[     2.740844] sdmmc1: MAX_BLK_SIZE3 = 512
[     2.751137] sdmmc1: SDIO function
[     2.751137] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 1 not configured
[     2.762329] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 2 not configured
[     2.762329] (manufacturer 0x2d0, product 0xa9a6, standard function interface code 0x2) at sdmmc1 function 3 not configured
[     4.311718] uhub1 at uhub0 port 1: vendor 0424 (0x424) product 2514 (0x2514), class 9/0, rev 2.00/b.b3, addr 2
[     4.321719] uhub1: multiple transaction translators
[     4.341722] uhub1: 4 ports with 3 removable, self powered
[     5.651837] uhub2 at uhub1 port 1: vendor 0424 (0x424) product 2514 (0x2514), class 9/0, rev 2.00/b.b3, addr 3
[     5.681842] uhub2: multiple transaction translators
[     5.681842] uhub2: 3 ports with 2 removable, self powered
[     6.992012] uhub3 at uhub2 port 3: vendor 214b (0x214b) USB2.0 HUB (0x7250), class 9/0, rev 2.00/1.00, addr 4
[     7.002603] uhub3: single transaction translator
[     7.002603] uhub3: 4 ports with 4 removable, self powered
[     8.312188] ure0 at uhub3 port 1
[     8.312188] ure0: Realtek (0xbda) USB 10/100 LAN (0x8152), rev 2.10/20.00, addr 5
[     8.342195] ure0: RTL8152 ver 4c10
[     8.422203] rlphy0 at ure0 phy 0: RTL8201E 10/100 media interface, rev. 2
[     8.442206] rlphy0: 10baseT, 10baseT-FDX, 100baseTX, 100baseTX-FDX, auto
[     8.442206] ure0: Ethernet address 00:e0:4c:36:03:b4
[     9.502347] uhidev0 at uhub1 port 3 configuration 1 interface 0
[     9.502347] uhidev0: SIGMACHIP (0x1c4f) USB Keyboard (0x02), rev 1.10/1.10, addr 6, iclass 3/1
[     9.552355] ukbd0 at uhidev0
[     9.962408] wskbd0 at ukbd0: console keyboard, using wsdisplay0
[     9.962408] uhidev1 at uhub1 port 3 configuration 1 interface 1
[     9.972501] uhidev1: SIGMACHIP (0x1c4f) USB Keyboard (0x02), rev 1.10/1.10, addr 6, iclass 3/0
[    10.032465] uhidev1: 2 report ids
[    10.032465] uhid0 at uhidev1 reportid 1: input=2, output=0, feature=0
[    10.032465] uhid1 at uhidev1 reportid 2: input=1, output=0, feature=0
[    10.044503] uhub0: autoconfiguration error: illegal enable change, port 1
[    11.023054] mue0 at uhub2 port 1
[    11.023054] mue0: vendor 0424 (0x424) product 7800 (0x7800), rev 2.10/3.00, addr 7
[    11.313092] mue0: LAN7800 id 0x7800 rev 0x2
[    11.343096] ukphy0 at mue0 phy 1: OUI 0x00800f, model 0x0013, rev. 2
[    11.343096] ukphy0: 10baseT, 10baseT-FDX, 100baseTX, 100baseTX-FDX, 1000baseT, 1000baseT-FDX, auto
[    11.353768] mue0: Ethernet address b8:27:eb:66:13:38
[    11.365842] WARNING: 2 errors while detecting hardware; check system log.
[    11.373771] boot device: ld0
[    11.393772] root on ld0a dumps on ld0b
[    11.403774] root file system type: ffs
[    11.413774] kern.module.path=/stand/evbarm/9.1/modules
[    11.413774] vchiq0: interrupting on icu irq 194
[    11.423776] vchiq: vchiq_init_state: slot_zero = 0xbaaa0000, is_master = 0
[    11.423776] vchiq: local ver 8 (min 3), remote ver 8.
[    11.423776] vcaudio0 at vchiq0: auds
[    11.423776] WARNING: no TOD clock present
[    11.423776] WARNING: using filesystem time
[    11.437738] WARNING: CHECK AND RESET THE DATE!
[    11.463779] audio0 at vcaudio0: playback, capture, half duplex, independent
[    11.473780] audio0: slinear_le:16 -> slinear_le:16 2ch 48000Hz, blk 7680 bytes (40ms) for playback
[    11.473780] audio0: slinear_le:16 2ch 48000Hz, blk 7680 bytes (40ms) for recording
[    11.483781] spkr0 at audio0: PC Speaker (synthesized)
[    11.493781] wsbell at spkr0 not configured
[    17.854670] wsdisplay0: screen 4 added (default, vt100 emulation)
```

## Logging in to NetBSD

The default account for the images are **root** without password.

You must set the password and configure **ssh** to allow root login before you can log in remotely.

## System characteristics

### uname

```shell
# uname -snrmp
NetBSD armv7 9.1 evbarm earmv7hf

# uname -ap
NetBSD armv7 9.1 NetBSD 9.1 (GENERIC) #0: Sun Oct 18 19:24:30 UTC 2020  mkrepro@mkrepro.NetBSD.org:/usr/src/sys/arch/evbarm/compile/GENERIC evbarm earmv7hf
```

### cpu

```shell
# dmesg | grep cpu
[     1.000000] cpus0 at simplebus0
[     1.000000] cpu0 at cpus0: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[     1.000000] cpu0: DC enabled IC enabled WB enabled EABT branch prediction enabled
[     1.000000] cpu0: 32KB/64B 2-way L1 VIPT Instruction cache
[     1.000000] cpu0: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[     1.000000] cpu0: 512KB/64B 16-way write-through L2 PIPT Unified cache
[     1.000000] vfp0 at cpu0: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[     1.000000] cpu1 at cpus0
[     1.000000] cpu2 at cpus0
[     1.000000] cpu3 at cpus0
[     1.000003] cpu3: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[     1.653655] cpu3: DC enabled IC enabled WB enabled EABT branch prediction enabled
[     1.693658] cpu3: 32KB/64B 2-way L1 VIPT Instruction cache
[     1.723660] cpu3: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[     1.763665] cpu3: 512KB/64B 16-way write-through L2 PIPT Unified cache
[     1.793668] vfp3 at cpu3: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[     1.833672] cpu1: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[     1.863676] cpu1: DC enabled IC enabled WB enabled EABT branch prediction enabled
[     1.903680] cpu1: 32KB/64B 2-way L1 VIPT Instruction cache
[     1.943685] cpu1: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[     1.973688] cpu1: 512KB/64B 16-way write-through L2 PIPT Unified cache
[     2.013692] vfp1 at cpu1: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[     2.053698] cpu2: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[     2.083702] cpu2: DC enabled IC enabled WB enabled EABT branch prediction enabled
[     2.123706] cpu2: 32KB/64B 2-way L1 VIPT Instruction cache
[     2.163712] cpu2: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[     2.203716] cpu2: 512KB/64B 16-way write-through L2 PIPT Unified cache
[     2.233718] vfp2 at cpu2: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
```

### memory

```shell
# cat /proc/meminfo
        total:    used:    free:  shared: buffers: cached:
Mem:  974045184 261570560 712474624        0 196403200 234627072
Swap:        0        0        0
MemTotal:    951216 kB
MemFree:     695776 kB
MemShared:        0 kB
Buffers:     191800 kB
Cached:      229128 kB
SwapTotal:        0 kB
SwapFree:         0 kB
```

### ure

Realtek RTL8152 Based USB Ethernet Adapter

```shell
# dmesg | grep ure0
[     8.294435] ure0 at uhub3 port 1
[     8.294435] ure0: Realtek (0xbda) USB 10/100 LAN (0x8152), rev 2.10/20.00, addr 5
[     8.304437] ure0: RTL8152 ver 4c10
[     8.394448] rlphy0 at ure0 phy 0: RTL8201E 10/100 media interface, rev. 2
[     8.444457] ure0: Ethernet address 00:e0:4c:36:03:b4

# dmesg | grep mue0
[    11.014923] mue0 at uhub2 port 1
[    11.014923] mue0: vendor 0424 (0x424) product 7800 (0x7800), rev 2.10/3.00, addr 7
[    11.304962] mue0: LAN7800 id 0x7800 rev 0x2
[    11.334966] ukphy0 at mue0 phy 1: OUI 0x00800f, model 0x0013, rev. 2
[    11.344968] mue0: Ethernet address b8:27:eb:66:13:38
```

### sysctl

```shell
hw.disknames = ld0
hw.machine = evbarm
hw.machine_arch = earmv7hf
hw.model = raspberrypi,3-model-b-plus
hw.ncpu = 4
hw.ncpuonline = 4
hw.pagesize = 8192
hw.physmem = 994050048
hw.physmem64 = 994050048
hw.usermem = 980459520
hw.usermem64 = 980459520


kern.aio_listio_max = 512
kern.aio_max = 8192
kern.argmax = 262144
kern.clockrate: tick = 10000, tickadj = 40, hz = 100, profhz = 100, stathz = 100
kern.configname = GENERIC
kern.consdev = ttyE0
kern.dump_on_panic = 1
kern.expose_address = 1
kern.forkfsleep = 0
kern.fscale = 2048
kern.fsync = 1
kern.hardclock_ticks = 88819
kern.iov_max = 1024
kern.mbuf.mblowat = 16
kern.mbuf.mclbytes = 2048
kern.mbuf.mcllowat = 8
kern.mbuf.msize = 256
kern.mbuf.nmbclusters = 30336
kern.osrelease = 9.1
kern.osrevision = 901000000
kern.ostype = NetBSD
kern.timecounter.choice = clockinterrupt(q=0, f=100 Hz) armgtmr0(q=500, f=19200000 Hz) dummy(q=-1000000, f=1000000 Hz)
kern.timecounter.hardware = armgtmr0
kern.timecounter.timestepwarnings = 0
kern.version = NetBSD 9.1 (GENERIC) #0: Sun Oct 18 19:24:30 UTC 2020
machdep.board_model = 0
machdep.board_revision = 10494163
machdep.booted_device = ld0
machdep.cpu.frequency.available = 600 1400
machdep.cpu.frequency.current = 1400
machdep.cpu.frequency.max = 1400
machdep.cpu.frequency.min = 600
machdep.cpu.frequency.target = 1400
machdep.cpu_arch = 8A
machdep.cpu_id = 1091555380
machdep.firmware_revision = 1536600398
machdep.fpu_id = 1090732084
machdep.fpu_present = 1
machdep.hwdiv_present = 1
machdep.kmpages = 3200
machdep.neon_present = 1
machdep.powersave = 1
machdep.printfataltraps = 0
machdep.serial = 0x66661338
machdep.simd_present = 1
machdep.simdex_present = 1
machdep.synchprim_present = 32
machdep.unaligned_sigbus = 0
```

### mmc

```shell
# dmesg | grep mmc
[     1.000003] mmcpwrseq0 at simplebus0autoconfiguration error: : couldn't get reset GPIOs
[     2.393736] sdmmc0 at bcmsdhost0
[     2.404406] sdmmc1 at sdhc0 slot 0
[     2.533852] sdmmc0: direct I/O error 5, r=6 p=0xba649f2c write
[     2.573857] sdmmc0: SD card status: 4-bit, C10, U1, V10, A1
[     2.573857] ld0 at sdmmc0: <0x27:0x5048:SD64G:0x60:0xda7f8e63:0x14a>
[     2.673869] sdmmc1: 4-bit width, 50.000 MHz
[     2.703876] sdmmc1: MAX_BLK_SIZE1 = 64
[     2.703876] sdmmc1: MAX_BLK_SIZE2 = 512
[     2.703876] sdmmc1: MAX_BLK_SIZE3 = 512
[     2.713874] sdmmc1: SDIO function
[     2.713874] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 1 not configured
[     2.723876] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 2 not configured
[     2.733877] (manufacturer 0x2d0, product 0xa9a6, standard function interface code 0x2) at sdmmc1 function 3 not configured

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
# fdisk: /dev/rld0
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
cylinders: 1163
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
  UID  PID PPID  CPU PRI NI   VSZ   RSS WCHAN   STAT TTY      TIME COMMAND
    0    0    0    0   0  0     0  8304 -       OKl  ?     1:05.99 [system]
    0    1    0   19  85  0  6808  1784 wait    Is   ?     0:00.02 - init
    0  242    1    0  85  0  8712  2352 kqueue  Ss   ?     0:00.27 |-- dhcpcd: [master] [ip4] [ip6]
32767  250    1    0  85  0  8208  2328 select  Ss   ?     0:00.23 |-- /usr/sbin/mdnsd
    0  291    1    0  85  0 10352  2680 kqueue  Ss   ?     0:00.28 |-- /usr/sbin/syslogd -s
    0  467    1    0  85  0 11824 13280 pause   Ss   ?     0:00.20 |-- /usr/sbin/ntpd -p /var/run/ntpd.pid -g
    0  558    1 3951  85  0  6208  1112 devmon  Is   ?     0:00.00 |-- /sbin/devpubd
    0  628    1    0  85  0 16776  6008 select  Is   ?     0:00.25 |-- /usr/sbin/sshd
    0  925  628    0  85  0 17120  6608 select  Ss   ?     0:00.42 | |-- sshd: root@pts/0 (sshd)
    0  105  925    0  85  0  7480  2320 ttyraw  Is+  pts/0 0:00.06 | | `-- -sh
    0  991  628    0  43  0 17904  6616 -       Ss   ?     0:00.51 | `-- sshd: root@pts/1 (sshd)
    0 1072  991    0  85  0  8152  2392 wait    Ss   pts/1 0:00.10 |   `-- -sh
    0  354 1072    0  43  0  7968  1664 -       O+   pts/1 0:00.01 |     `-- ps -axdl
    0  919    1 7174  84  0  7672  1712 kqueue  Is   ?     0:00.00 |-- /usr/sbin/inetd -l
    0  958    1    0  85  0  7064  1872 nanoslp Ss   ?     0:00.01 |-- /usr/sbin/cron
    0  981    1    0  85  0 15400  3088 kqueue  Is   ?     0:00.04 |-- /usr/libexec/postfix/master -w
   12  987  981    0  85  0 13904  4448 kqueue  I    ?     0:00.05 | |-- qmgr -l -t unix -u
   12 1041  981    0  85  0 13840  4432 kqueue  I    ?     0:00.02 | `-- pickup -l -t unix -u
    0  908    1  292  85  0 13528  4936 wait    Is   ttyE0 0:00.12 `-- login
    0  618  908    0  85  0  8104  2360 ttyraw  I+   ttyE0 0:00.14   `-- -sh
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
        media: Ethernet autoselect (none)
        status: no carrier
        supported Ethernet media:
                media 10baseT
                media 10baseT mediaopt full-duplex
                media 100baseTX
                media 100baseTX mediaopt full-duplex
                media autoselect
mue0: flags=0x8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        capabilities=7ff80<TSO4,IP4CSUM_Rx,IP4CSUM_Tx,TCP4CSUM_Rx>
        capabilities=7ff80<TCP4CSUM_Tx,UDP4CSUM_Rx,UDP4CSUM_Tx,TCP6CSUM_Rx>
        capabilities=7ff80<TCP6CSUM_Tx,UDP6CSUM_Rx,UDP6CSUM_Tx,TSO6>
        enabled=0
        ec_capabilities=1<VLAN_MTU>
        ec_enabled=0
        address: b8:27:eb:66:13:38
        media: Ethernet autoselect (100baseTX full-duplex)
        status: active
        supported Ethernet media:
                media none
                media 10baseT
                media 10baseT mediaopt full-duplex
                media 100baseTX
                media 100baseTX mediaopt full-duplex
                media 1000baseT
                media 1000baseT mediaopt full-duplex
                media autoselect
        inet6 fe80::60fc:82ab:c140:c7d1%mue0/64 flags 0x0 scopeid 0x2
        inet6 2409:8a55:8bc:dda0:a6cf:5bf6:c34e:b3de/64 flags 0x0
        inet 192.168.1.5/24 broadcast 192.168.1.255 flags 0x0
lo0: flags=0x8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 33176
        inet6 ::1/128 flags 0x20<NODAD>
        inet6 fe80::1%lo0/64 flags 0x0 scopeid 0x3
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
Target: armv7--netbsdelf-eabihf
Configured with: /usr/src/tools/gcc/../../external/gpl3/gcc/dist/configure --target=armv7--netbsdelf-eabihf --enable-long-long --enable-threads --with-bugurl=http://www.NetBSD.org/Misc/send-pr.html --with-pkgversion='NetBSD nb4 20200810' --with-system-zlib --without-isl --enable-__cxa_atexit --enable-libstdcxx-time=rt --enable-libstdcxx-threads --with-diagnostics-color=auto-if-env --with-default-libstdcxx-abi=new --with-mpc-lib=/var/obj/mknative/evbarm-earmv7hf/usr/src/external/lgpl3/mpc/lib/libmpc --with-mpfr-lib=/var/obj/mknative/evbarm-earmv7hf/usr/src/external/lgpl3/mpfr/lib/libmpfr --with-gmp-lib=/var/obj/mknative/evbarm-earmv7hf/usr/src/external/lgpl3/gmp/lib/libgmp --with-mpc-include=/usr/src/external/lgpl3/mpc/dist/src --with-mpfr-include=/usr/src/external/lgpl3/mpfr/dist/src --with-gmp-include=/usr/src/external/lgpl3/gmp/lib/libgmp/arch/arm --enable-tls --enable-initfini-array --disable-multilib --disable-libstdcxx-pch --build=armv7--netbsdelf-eabihf --host=armv7--netbsdelf-eabihf --with-sysroot=/var/obj/mknative/evbarm-earmv7hf/usr/src/destdir.evbarm
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
export PKG_PATH="http://cdn.netbsd.org/pub/pkgsrc/packages/NetBSD/earmv7hf/9.1/All/;http://cdn.netbsd.org/pub/pkgsrc/packages/NetBSD/earmv7hf/9.0/All/"

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

http://cdn.netbsd.org/pub/pkgsrc/packages/NetBSD/earmv7hf/9.1/All/
http://cdn.netbsd.org/pub/pkgsrc/packages/NetBSD/earmv7hf/9.0/All/
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

NetBSD 9.1 earmv7hf can use my Realtek RTL8152 Based USB Ethernet Adapter, it's works very good.
But is can't found WiFi Adapter in Raspberry Pi 3 Model B Plus Rev 1.3, it's really disappointing.
