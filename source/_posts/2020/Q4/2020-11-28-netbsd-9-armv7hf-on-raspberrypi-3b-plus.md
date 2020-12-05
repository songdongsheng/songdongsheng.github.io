---
title: NetBSD 9 armv7hf on Raspberry Pi 3B+
description: NetBSD 9 armv7hf on Raspberry Pi 3B+
date: 2020-11-28 17:32:04
tags:
    - Operating system
    - NetBSD
categories: [Operating system, NetBSD]
permalink: netbsd-9-armv7hf-on-raspberrypi-3b-plus
---

# NetBSD 9 armv7hf on Raspberry Pi 3B+

## Download NetBSD image

-   http://netbsd.org/
-   http://wiki.netbsd.org/ports/
-   http://wiki.netbsd.org/ports/evbarm/
-   http://wiki.netbsd.org/ports/aarch64/
-   http://wiki.netbsd.org/ports/evbmips/
-   https://www.netbsd.org/docs/guide/en/chap-boot.html
-   http://netbsd.org/releases/formal-9/NetBSD-9.1.html
-   https://man.netbsd.org/NetBSD-9.1/mue.4
-   https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-earm/INSTALL.html
-   https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-aarch64/binary/gzimg/arm64.img.gz
-   https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-earmv6hf/binary/gzimg/rpi.img.gz
-   https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-earmv7hf/binary/gzimg/armv7.img.gz
-   https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-amd64-install.img.gz
-   https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-amd64.iso
-   https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-evbarm-aarch64.iso
-   https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-evbarm-earmv6hf.iso
-   https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-evbarm-earmv7hf.iso
-   https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-evbmips-mips64el.iso
-   https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/images/NetBSD-9.1-evbmips-mipsel.iso

```shell
$ aria2c https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-earmv7hf/binary/gzimg/armv7.img.gz

$ aria2c https://nycdn.netbsd.org/pub/NetBSD-daily/netbsd-9/latest/evbarm-earmv7hf/binary/gzimg/rpi.img.gz
```

## Write image to SDXC card

```shell
$ cat armv7.img.gz | gzip -cd | sudo dd of=/dev/sdb bs=4M oflag=sync status=progress; time sync
1210023936 bytes (1.2 GB, 1.1 GiB) copied, 70 s, 17.3 MB/s
0+18614 records in
0+18614 records out
1219756032 bytes (1.2 GB, 1.1 GiB) copied, 70.581 s, 17.3 MB/s

real	0m0.015s
user	0m0.002s
sys	0m0.000s
```

## Booting Raspberry Pi

```shell
$ sudo minicom -b 115200 -8 -D /dev/ttyUSB0
```

```shell
armv7# dmesg
[   1.0000000] NetBSD/evbarm (fdt) booting ...
[   1.0000000] [ Kernel symbol table missing! ]
[   1.0000000] Copyright (c) 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005,
[   1.0000000]     2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017,
[   1.0000000]     2018, 2019, 2020 The NetBSD Foundation, Inc.  All rights reserved.
[   1.0000000] Copyright (c) 1982, 1986, 1989, 1991, 1993
[   1.0000000]     The Regents of the University of California.  All rights reserved.

[   1.0000000] NetBSD 9.1 (GENERIC) #0: Sun Oct 18 19:24:30 UTC 2020
[   1.0000000]  mkrepro@mkrepro.NetBSD.org:/usr/src/sys/arch/evbarm/compile/GENERIC
[   1.0000000] total memory = 948 MB
[   1.0000000] avail memory = 928 MB
[   1.0000000] running cgd selftest aes-xts-256 aes-xts-512 done
[   1.0000000] armfdt0 (root)
[   1.0000000] simplebus0 at armfdt0: Raspberry Pi 3 Model B Plus Rev 1.3
[   1.0000000] simplebus1 at simplebus0
[   1.0000000] simplebus2 at simplebus0
[   1.0000000] cpus0 at simplebus0
[   1.0000000] simplebus3 at simplebus0
[   1.0000000] cpu0 at cpus0: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[   1.0000000] cpu0: DC enabled IC enabled WB enabled EABT branch prediction enabled
[   1.0000000] cpu0: 32KB/64B 2-way L1 VIPT Instruction cache
[   1.0000000] cpu0: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[   1.0000000] cpu0: 512KB/64B 16-way write-through L2 PIPT Unified cache
[   1.0000000] vfp0 at cpu0: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[   1.0000000] cpu1 at cpus0
[   1.0000000] cpu2 at cpus0
[   1.0000000] cpu3 at cpus0
[   1.0000000] simplebus4 at simplebus1
[   1.0000000] bcmicu0 at simplebus1
[   1.0000000] bcmicu1 at simplebus1: Multiprocessor
[   1.0000000] bcmcprman0 at simplebus1: BCM283x Clock Controller
[   1.0000000] fclock0 at simplebus2: 19200000 Hz fixed clock (osc)
[   1.0000000] bcmaux0 at simplebus1
[   1.0000000] fclock1 at simplebus2: 480000000 Hz fixed clock (otg)
[   1.0000000] gtmr0 at simplebus0: Generic Timer
[   1.0000000] gtmr0: interrupting on local_intc irq 3
[   1.0000000] armgtmr0 at gtmr0: Generic Timer (19200 kHz, virtual)
[   1.0000030] plcom0 at simplebus1: ARM PL011 UART
[   1.0000030] plcom0: txfifo disabled
[   1.0000030] plcom0: interrupting on icu irq 185
[   1.0000030] com0 at simplebus1: BCM AUX UART, working fifo
[   1.0000030] com0: console
[   1.0000030] com0: interrupting on icu irq 157
[   1.0000030] usbnopphy0 at simplebus0: USB PHY
[   1.0000030] /soc/thermal@7e212000 at simplebus1 not configured
[   1.0000030] /soc/dsi@7e209000 at simplebus1 not configured
[   1.0000030] bcmgpio0 at simplebus1: GPIO controller
[   1.0000030] bcmgpio0: pins 0..31 interrupting on icu irq 177
[   1.0000030] bcmgpio0: pins 32..54 interrupting on icu irq 178
[   1.0000030] gpio0 at bcmgpio0: 54 pins
[   1.0000030] /soc/firmware/gpio at simplebus4 not configured
[   1.0000030] bcmdmac0 at simplebus1: DMA0 DMA2 DMA4 DMA5 DMA8 DMA9 DMA10
[   1.0000030] /soc/power at simplebus1 not configured
[   1.0000030] mmcpwrseq0 at simplebus0: couldn't get reset GPIOs
[   1.0000030] bsciic0 at simplebus1: Broadcom Serial Controller
[   1.0000030] iic0 at bsciic0: I2C bus
[   1.0000030] bcmpmwdog0 at simplebus1: Power management, Reset and Watchdog controller
[   1.0000030] bcmmbox0 at simplebus1: VC mailbox
[   1.0000030] bcmmbox0: interrupting on icu irq 193
[   1.0000030] vcmbox0 at bcmmbox0
[   1.0000030] bcmsdhost0 at simplebus1: SD HOST controller
[   1.0000030] bcmsdhost0: interrupting on icu irq 184
[   1.0000030] bsciic1 at simplebus1: Broadcom Serial Controller
[   1.0000030] iic1 at bsciic1: I2C bus
[   1.0000030] /soc/pwm@7e20c000 at simplebus1 not configured
[   1.0000030] sdhc0 at simplebus1: SDHC controller
[   1.0000030] sdhc0: interrupting on icu irq 190
[   1.0000030] bsciic2 at simplebus1: Broadcom Serial Controller
[   1.0000030] iic2 at bsciic2: I2C bus
[   1.0000030] /soc/vec@7e806000 at simplebus1 not configured
[   1.0000030] /soc/hdmi@7e902000 at simplebus1 not configured
[   1.0000030] dwctwo0 at simplebus1: USB controller
[   1.0000030] dwctwo0: interrupting on icu irq 137
[   1.0000030] /soc/gpu at simplebus1 not configured
[   1.0000030] genfb0 at simplebus1: switching to framebuffer console
[   1.0000030] wsdisplay0 at genfb0 kbdmux 1: console (default, vt100 emulation)
[   1.0000030] vchiq0 at simplebus1: BCM2835 VCHIQ
[   1.0000030] armpmu0 at simplebus0: Performance Monitor Unit
[   1.0000030] gpioleds0 at simplebus0: ACT
[   1.0000030] /soc/timer@7e003000 at simplebus1 not configured
[   1.0000030] /soc/txp@7e004000 at simplebus1 not configured
[   1.0000030] bcmrng0 at simplebus1: RNG
[   1.0000030] cpu3: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[   1.6565570] cpu3: DC enabled IC enabled WB enabled EABT branch prediction enabled
[   1.6965622] cpu3: 32KB/64B 2-way L1 VIPT Instruction cache
[   1.7265639] cpu3: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[   1.7665691] cpu3: 512KB/64B 16-way write-through L2 PIPT Unified cache
[   1.7965720] vfp3 at cpu3: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[   1.8365777] cpu2: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[   1.8665808] cpu2: DC enabled IC enabled WB enabled EABT branch prediction enabled
[   1.9065852] cpu2: 32KB/64B 2-way L1 VIPT Instruction cache
[   1.9465892] cpu2: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[   1.9765938] cpu2: 512KB/64B 16-way write-through L2 PIPT Unified cache
[   2.0165980] vfp2 at cpu2: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[   2.0566027] cpu1: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[   2.0866051] cpu1: DC enabled IC enabled WB enabled EABT branch prediction enabled
[   2.1266104] cpu1: 32KB/64B 2-way L1 VIPT Instruction cache
[   2.1666146] cpu1: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[   2.2066191] cpu1: 512KB/64B 16-way write-through L2 PIPT Unified cache
[   2.2366239] vfp1 at cpu1: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[   2.3966416] sdmmc0 at bcmsdhost0
[   2.3966416] sdhc0: SDHC 3.0, rev 153, platform DMA, 200000 kHz, HS 3.3V, re-tuning mode 1, 1024 byte blocks
[   2.4072849] sdmmc1 at sdhc0 slot 0
[   2.4072849] usb0 at dwctwo0: USB revision 2.0
[   2.4467208] armpmu0: interrupting on local_intc irq 9
[   2.4567205] uhub0 at usb0: NetBSD (0000) DWC2 root hub (0000), class 9/0, rev 2.00/1.00, addr 1
[   2.5367303] sdmmc0: direct I/O error 5, r=6 p=0xba649f2c write
[   2.5767364] sdmmc0: SD card status: 4-bit, C10, U1, V10, A1
[   2.5767364] ld0 at sdmmc0: <0x27:0x5048:SD64G:0x60:0xda7f8e63:0x14a>
[   2.5882678] ld0: 59616 MB, 7599 cyl, 255 head, 63 sec, 512 bytes/sect x 122093568 sectors
[   2.5967407] ld0: 4-bit width, High-Speed/SDR25, 50.000 MHz
[   2.6767485] sdmmc1: 4-bit width, 50.000 MHz
[   2.6767485] sdmmc1: MAX_BLK_SIZE1 = 64
[   2.7067548] sdmmc1: MAX_BLK_SIZE2 = 512
[   2.7067548] sdmmc1: MAX_BLK_SIZE3 = 512
[   2.7174104] sdmmc1: SDIO function
[   2.7174104] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 1 not configured
[   2.7271037] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 2 not configured
[   2.7271037] (manufacturer 0x2d0, product 0xa9a6, standard function interface code 0x2) at sdmmc1 function 3 not configured
[   4.2772411] uhub1 at uhub0 port 1: vendor 0424 (0x424) product 2514 (0x2514), class 9/0, rev 2.00/b.b3, addr 2
[   4.2772411] uhub1: multiple transaction translators
[   5.5973567] uhub2 at uhub1 port 1: vendor 0424 (0x424) product 2514 (0x2514), class 9/0, rev 2.00/b.b3, addr 3
[   5.5973567] uhub2: multiple transaction translators
[   5.9673979] uhub0: illegal enable change, port 1
[   5.9673979] WARNING: 2 errors while detecting hardware; check system log.
[   5.9773999] boot device: ld0
[   5.9773999] root on ld0a dumps on ld0b
[   6.0074035] root file system type: ffs
[   6.0174052] kern.module.path=/stand/evbarm/9.1/modules
[   6.0274058] vchiq0: interrupting on icu irq 194
[   6.0274058] vcaudio0 at vchiq0: auds
[   6.0374075] WARNING: no TOD clock present
[   6.0374075] WARNING: using filesystem time
[   6.0481341] WARNING: CHECK AND RESET THE DATE!
[   6.6974954] audio0 at vcaudio0: playback, capture, half duplex, independent
[   6.7074971] audio0: slinear_le:16 -> slinear_le:16 2ch 48000Hz, blk 7680 bytes (40ms) for playback
[   6.7174985] audio0: slinear_le:16 2ch 48000Hz, blk 7680 bytes (40ms) for recording
[   6.7274995] spkr0 at audio0: PC Speaker (synthesized)
[   6.7274995] wsbell at spkr0 not configured
Mon Oct 19 02:46:35 UTC 2020
Starting root file system check:
[   7.3775938] mue0 at uhub2 port 1
[   7.3775938] mue0: SMSC (0x424) LAN7800 USB 3.1 gigabit ethernet device (0x7800), rev 2.10/3.00, addr 4
/dev/rld0a: file system is clean; not checking
Growing ld0 MBR partition #1 (1067MB -> 59520MB)
Growing ld0 disklabel (1163MB -> 59616MB)
[   7.6876371] mue0: LAN7800 id 0x7800 rev 0x2
Resizing /
[   7.7176402] ukphy0 at mue0 phy 1: SMSC LAN8742 10/100 media interface (OUI 0x00800f, model 0x0013), rev. 2
[   7.7276432] ukphy0: 10baseT, 10baseT-FDX, 100baseTX, 100baseTX-FDX, 1000baseT, 1000baseT-FDX, auto
[   7.7376431] mue0: Ethernet address b8:27:eb:66:13:38
reboot: rebooted by root
[ 186.3959532] rebooting...
[   1.0000000] NetBSD/evbarm (fdt) booting ...
[   1.0000000] [ Kernel symbol table missing! ]
[   1.0000000] Copyright (c) 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005,
[   1.0000000]     2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017,
[   1.0000000]     2018, 2019, 2020 The NetBSD Foundation, Inc.  All rights reserved.
[   1.0000000] Copyright (c) 1982, 1986, 1989, 1991, 1993
[   1.0000000]     The Regents of the University of California.  All rights reserved.

[   1.0000000] NetBSD 9.1 (GENERIC) #0: Sun Oct 18 19:24:30 UTC 2020
[   1.0000000]  mkrepro@mkrepro.NetBSD.org:/usr/src/sys/arch/evbarm/compile/GENERIC
[   1.0000000] total memory = 948 MB
[   1.0000000] avail memory = 928 MB
[   1.0000000] running cgd selftest aes-xts-256 aes-xts-512 done
[   1.0000000] armfdt0 (root)
[   1.0000000] simplebus0 at armfdt0: Raspberry Pi 3 Model B Plus Rev 1.3
[   1.0000000] simplebus1 at simplebus0
[   1.0000000] simplebus2 at simplebus0
[   1.0000000] simplebus3 at simplebus1
[   1.0000000] cpus0 at simplebus0
[   1.0000000] simplebus4 at simplebus0
[   1.0000000] cpu0 at cpus0: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[   1.0000000] cpu0: DC enabled IC enabled WB enabled EABT branch prediction enabled
[   1.0000000] cpu0: 32KB/64B 2-way L1 VIPT Instruction cache
[   1.0000000] cpu0: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[   1.0000000] cpu0: 512KB/64B 16-way write-through L2 PIPT Unified cache
[   1.0000000] vfp0 at cpu0: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[   1.0000000] cpu1 at cpus0
[   1.0000000] cpu2 at cpus0
[   1.0000000] cpu3 at cpus0
[   1.0000000] bcmicu0 at simplebus1
[   1.0000000] bcmicu1 at simplebus1: Multiprocessor
[   1.0000000] bcmcprman0 at simplebus1: BCM283x Clock Controller
[   1.0000000] fclock0 at simplebus2: 19200000 Hz fixed clock (osc)
[   1.0000000] bcmaux0 at simplebus1
[   1.0000000] fclock1 at simplebus2: 480000000 Hz fixed clock (otg)
[   1.0000000] gtmr0 at simplebus0: Generic Timer
[   1.0000000] gtmr0: interrupting on local_intc irq 3
[   1.0000000] armgtmr0 at gtmr0: Generic Timer (19200 kHz, virtual)
[   1.0000030] plcom0 at simplebus1: ARM PL011 UART
[   1.0000030] plcom0: txfifo disabled
[   1.0000030] plcom0: interrupting on icu irq 185
[   1.0000030] com0 at simplebus1: BCM AUX UART, working fifo
[   1.0000030] com0: console
[   1.0000030] com0: interrupting on icu irq 157
[   1.0000030] usbnopphy0 at simplebus0: USB PHY
[   1.0000030] /soc/thermal@7e212000 at simplebus1 not configured
[   1.0000030] /soc/dsi@7e209000 at simplebus1 not configured
[   1.0000030] bcmgpio0 at simplebus1: GPIO controller
[   1.0000030] bcmgpio0: pins 0..31 interrupting on icu irq 177
[   1.0000030] bcmgpio0: pins 32..54 interrupting on icu irq 178
[   1.0000030] gpio0 at bcmgpio0: 54 pins
[   1.0000030] /soc/firmware/gpio at simplebus3 not configured
[   1.0000030] bcmdmac0 at simplebus1: DMA0 DMA2 DMA4 DMA5 DMA8 DMA9 DMA10
[   1.0000030] /soc/power at simplebus1 not configured
[   1.0000030] mmcpwrseq0 at simplebus0: couldn't get reset GPIOs
[   1.0000030] bsciic0 at simplebus1: Broadcom Serial Controller
[   1.0000030] iic0 at bsciic0: I2C bus
[   1.0000030] bcmpmwdog0 at simplebus1: Power management, Reset and Watchdog controller
[   1.0000030] bcmmbox0 at simplebus1: VC mailbox
[   1.0000030] bcmmbox0: interrupting on icu irq 193
[   1.0000030] vcmbox0 at bcmmbox0
[   1.0000030] bcmsdhost0 at simplebus1: SD HOST controller
[   1.0000030] bcmsdhost0: interrupting on icu irq 184
[   1.0000030] bsciic1 at simplebus1: Broadcom Serial Controller
[   1.0000030] iic1 at bsciic1: I2C bus
[   1.0000030] /soc/pwm@7e20c000 at simplebus1 not configured
[   1.0000030] sdhc0 at simplebus1: SDHC controller
[   1.0000030] sdhc0: interrupting on icu irq 190
[   1.0000030] bsciic2 at simplebus1: Broadcom Serial Controller
[   1.0000030] iic2 at bsciic2: I2C bus
[   1.0000030] /soc/vec@7e806000 at simplebus1 not configured
[   1.0000030] /soc/hdmi@7e902000 at simplebus1 not configured
[   1.0000030] dwctwo0 at simplebus1: USB controller
[   1.0000030] dwctwo0: interrupting on icu irq 137
[   1.0000030] /soc/gpu at simplebus1 not configured
[   1.0000030] genfb0 at simplebus1: switching to framebuffer console
[   1.0000030] wsdisplay0 at genfb0 kbdmux 1: console (default, vt100 emulation)
[   1.0000030] vchiq0 at simplebus1: BCM2835 VCHIQ
[   1.0000030] armpmu0 at simplebus0: Performance Monitor Unit
[   1.0000030] gpioleds0 at simplebus0: ACT
[   1.0000030] /soc/timer@7e003000 at simplebus1 not configured
[   1.0000030] /soc/txp@7e004000 at simplebus1 not configured
[   1.0000030] bcmrng0 at simplebus1: RNG
[   1.0000030] cpu3: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[   1.6869184] cpu3: DC enabled IC enabled WB enabled EABT branch prediction enabled
[   1.7269250] cpu3: 32KB/64B 2-way L1 VIPT Instruction cache
[   1.7569260] cpu3: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[   1.7969313] cpu3: 512KB/64B 16-way write-through L2 PIPT Unified cache
[   1.8269342] vfp3 at cpu3: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[   1.8669401] cpu2: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[   1.8969428] cpu2: DC enabled IC enabled WB enabled EABT branch prediction enabled
[   1.9369469] cpu2: 32KB/64B 2-way L1 VIPT Instruction cache
[   1.9769531] cpu2: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[   2.0069557] cpu2: 512KB/64B 16-way write-through L2 PIPT Unified cache
[   2.0469601] vfp2 at cpu2: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[   2.0869650] cpu1: 600 MHz Cortex-A53 r0p4 (Cortex V8A core)
[   2.1169696] cpu1: DC enabled IC enabled WB enabled EABT branch prediction enabled
[   2.1569736] cpu1: 32KB/64B 2-way L1 VIPT Instruction cache
[   2.1969777] cpu1: 32KB/64B 4-way write-back-locking-C L1 PIPT Data cache
[   2.2369832] cpu1: 512KB/64B 16-way write-through L2 PIPT Unified cache
[   2.2669872] vfp1 at cpu1: NEON MPE (VFP 3.0+), rounding, NaN propagation, denormals
[   2.4270045] sdmmc0 at bcmsdhost0
[   2.4270045] sdhc0: SDHC 3.0, rev 153, platform DMA, 200000 kHz, HS 3.3V, re-tuning mode 1, 1024 byte blocks
[   2.4376506] sdmmc1 at sdhc0 slot 0
[   2.4376506] usb0 at dwctwo0: USB revision 2.0
[   2.4770867] armpmu0: interrupting on local_intc irq 9
[   2.4870872] uhub0 at usb0: NetBSD (0000) DWC2 root hub (0000), class 9/0, rev 2.00/1.00, addr 1
[   2.5670959] sdmmc0: direct I/O error 5, r=6 p=0xba649f2c write
[   2.6071029] sdmmc0: SD card status: 4-bit, C10, U1, V10, A1
[   2.6171013] ld0 at sdmmc0: <0x27:0x5048:SD64G:0x60:0xda7f8e63:0x14a>
[   2.6171013] ld0: 59616 MB, 7599 cyl, 255 head, 63 sec, 512 bytes/sect x 122093568 sectors
[   2.6471071] ld0: 4-bit width, High-Speed/SDR25, 50.000 MHz
[   2.7271151] sdmmc1: 4-bit width, 50.000 MHz
[   2.7383915] sdmmc1: MAX_BLK_SIZE1 = 64
[   2.7383915] sdmmc1: MAX_BLK_SIZE2 = 512
[   2.7472047] sdmmc1: MAX_BLK_SIZE3 = 512
[   2.7472047] sdmmc1: SDIO function
[   2.7472047] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 1 not configured
[   2.7772101] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 2 not configured
[   2.7878890] (manufacturer 0x2d0, product 0xa9a6, standard function interface code 0x2) at sdmmc1 function 3 not configured
[   4.3080208] uhub1 at uhub0 port 1: vendor 0424 (0x424) product 2514 (0x2514), class 9/0, rev 2.00/b.b3, addr 2
[   4.3180228] uhub1: multiple transaction translators
[   5.6281963] uhub2 at uhub1 port 1: vendor 0424 (0x424) product 2514 (0x2514), class 9/0, rev 2.00/b.b3, addr 3
[   5.6381981] uhub2: multiple transaction translators
[   5.9982456] uhub0: illegal enable change, port 1
[   6.0082476] WARNING: 2 errors while detecting hardware; check system log.
[   6.0082476] boot device: ld0
[   6.0382519] root on ld0a dumps on ld0b
[   6.0482528] root file system type: ffs
[   6.0582542] kern.module.path=/stand/evbarm/9.1/modules
[   6.0582542] vchiq0: interrupting on icu irq 194
[   6.0682560] vcaudio0 at vchiq0: auds
[   6.0682560] WARNING: no TOD clock present
[   6.0682560] WARNING: using filesystem time
[   6.0815215] WARNING: CHECK AND RESET THE DATE!
[   6.8683619] audio0 at vcaudio0: playback, capture, half duplex, independent
[   6.8791971] audio0: slinear_le:16 -> slinear_le:16 2ch 48000Hz, blk 7680 bytes (40ms) for playback
[   6.8884522] audio0: slinear_le:16 2ch 48000Hz, blk 7680 bytes (40ms) for recording
[   6.8884522] spkr0 at audio0: PC Speaker (synthesized)
[   6.9184549] wsbell at spkr0 not configured
Mon Oct 19 02:46:37 UTC 2020
[   7.4185125] mue0 at uhub2 port 1
[   7.4185125] mue0: SMSC (0x424) LAN7800 USB 3.1 gigabit ethernet device (0x7800), rev 2.10/3.00, addr 4
Starting root file system check:
/dev/rld0a: file system is clean; not checking
fdisk: Cannot determine the number of heads
[   7.7185551] mue0: LAN7800 id 0x7800 rev 0x2
[   7.7285539] ukphy0 at mue0 phy 1: SMSC LAN8742 10/100 media interface (OUI 0x00800f, model 0x0013), rev. 2
[   7.7585544] ukphy0: 10baseT, 10baseT-FDX, 100baseTX, 100baseTX-FDX, 1000baseT, 1000baseT-FDX, auto
[   7.7697274] mue0: Ethernet address b8:27:eb:66:13:38
Not resizing /: already correct size
Starting file system checks:
/dev/rld0e: 229 files, 48492 free (12123 clusters)
random_seed: /var/db/entropy-file: Not present
Setting tty flags.
Setting sysctl variables:
ddb.onpanic: 1 -> 0
Starting network.
Hostname: armv7
IPv6 mode: host
Configuring network interfaces:.
Adding interface aliases:.
Waiting for DAD to complete for statically configured addresses...
Starting dhcpcd.
Starting mdnsd.
Building databases: dev, utmp, utmpx, services.
wsconscfg: screen 1 is already configured
wsconscfg: screen 2 is already configured
wsconscfg: screen 3 is already configured
Starting syslogd.
Mounting all file systems...
Clearing temporary files.
Updating fontconfig cache:Oct 19 02:46:51 armv7 dhcpcd[146]: mue0: failed to request information
Oct 19 02:46:51 armv7 dhcpcd[146]: mue0: failed to request information
 done.
Creating a.out runtime link editor directory cache.
Checking quotas: done.
Setting securelevel: kern.securelevel: 0 -> 1
Starting virecover.
Starting devpubd.
Starting local daemons:.
machdep.cpu.frequency.target: 600 -> 1400
Updating motd.
Starting ntpd.
ssh-keygen: 1024 SHA256:0uBBYuLHNxBj62wpppdqB4Apy+vqJhUfQ+DG2V0VDzI root@armv7 (DSA)
ssh-keygen: 521 SHA256:OnuZ63XAzG0KkGp1kB8Vp6ZRPjWwkX05fvw063Q7g7s root@armv7 (ECDSA)
ssh-keygen: 256 SHA256:dGZh3t1ihi2p21FToPz52+fgmS6Bn9bBDgx4WP31fT8 root@armv7 (ED25519)
ssh-keygen: 3072 SHA256:DsdSxrMYiM5h1rHx5uVgW38xL/dspSzaTXKTrY9vdNw root@armv7 (RSA)
Starting sshd.
postfix: rebuilding /etc/mail/aliases (missing /etc/mail/aliases.db)
Starting postfix.
Starting inetd.
Starting cron.
Mon Oct 19 02:47:35 UTC 2020

NetBSD/evbarm (armv7) (constty)

login:
```

## Logging in to NetBSD

The default account for the images are **root** without password.

You must set the password and configure **ssh** to allow root login before you can log in remotely.

```shell
NetBSD/evbarm (armv7) (constty)

login: root
Nov 30 15:08:07 armv7 login: ROOT LOGIN (root) on tty constty
Copyright (c) 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005,
    2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017,
    2018, 2019, 2020 The NetBSD Foundation, Inc.  All rights reserved.
Copyright (c) 1982, 1986, 1989, 1991, 1993
    The Regents of the University of California.  All rights reserved.

NetBSD 9.1 (GENERIC) #0: Sun Oct 18 19:24:30 UTC 2020

Welcome to NetBSD!

We recommend that you create a non-root account and use su(1) for root access.
armv7# passwd
Changing password for root.
New Password:
Retype New Password:
armv7#

# vi /etc/ssh/sshd_config
    PermitRootLogin yes
    #PermitRootLogin prohibit-password

# /etc/rc.d/sshd reload
Reloading sshd config files.
```

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
machdep.neon_present = 1
machdep.powersave = 1
machdep.printfataltraps = 0
machdep.serial = 0x66661338
machdep.simd_present = 1
machdep.simdex_present = 1
machdep.synchprim_present = 32
```

### mmc

````shell
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
armv7# ps -wxdlww
UID PID PPID   CPU PRI NI   VSZ   RSS WCHAN   STAT TTY      TIME COMMAND
  0   0    0     0   0  0     0  8264 -       OKl  ?     0:08.51 [system]
  0   1    0 19358  81  0  6248  1784 wait    Is   ?     0:00.04 - init
  0 146    1     0  85  0  8456  2424 kqueue  Ss   ?     0:00.20 |-- dhcpcd: [master] [ip4] [ip6]
  0 196    1     0  85  0 12232  2664 kqueue  Ss   ?     0:00.19 |-- /usr/sbin/syslogd -s
  0 378    1     0  85  0 11632 13304 pause   Ss   ?     0:00.17 |-- /usr/sbin/ntpd -p /var/run/ntpd.pid -g
  0 508    1     3  85  0 15152  6008 select  Is   ?     0:00.23 |-- /usr/sbin/sshd
  0 534    1  7488  84  0  6288  1120 devmon  Is   ?     0:00.00 |-- /sbin/devpubd
  0 778    1 15309  82  0  7216  1728 kqueue  Is   ?     0:00.00 |-- /usr/sbin/inetd -l
  0 857    1     0  85  0 15328  3080 kqueue  Is   ?     0:00.04 |-- /usr/libexec/postfix/master -w
  0 942    1     0  85  0  8160  1864 nanoslp Is   ?     0:00.01 |-- /usr/sbin/cron
  0 821    1   217  85  0 13640  4944 wait    Is   ttyE0 0:00.13 `-- login
  0 940  821     0  85  0  8760  2352 wait    S    ttyE0 0:00.17   `-- -sh
  0 692  940     0  42  0  5960  1664 -       O+   ttyE0 0:00.00     `-- ps -wxdlww
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

-   https://www.pkgsrc.org/
-   https://www.netbsd.org/docs/pkgsrc/using.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/README-all.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/emulators/qemu/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/inputmethod/fcitx/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/inputmethod/ibus/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/clang/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/gcc10/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/ghc88/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/go/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/lua54/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/LuaJIT2/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/nodejs/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/openjdk11/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/openjdk8/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/python38/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/rust/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/lang/zig/README.html
-   http://cdn.netbsd.org/pub/pkgsrc/current/pkgsrc/net/avahi/README.html

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
