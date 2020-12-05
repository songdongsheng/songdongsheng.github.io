---
title: NetBSD 9 aarch64 on Raspberry Pi 3B+
description: NetBSD 9 aarch64 on Raspberry Pi 3B+
date: 2020-11-28 23:08:17
tags:
    - Operating system
    - NetBSD
categories: [Operating system, NetBSD]
permalink: netbsd-9-aarch64-on-raspberrypi-3b-plus
---

# NetBSD 9 aarch64 on Raspberry Pi 3B+

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
$ aria2c https://cdn.netbsd.org/pub/NetBSD/NetBSD-9.1/evbarm-aarch64/binary/gzimg/arm64.img.gz

$ aria2c https://nycdn.netbsd.org/pub/NetBSD-daily/netbsd-9/latest/evbarm-aarch64/binary/gzimg/arm64.img.gz
```

## Write image to SDXC card

```shell
$ cat arm64.img.gz | gzip -cd | sudo dd of=/dev/sdb bs=4M oflag=sync status=progress; time sync
1193705472 bytes (1.2 GB, 1.1 GiB) copied, 69 s, 17.3 MB/s
0+18388 records in
0+18388 records out
1204551680 bytes (1.2 GB, 1.1 GiB) copied, 69.6269 s, 17.3 MB/s

real	0m0.006s
user	0m0.000s
sys	0m0.001s
```

## Booting Raspberry Pi

If you use USB TTL, don't forgot connect **GND**, and cross-connect **TXD** and **RXD**, i.e.:

1. Connect Gnd Pin of converter to Gnd of Raspberry Pi.
2. Connect TXD pin of converter to RXD0 of Raspberry Pi.
3. Connect RXD pin of converter to TXD0 of Raspberry Pi.

![Raspberry Pi 3 B+ Pins](raspberry_pi_pins.png)

You also need to setup the serial terminal to following specs:

1. Baud rate = 115200,
2. Bits = 8,
3. Parity = None,
4. Stop bits = 1 and
5. Flow control = None.

```shell
$ sudo minicom -b 115200 -8 -D /dev/ttyUSB0
```

```shell
[   1.0000000] NetBSD/evbarm (fdt) booting ...
[   1.0000000] Copyright (c) 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005,
[   1.0000000]     2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017,
[   1.0000000]     2018, 2019, 2020 The NetBSD Foundation, Inc.  All rights reserved.
[   1.0000000] Copyright (c) 1982, 1986, 1989, 1991, 1993
[   1.0000000]     The Regents of the University of California.  All rights reserved.

[   1.0000000] NetBSD 9.1 (GENERIC64) #0: Sun Oct 18 19:24:30 UTC 2020
[   1.0000000]  mkrepro@mkrepro.NetBSD.org:/usr/src/sys/arch/evbarm/compile/GENERIC64
[   1.0000000] total memory = 933 MB
[   1.0000000] avail memory = 900 MB
[   1.0000000] running cgd selftest aes-xts-256 aes-xts-512 done
[   1.0000000] armfdt0 (root)
[   1.0000000] simplebus0 at armfdt0: Raspberry Pi 3 Model B Plus Rev 1.3
[   1.0000000] simplebus1 at simplebus0
[   1.0000000] simplebus2 at simplebus0
[   1.0000000] cpus0 at simplebus0
[   1.0000000] simplebus3 at simplebus0
[   1.0000000] cpu0 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[   1.0000000] cpu0: package 0, core 0, smt 0
[   1.0000000] cpu0: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[   1.0000000] cpu0: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[   1.0000000] cpu0: Dcache line 64, Icache line 64
[   1.0000000] cpu0: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[   1.0000000] cpu0: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[   1.0000000] cpu0: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[   1.0000000] cpu0: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[   1.0000000] cpu0: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
[   1.0000000] cpu1 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[   1.0000000] cpu1: package 0, core 1, smt 0
[   1.0000000] cpu2 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[   1.0000000] cpu2: package 0, core 2, smt 0
[   1.0000000] cpu3 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[   1.0000000] cpu3: package 0, core 3, smt 0
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
[   1.0000030] mmcpwrseq0 at simplebus0: couldn't get reset GPIOs
[   1.0000030] /soc/thermal@7e212000 at simplebus1 not configured
[   1.0000030] /soc/dsi@7e209000 at simplebus1 not configured
[   1.0000030] bcmgpio0 at simplebus1: GPIO controller
[   1.0000030] bcmgpio0: pins 0..31 interrupting on icu irq 177
[   1.0000030] bcmgpio0: pins 32..54 interrupting on icu irq 178
[   1.0000030] gpio0 at bcmgpio0: 54 pins
[   1.0000030] /soc/firmware/gpio at simplebus4 not configured
[   1.0000030] bcmdmac0 at simplebus1: DMA0 DMA2 DMA4 DMA5 DMA8 DMA9 DMA10
[   1.0000030] /soc/power at simplebus1 not configured
[   1.0000030] bsciic0 at simplebus1: Broadcom Serial Controller
[   1.0000030] iic0 at bsciic0: I2C bus
[   1.0000030] /phy at simplebus0 not configured
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
[   1.0000030] /soc/mailbox@7e00b840 at simplebus1 not configured
[   1.0000030] armpmu0 at simplebus0: Performance Monitor Unit
[   1.0000030] gpioleds0 at simplebus0: ACT
[   1.0000030] /soc/timer@7e003000 at simplebus1 not configured
[   1.0000030] /soc/txp@7e004000 at simplebus1 not configured
[   1.0000030] bcmrng0 at simplebus1: RNG
[   1.0000030] cpu1: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[   1.3822156] cpu1: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[   1.4022184] cpu1: Dcache line 64, Icache line 64
[   1.4222202] cpu1: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[   1.4422221] cpu1: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[   1.4622237] cpu1: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[   1.4922276] cpu1: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[   1.5122299] cpu1: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
[   1.5522349] cpu2: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[   1.5722364] cpu2: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[   1.5922378] cpu2: Dcache line 64, Icache line 64
[   1.6122400] cpu2: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[   1.6322417] cpu2: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[   1.6622453] cpu2: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[   1.6922489] cpu2: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[   1.7122507] cpu2: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
[   1.7522545] cpu3: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[   1.7722565] cpu3: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[   1.8022588] cpu3: Dcache line 64, Icache line 64
[   1.8122601] cpu3: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[   1.8422639] cpu3: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[   1.8622659] cpu3: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[   1.8922684] cpu3: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[   1.9122707] cpu3: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
[   2.0122823] sdmmc0 at bcmsdhost0
[   2.0122823] sdhc0: SDHC 3.0, rev 153, platform DMA, 200000 kHz, HS 3.3V, re-tuning mode 1, 1024 byte blocks
[   2.0237139] sdmmc1 at sdhc0 slot 0
[   2.0237139] usb0 at dwctwo0: USB revision 2.0
[   2.0531467] armpmu0: interrupting on local_intc irq 9
[   2.0531467] uhub0 at usb0: NetBSD (0000) DWC2 root hub (0000), class 9/0, rev 2.00/1.00, addr 1
[   2.1331539] sdmmc0: direct I/O error 5, r=6 p=0xffffffc032d60e4c write
[   2.1531565] sdmmc0: SD card status: 4-bit, C10, U1, V10, A1
[   2.1631581] ld0 at sdmmc0: <0x27:0x5048:SD64G:0x60:0xda7f8e63:0x14a>
[   2.1731600] ld0: 59616 MB, 7599 cyl, 255 head, 63 sec, 512 bytes/sect x 122093568 sectors
[   2.1831597] ld0: 4-bit width, High-Speed/SDR25, 50.000 MHz
[   2.2531660] sdmmc1: 4-bit width, 50.000 MHz
[   2.2531660] sdmmc1: MAX_BLK_SIZE1 = 64
[   2.2531660] sdmmc1: MAX_BLK_SIZE2 = 512
[   2.2646882] sdmmc1: MAX_BLK_SIZE3 = 512
[   2.2646882] sdmmc1: SDIO function
[   2.2737676] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 1 not configured
[   2.2737676] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 2 not configured
[   2.2942676] (manufacturer 0x2d0, product 0xa9a6, standard function interface code 0x2) at sdmmc1 function 3 not configured
[   3.8843969] uhub1 at uhub0 port 1: vendor 0424 (0x424) product 2514 (0x2514), class 9/0, rev 2.00/b.b3, addr 2
[   3.8943974] uhub1: multiple transaction translators
[   5.2145070] uhub2 at uhub1 port 1: vendor 0424 (0x424) product 2514 (0x2514), class 9/0, rev 2.00/b.b3, addr 3
[   5.2245083] uhub2: multiple transaction translators
[   6.5446609] uhub3 at uhub2 port 3: vendor 214b (0x214b) USB2.0 HUB (0x7250), class 9/0, rev 2.00/1.00, addr 4
[   6.5546631] uhub3: single transaction translator
[   7.8748387] ure0 at uhub3 port 1
[   7.8848401] ure0: Realtek (0xbda) USB 10/100 LAN (0x8152), rev 2.10/20.00, addr 5
[   7.8948410] ure0: RTL8152 ver 4c10
[   7.9648503] rlphy0 at ure0 phy 0: RTL8201E 10/100 media interface, rev. 2
[   7.9848541] rlphy0: 10baseT, 10baseT-FDX, 100baseTX, 100baseTX-FDX, auto
[   7.9954319] ure0: Ethernet address 00:e0:4c:36:03:b4
[   9.0249916] uhidev0 at uhub1 port 3 configuration 1 interface 0
[   9.0249916] uhidev0: SIGMACHIP (0x1c4f) USB Keyboard (0x02), rev 1.10/1.10, addr 6, iclass 3/1
[   9.0749989] ukbd0 at uhidev0
[   9.4850529] wskbd0 at ukbd0: console keyboard, using wsdisplay0
[   9.4850529] uhidev1 at uhub1 port 3 configuration 1 interface 1
[   9.4951562] uhidev1: SIGMACHIP (0x1c4f) USB Keyboard (0x02), rev 1.10/1.10, addr 6, iclass 3/0
[   9.5351599] uhidev1: 2 report ids
[   9.5351599] uhid0 at uhidev1 reportid 1: input=2, output=0, feature=0
[   9.5453441] uhid1 at uhidev1 reportid 2: input=1, output=0, feature=0
[   9.5453441] uhub0: illegal enable change, port 1
[  10.5254420] mue0 at uhub2 port 1
[  10.5254420] mue0: vendor 0424 (0x424) product 7800 (0x7800), rev 2.10/3.00, addr 7
[  10.8154803] mue0: LAN7800 id 0x7800 rev 0x2
[  10.8254821] ukphy0 at mue0 phy 1: OUI 0x00800f, model 0x0013, rev. 2
[  10.8254821] ukphy0: 10baseT, 10baseT-FDX, 100baseTX, 100baseTX-FDX, 1000baseT, 1000baseT-FDX, auto
[  10.8359002] mue0: Ethernet address b8:27:eb:66:13:38
[  10.8559059] WARNING: 2 errors while detecting hardware; check system log.
[  10.8559059] boot device: ld0
[  10.8659071] root on ld0a dumps on ld0b
[  10.8759115] root file system type: ffs
[  10.8888685] kern.module.path=/stand/evbarm/9.1/modules
[  10.8967343] WARNING: no TOD clock present
[  10.8967343] WARNING: using filesystem time
[  10.9049328] WARNING: CHECK AND RESET THE DATE!
Mon Oct 19 02:20:53 UTC 2020
Starting root file system check:
/dev/rld0a: file system is clean; not checking
Growing ld0 MBR partition #1 (1052MB -> 59520MB)
Growing ld0 disklabel (1148MB -> 59616MB)
Resizing /
reboot: rebooted by root
[ 201.1052241] rebooting...
[   1.0000000] NetBSD/evbarm (fdt) booting ...
[   1.0000000] Copyright (c) 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005,
[   1.0000000]     2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017,
[   1.0000000]     2018, 2019, 2020 The NetBSD Foundation, Inc.  All rights reserved.
[   1.0000000] Copyright (c) 1982, 1986, 1989, 1991, 1993
[   1.0000000]     The Regents of the University of California.  All rights reserved.

[   1.0000000] NetBSD 9.1 (GENERIC64) #0: Sun Oct 18 19:24:30 UTC 2020
[   1.0000000]  mkrepro@mkrepro.NetBSD.org:/usr/src/sys/arch/evbarm/compile/GENERIC64
[   1.0000000] total memory = 933 MB
[   1.0000000] avail memory = 900 MB
[   1.0000000] running cgd selftest aes-xts-256 aes-xts-512 done
[   1.0000000] armfdt0 (root)
[   1.0000000] simplebus0 at armfdt0: Raspberry Pi 3 Model B Plus Rev 1.3
[   1.0000000] simplebus1 at simplebus0
[   1.0000000] simplebus2 at simplebus0
[   1.0000000] simplebus3 at simplebus1
[   1.0000000] cpus0 at simplebus0
[   1.0000000] simplebus4 at simplebus0
[   1.0000000] cpu0 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[   1.0000000] cpu0: package 0, core 0, smt 0
[   1.0000000] cpu0: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[   1.0000000] cpu0: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[   1.0000000] cpu0: Dcache line 64, Icache line 64
[   1.0000000] cpu0: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[   1.0000000] cpu0: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[   1.0000000] cpu0: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[   1.0000000] cpu0: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[   1.0000000] cpu0: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
[   1.0000000] cpu1 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[   1.0000000] cpu1: package 0, core 1, smt 0
[   1.0000000] cpu2 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[   1.0000000] cpu2: package 0, core 2, smt 0
[   1.0000000] cpu3 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[   1.0000000] cpu3: package 0, core 3, smt 0
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
[   1.0000030] mmcpwrseq0 at simplebus0: couldn't get reset GPIOs
[   1.0000030] /soc/thermal@7e212000 at simplebus1 not configured
[   1.0000030] /soc/dsi@7e209000 at simplebus1 not configured
[   1.0000030] bcmgpio0 at simplebus1: GPIO controller
[   1.0000030] bcmgpio0: pins 0..31 interrupting on icu irq 177
[   1.0000030] bcmgpio0: pins 32..54 interrupting on icu irq 178
[   1.0000030] gpio0 at bcmgpio0: 54 pins
[   1.0000030] /soc/firmware/gpio at simplebus3 not configured
[   1.0000030] bcmdmac0 at simplebus1: DMA0 DMA2 DMA4 DMA5 DMA8 DMA9 DMA10
[   1.0000030] /soc/power at simplebus1 not configured
[   1.0000030] bsciic0 at simplebus1: Broadcom Serial Controller
[   1.0000030] iic0 at bsciic0: I2C bus
[   1.0000030] /phy at simplebus0 not configured
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
[   1.0000030] /soc/mailbox@7e00b840 at simplebus1 not configured
[   1.0000030] armpmu0 at simplebus0: Performance Monitor Unit
[   1.0000030] gpioleds0 at simplebus0: ACT
[   1.0000030] /soc/timer@7e003000 at simplebus1 not configured
[   1.0000030] /soc/txp@7e004000 at simplebus1 not configured
[   1.0000030] bcmrng0 at simplebus1: RNG
[   1.0000030] cpu2: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[   1.4189881] cpu2: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[   1.4389903] cpu2: Dcache line 64, Icache line 64
[   1.4589926] cpu2: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[   1.4789951] cpu2: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[   1.4989968] cpu2: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[   1.5290001] cpu2: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[   1.5490016] cpu2: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
[   1.5890059] cpu1: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[   1.6090086] cpu1: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[   1.6290103] cpu1: Dcache line 64, Icache line 64
[   1.6490124] cpu1: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[   1.6690139] cpu1: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[   1.6990171] cpu1: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[   1.7290192] cpu1: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[   1.7490237] cpu1: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
[   1.7890273] cpu3: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[   1.8090291] cpu3: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[   1.8390323] cpu3: Dcache line 64, Icache line 64
[   1.8490327] cpu3: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[   1.8790361] cpu3: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[   1.8990385] cpu3: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[   1.9290404] cpu3: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[   1.9490427] cpu3: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
[   2.0590540] sdmmc0 at bcmsdhost0
[   2.0590540] sdhc0: SDHC 3.0, rev 153, platform DMA, 200000 kHz, HS 3.3V, re-tuning mode 1, 1024 byte blocks
[   2.0795260] sdmmc1 at sdhc0 slot 0
[   2.0795260] usb0 at dwctwo0: USB revision 2.0
[   2.0990667] armpmu0: interrupting on local_intc irq 9
[   2.1090670] uhub0 at usb0: NetBSD (0000) DWC2 root hub (0000), class 9/0, rev 2.00/1.00, addr 1
[   2.1790721] sdmmc0: direct I/O error 5, r=6 p=0xffffffc032d60e4c write
[   2.2090760] sdmmc0: SD card status: 4-bit, C10, U1, V10, A1
[   2.2090760] ld0 at sdmmc0: <0x27:0x5048:SD64G:0x60:0xda7f8e63:0x14a>
[   2.2190769] ld0: 59616 MB, 7599 cyl, 255 head, 63 sec, 512 bytes/sect x 122093568 sectors
[   2.2290770] ld0: 4-bit width, High-Speed/SDR25, 50.000 MHz
[   2.2990837] sdmmc1: 4-bit width, 50.000 MHz
[   2.2990837] sdmmc1: MAX_BLK_SIZE1 = 64
[   2.3101078] sdmmc1: MAX_BLK_SIZE2 = 512
[   2.3101078] sdmmc1: MAX_BLK_SIZE3 = 512
[   2.3101078] sdmmc1: SDIO function
[   2.3212059] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 1 not configured
[   2.3212059] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 2 not configured
[   2.3406603] (manufacturer 0x2d0, product 0xa9a6, standard function interface code 0x2) at sdmmc1 function 3 not configured
[   3.9607932] uhub1 at uhub0 port 1: vendor 0424 (0x424) product 2514 (0x2514), class 9/0, rev 2.00/b.b3, addr 2
[   3.9707941] uhub1: multiple transaction translators
[   5.2909041] uhub2 at uhub1 port 1: vendor 0424 (0x424) product 2514 (0x2514), class 9/0, rev 2.00/b.b3, addr 3
[   5.3009059] uhub2: multiple transaction translators
[   6.6310827] uhub3 at uhub2 port 3: vendor 214b (0x214b) USB2.0 HUB (0x7250), class 9/0, rev 2.00/1.00, addr 4
[   6.6410842] uhub3: single transaction translator
[   7.9612599] ure0 at uhub3 port 1
[   7.9612599] ure0: Realtek (0xbda) USB 10/100 LAN (0x8152), rev 2.10/20.00, addr 5
[   7.9812634] ure0: RTL8152 ver 4c10
[   8.0412708] rlphy0 at ure0 phy 0: RTL8201E 10/100 media interface, rev. 2
[   8.0612739] rlphy0: 10baseT, 10baseT-FDX, 100baseTX, 100baseTX-FDX, auto
[   8.0712753] ure0: Ethernet address 00:e0:4c:36:03:b4
[   9.1014122] uhidev0 at uhub1 port 3 configuration 1 interface 0
[   9.1014122] uhidev0: SIGMACHIP (0x1c4f) USB Keyboard (0x02), rev 1.10/1.10, addr 6, iclass 3/1
[   9.1514190] ukbd0 at uhidev0
[   9.5614737] wskbd0 at ukbd0: console keyboard, using wsdisplay0
[   9.5614737] uhidev1 at uhub1 port 3 configuration 1 interface 1
[   9.5715696] uhidev1: SIGMACHIP (0x1c4f) USB Keyboard (0x02), rev 1.10/1.10, addr 6, iclass 3/0
[   9.6115721] uhidev1: 2 report ids
[   9.6115721] uhid0 at uhidev1 reportid 1: input=2, output=0, feature=0
[   9.6115721] uhid1 at uhidev1 reportid 2: input=1, output=0, feature=0
[   9.6235892] uhub0: illegal enable change, port 1
[  10.6021321] mue0 at uhub2 port 1
[  10.6021321] mue0: vendor 0424 (0x424) product 7800 (0x7800), rev 2.10/3.00, addr 7
[  10.8921574] mue0: LAN7800 id 0x7800 rev 0x2
[  10.9021599] ukphy0 at mue0 phy 1: OUI 0x00800f, model 0x0013, rev. 2
[  10.9021599] ukphy0: 10baseT, 10baseT-FDX, 100baseTX, 100baseTX-FDX, 1000baseT, 1000baseT-FDX, auto
[  10.9128211] mue0: Ethernet address b8:27:eb:66:13:38
[  10.9328228] WARNING: 2 errors while detecting hardware; check system log.
[  10.9328228] boot device: ld0
[  10.9428244] root on ld0a dumps on ld0b
[  10.9528240] root file system type: ffs
[  10.9628260] kern.module.path=/stand/evbarm/9.1/modules
[  10.9628260] WARNING: no TOD clock present
[  10.9728256] WARNING: using filesystem time
[  10.9797371] WARNING: CHECK AND RESET THE DATE!
Mon Oct 19 02:20:54 UTC 2020
Starting root file system check:
/dev/rld0a: file system is clean; not checking
fdisk: Cannot determine the number of heads
Not resizing /: already correct size
Starting file system checks:
/dev/rld0e: 92 files, 61668 free (15417 clusters)
random_seed: /var/db/entropy-file: Not present
Setting tty flags.
Setting sysctl variables:
ddb.onpanic: 1 -> 0
Starting network.
Hostname: arm64
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
 done.
Checking quotas: done.
Setting securelevel: kern.securelevel: 0 -> 1
Starting virecover.
Starting devpubd.
Starting local daemons:.
Updating motd.
Starting ntpd.
ssh-keygen: 1024 SHA256:5qTSr5sYztHHHAeWznv1Q1KnaL83YIgPZLiC8Tbp5H0 root@arm64 (DSA)
ssh-keygen: 521 SHA256:CW5iq1aPZtxw/gWfYWf6gg4diG8h2lZQM3ZphX7+GZs root@arm64 (ECDSA)
ssh-keygen: 256 SHA256:9gyB3RMi0TzbFyt6rPzMXFkIddbrajegXnclh/wss4c root@arm64 (ED25519)
ssh-keygen: 3072 SHA256:fbRm+YiCX6zKYm1CNU2dY5Cm3+XKOfMVSsNm9xM0tno root@arm64 (RSA)
Starting sshd.
postfix: rebuilding /etc/mail/aliases (missing /etc/mail/aliases.db)
Starting postfix.
Starting inetd.
Starting cron.
Mon Oct 19 02:21:43 UTC 2020

NetBSD/evbarm (arm64) (constty)

login: root
Nov 29 14:57:30 arm64 login: ROOT LOGIN (root) on tty constty
Copyright (c) 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005,
    2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017,
    2018, 2019, 2020 The NetBSD Foundation, Inc.  All rights reserved.
Copyright (c) 1982, 1986, 1989, 1991, 1993
    The Regents of the University of California.  All rights reserved.

NetBSD 9.1 (GENERIC64) #0: Sun Oct 18 19:24:30 UTC 2020

Welcome to NetBSD!

We recommend that you create a non-root account and use su(1) for root access.
```

## Logging in to NetBSD

The default account for the images are **root** without password.

You must set the password and configure **ssh** to allow root login before you can log in remotely.

```shell
# vi /etc/ssh/sshd_config
    PermitRootLogin yes
    #PermitRootLogin prohibit-password

# /etc/rc.d/sshd reload
Reloading sshd config files.
```

## System characteristics

### dimension

![Raspberry Pi 3 B+ Dimension](rpi_machine_3bplus.pdf)
### uname

```shell
# uname -snrmp
NetBSD arm64 9.1 evbarm aarch64

# uname -ap
NetBSD arm64 9.1 NetBSD 9.1 (GENERIC64) #0: Sun Oct 18 19:24:30 UTC 2020  mkrepro@mkrepro.NetBSD.org:/usr/src/sys/arch/evbarm/compile/GENERIC64 evbarm aarch64
```

### cpu

```shell
# dmesg | grep cpu
[     1.000000] cpus0 at simplebus0
[     1.000000] cpu0 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[     1.000000] cpu0: package 0, core 0, smt 0
[     1.000000] cpu0: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[     1.000000] cpu0: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[     1.000000] cpu0: Dcache line 64, Icache line 64
[     1.000000] cpu0: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[     1.000000] cpu0: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[     1.000000] cpu0: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[     1.000000] cpu0: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[     1.000000] cpu0: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
[     1.000000] cpu1 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[     1.000000] cpu1: package 0, core 1, smt 0
[     1.000000] cpu2 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[     1.000000] cpu2: package 0, core 2, smt 0
[     1.000000] cpu3 at cpus0: Cortex-A53 r0p4 (Cortex V8-A core)
[     1.000000] cpu3: package 0, core 3, smt 0
[     1.000003] cpu1: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[     1.382216] cpu1: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[     1.402218] cpu1: Dcache line 64, Icache line 64
[     1.422220] cpu1: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[     1.442222] cpu1: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[     1.462224] cpu1: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[     1.492228] cpu1: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[     1.512230] cpu1: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
[     1.552235] cpu2: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[     1.572236] cpu2: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[     1.592238] cpu2: Dcache line 64, Icache line 64
[     1.612240] cpu2: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[     1.632242] cpu2: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[     1.662245] cpu2: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[     1.692249] cpu2: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[     1.712251] cpu2: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
[     1.752254] cpu3: IC enabled, DC enabled, EL0/EL1 stack Alignment check enabled
[     1.772256] cpu3: Cache Writeback Granule 16B, Exclusives Reservation Granule 16B
[     1.802259] cpu3: Dcache line 64, Icache line 64
[     1.812260] cpu3: L1 32KB/64B 2-way read-allocate VIPT Instruction cache
[     1.842264] cpu3: L1 32KB/64B 4-way write-back read-allocate write-allocate PIPT Data cache
[     1.862266] cpu3: L2 512KB/64B 16-way write-back read-allocate write-allocate PIPT Unified cache
[     1.892268] cpu3: revID=0x80, PMCv3, 4k table, 64k table, 16bit ASID
[     1.912271] cpu3: auxID=0x10000, FP, CRC32, NEON, rounding, NaN propagation, denormals, 32x64bitRegs, Fused Multiply-Add
```

### memory

```shell
# cat /proc/meminfo
        total:    used:    free:  shared: buffers: cached:
Mem:  944205824 275148800 669057024        0 191070208 235098112
Swap:        0        0        0
MemTotal:    922076 kB
MemFree:     653376 kB
MemShared:        0 kB
Buffers:     186592 kB
Cached:      229588 kB
SwapTotal:        0 kB
SwapFree:         0 kB
```

### ure

Realtek RTL8152 Based USB Ethernet Adapter

```shell
# dmesg | grep ure0
[     7.874839] ure0 at uhub3 port 1
[     7.884840] ure0: Realtek (0xbda) USB 10/100 LAN (0x8152), rev 2.10/20.00, addr 5
[     7.894841] ure0: RTL8152 ver 4c10
[     7.964850] rlphy0 at ure0 phy 0: RTL8201E 10/100 media interface, rev. 2
[     7.995432] ure0: Ethernet address 00:e0:4c:36:03:b4

# dmesg | grep mue0
[    10.525442] mue0 at uhub2 port 1
[    10.525442] mue0: vendor 0424 (0x424) product 7800 (0x7800), rev 2.10/3.00, addr 7
[    10.815480] mue0: LAN7800 id 0x7800 rev 0x2
[    10.825482] ukphy0 at mue0 phy 1: OUI 0x00800f, model 0x0013, rev. 2
[    10.835900] mue0: Ethernet address b8:27:eb:66:13:38
```

### sysctl

```shell
hw.disknames = ld0
hw.machine = evbarm
hw.machine_arch = aarch64
hw.model = raspberrypi,3-model-b-plus
hw.ncpu = 4
hw.ncpuonline = 4
hw.pagesize = 4096
hw.physmem = 979247104
hw.physmem64 = 979247104
hw.usermem = 955142144
hw.usermem64 = 955142144
kern.aio_listio_max = 512
kern.aio_max = 8192
kern.arandom = 1065677498
kern.argmax = 262144
kern.ccpu = 1948
kern.clockrate: tick = 10000, tickadj = 40, hz = 100, profhz = 100, stathz = 100
kern.configname = GENERIC64
kern.consdev = ttyE0
kern.dump_on_panic = 1
kern.expose_address = 1
kern.forkfsleep = 0
kern.fscale = 2048
kern.fsync = 1
kern.hardclock_ticks = 70391
kern.iov_max = 1024
kern.mbuf.mblowat = 16
kern.mbuf.mclbytes = 2048
kern.mbuf.mcllowat = 8
kern.mbuf.msize = 512
kern.mbuf.nmbclusters = 29884
kern.osrelease = 9.1
kern.osrevision = 901000000
kern.ostype = NetBSD
kern.timecounter.choice = clockinterrupt(q=0, f=100 Hz) armgtmr0(q=500, f=19200000 Hz) dummy(q=-1000000, f=1000000 Hz)
kern.timecounter.hardware = armgtmr0
kern.timecounter.timestepwarnings = 0
kern.version = NetBSD 9.1 (GENERIC64) #0: Sun Oct 18 19:24:30 UTC 2020
machdep.board_model = 0
machdep.board_revision = 10494163
machdep.cpu.frequency.available = 600 1400
machdep.cpu.frequency.current = 600
machdep.cpu.frequency.max = 1400
machdep.cpu.frequency.min = 600
machdep.cpu.frequency.target = 600
machdep.firmware_revision = 1536600398
machdep.serial = 0x66661338
```

### mmc

```shell
# dmesg | grep mmc
[     1.000003] mmcpwrseq0 at simplebus0autoconfiguration error: : couldn't get reset GPIOs
[     2.012282] sdmmc0 at bcmsdhost0
[     2.023714] sdmmc1 at sdhc0 slot 0
[     2.133154] sdmmc0: direct I/O error 5, r=6 p=0xffffffc032d60e4c write
[     2.153156] sdmmc0: SD card status: 4-bit, C10, U1, V10, A1
[     2.163158] ld0 at sdmmc0: <0x27:0x5048:SD64G:0x60:0xda7f8e63:0x14a>
[     2.253166] sdmmc1: 4-bit width, 50.000 MHz
[     2.253166] sdmmc1: MAX_BLK_SIZE1 = 64
[     2.253166] sdmmc1: MAX_BLK_SIZE2 = 512
[     2.264688] sdmmc1: MAX_BLK_SIZE3 = 512
[     2.264688] sdmmc1: SDIO function
[     2.273768] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 1 not configured
[     2.273768] (manufacturer 0x2d0, product 0xa9a6) at sdmmc1 function 2 not configured
[     2.294268] (manufacturer 0x2d0, product 0xa9a6, standard function interface code 0x2) at sdmmc1 function 3 not configured
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
cylinders: 1148
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
  UID  PID PPID   CPU PRI NI   VSZ   RSS WCHAN   STAT TTY      TIME COMMAND
    0    0    0     0   0  0     0 31148 -       OKl  ?     0:08.60 [system]
    0    1    0     0  85  0 12092  1584 wait    Is   ?     0:00.02 - init
    0  252    1     0  85  0 19228  1920 kqueue  Ss   ?     0:00.16 |-- dhcpcd: [master] [ip4] [ip6]
32767  255    1     0  85  0 15120  2136 select  Ss   ?     0:00.14 |-- /usr/sbin/mdnsd
    0  306    1     0  85  0 18944  2332 kqueue  Ss   ?     0:00.27 |-- /usr/sbin/syslogd -s
    0  462    1  3142  85  0 14084   828 devmon  Is   ?     0:00.00 |-- /sbin/devpubd
    0  520    1     0  85  0 24248  6080 select  Ss   ?     0:00.13 |-- /usr/sbin/sshd
    0  597    1     0  85  0 20320 23540 pause   Ss   ?     0:00.30 |-- /usr/sbin/ntpd -p /var/run/ntpd.pid -g
    0  785    1     0  85  0 28752  2784 kqueue  Ss   ?     0:00.05 |-- /usr/libexec/postfix/master -w
   12  873  785     0  85  0 23532  4656 kqueue  I    ?     0:00.07 | |-- qmgr -l -t unix -u
   12  880  785     0  85  0 25008  4656 kqueue  S    ?     0:00.06 | `-- pickup -l -t unix -u
    0  876    1 10611  83  0 17668  1364 kqueue  Is   ?     0:00.01 |-- /usr/sbin/inetd -l
    0 1037    1     0  85  0 17896  1632 nanoslp Is   ?     0:00.02 |-- /usr/sbin/cron
    0 1046    1  4912  84  0 23168  4728 wait    Is   ttyE0 0:00.13 `-- login
    0  771 1046     0  85  0 15812  2072 wait    S    ttyE0 0:00.14   `-- -sh
    0  203  771  1024  42  0 13840  1572 -       O+   ttyE0 0:00.02     `-- ps -axdl

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
        inet6 fe80::4dbc:22c8:4e8c:5d18%mue0/64 flags 0x0 scopeid 0x2
        inet6 2409:8a55:8bc:dda0:252:275b:2061:5757/64 flags 0x0
        inet 192.168.1.5/24 broadcast 192.168.1.255 flags 0x0
lo0: flags=0x8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 33624
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
Target: aarch64--netbsd
Configured with: /usr/src/tools/gcc/../../external/gpl3/gcc/dist/configure --target=aarch64--netbsd --enable-long-long --enable-threads --with-bugurl=http://www.NetBSD.org/Misc/send-pr.html --with-pkgversion='NetBSD nb4 20200810' --with-system-zlib --without-isl --enable-__cxa_atexit --enable-libstdcxx-time=rt --enable-libstdcxx-threads --with-diagnostics-color=auto-if-env --enable-fix-cortex-a53-835769 --enable-fix-cortex-a53-843419 --with-default-libstdcxx-abi=new --with-mpc-lib=/var/obj/mknative/evbarm-aarch64/usr/src/external/lgpl3/mpc/lib/libmpc --with-mpfr-lib=/var/obj/mknative/evbarm-aarch64/usr/src/external/lgpl3/mpfr/lib/libmpfr --with-gmp-lib=/var/obj/mknative/evbarm-aarch64/usr/src/external/lgpl3/gmp/lib/libgmp --with-mpc-include=/usr/src/external/lgpl3/mpc/dist/src --with-mpfr-include=/usr/src/external/lgpl3/mpfr/dist/src --with-gmp-include=/usr/src/external/lgpl3/gmp/lib/libgmp/arch/aarch64 --enable-tls --disable-multilib --disable-libstdcxx-pch --build=aarch64--netbsd --host=aarch64--netbsd --with-sysroot=/var/obj/mknative/evbarm-aarch64/usr/src/destdir.evbarm
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
export PKG_PATH="http://cdn.NetBSD.org/pub/pkgsrc/packages/$(uname -s)/$(uname -p)/$(uname -r | cut -f '1 2' -d.)/All/"
export PKG_PATH="http://cdn.netbsd.org/pub/pkgsrc/packages/NetBSD/aarch64/9.1/All/;http://cdn.netbsd.org/pub/pkgsrc/packages/NetBSD/aarch64/9.0/All/"

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

NetBSD 9.1 aarch64 can use my Realtek RTL8152 Based USB Ethernet Adapter, it's works very good.
But is can't found WiFi Adapter in Raspberry Pi 3 Model B Plus Rev 1.3, it's really disappointing.
