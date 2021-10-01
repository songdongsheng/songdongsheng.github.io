---
title: USB Bluetooth Adaper
excerpt: USB Bluetooth Adaper
date: 2020-02-25 23:27:51
tags:
  - Trends
categories: [Trends]
---

# USB Bluetooth Adaper

## Introductions

- https://en.wikipedia.org/wiki/Bluetooth
- https://en.wikipedia.org/wiki/Bluetooth\_Low\_Energy
- https://www.broadcom.com/products/wireless/wireless-lan-bluetooth
- https://www.qualcomm.com/products/csr8510
- http://www.barrot.com.cn/a/BR200Xxilie/12.html
- https://www.lulian.cn/product/list-160-cn.html
- https://bugzilla.kernel.org/show\_bug.cgi?id=60824
- https://blog.csdn.net/xubin341719/article/details/38145507
- https://blog.csdn.net/xubin341719/article/details/38228705
- https://blog.csdn.net/xubin341719/article/details/38303881
- https://blog.csdn.net/xubin341719/article/details/38305331
- https://blog.csdn.net/xubin341719/article/details/38335533

国内市场上的 USB 蓝牙适配器，大多数基于 CSR8510 芯片，少数采用 BCM20702 芯片，都是蓝牙 4.0 产品。基于 BR8041 芯片的低端产品从 2019 下半年开始陆续出现。

## BR8041

百瑞互联的 BR8041 于 2019 年 1 月通过了 SIG Bluetooth 5.0 的认证。BR8041 声称兼容 BCM20702 和 CSR8510，并且因为没有官方驱动，所以通常被识别为 CSR8510。它与 Linux 的兼容性很差，个人尝试不成功，也没有搜索到成功案例。在 Windows 10 下，使用系统内置驱动工作不够稳定，会频繁断开，或者报告驱动程序错误。

代表产品是胜为的 UDC-328B，19.9 包邮。

```bash
# btmon

< HCI Command: Delete Stored Link Key (0x03|0x0012) plen 7                 #59 [hci0] 47.447047
        Address: 00:00:00:00:00:00 (OUI 00-00-00)
        Delete all: 0x01
> HCI Event: Command Complete (0x0e) plen 6                                #60 [hci0] 47.447946
      Delete Stored Link Key (0x03|0x0012) ncmd 1
        Status: Unsupported Feature or Parameter Value (0x11)
        Num keys: 0
= Close Index: 00:1A:7D:DA:71:11                                               [hci0] 47.448017
```

## CSR8510 - UGREEN CM109

### Specifications

- Bluetooth adapter 30722
- CMIIT ID: 2017DP1793
- 产品型号：CM109
- 产品尺寸：L27.5xW16xH7.5(mm)
- 芯片型号：CSR8510
- 蓝牙版本：4.0
- 传输频率：2.4GHz
- 传输速率：3Mbps
- 无障碍传输距离：20m
- USB 接口：2.0 A 公接口
- 协议：HFP 1.5，HSP 1.2，AVRCP 1.4，A2DP 1.2，FTB 1.1
- 支持 Windows XP，Vista 和 7/8/10

### Linux

#### dmesg

```bash
$ dmesg | grep -i Bluetooth
[  223.346181] usb 2-8: new full-speed USB device number 5 using xhci_hcd
[  223.588222] usb 2-8: New USB device found, idVendor=0a12, idProduct=0001, bcdDevice=88.91
[  223.588227] usb 2-8: New USB device strings: Mfr=0, Product=2, SerialNumber=0
[  223.588229] usb 2-8: Product: CSR8510 A10
[  223.728329] alg: No test for fips(ansi_cprng) (fips_ansi_cprng)
[  223.783748] Bluetooth: Core ver 2.22
[  223.783765] NET: Registered protocol family 31
[  223.783765] Bluetooth: HCI device and connection manager initialized
[  223.783768] Bluetooth: HCI socket layer initialized
[  223.783769] Bluetooth: L2CAP socket layer initialized
[  223.783773] Bluetooth: SCO socket layer initialized
[  223.815292] usbcore: registered new interface driver btusb
[  224.067336] Bluetooth: BNEP (Ethernet Emulation) ver 1.3
[  224.067337] Bluetooth: BNEP filters: protocol multicast
[  224.067340] Bluetooth: BNEP socket layer initialized
[  224.113811] Bluetooth: RFCOMM TTY layer initialized
[  224.113818] Bluetooth: RFCOMM socket layer initialized
[  224.113825] Bluetooth: RFCOMM ver 1.11

[  265.491088] usb 2-8: USB disconnect, device number 5

[  268.881973] usb 2-8: new full-speed USB device number 6 using xhci_hcd
[  269.131874] usb 2-8: New USB device found, idVendor=0a12, idProduct=0001, bcdDevice=88.91
[  269.131880] usb 2-8: New USB device strings: Mfr=0, Product=2, SerialNumber=0
[  269.131883] usb 2-8: Product: CSR8510 A10
```

#### lsusb

```bash
# lsusb
Bus 004 Device 002: ID 8087:8000 Intel Corp.
Bus 004 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 8087:8008 Intel Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 002 Device 006: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)
Bus 002 Device 002: ID 04ca:0061 Lite-On Technology Corp.
Bus 002 Device 004: ID 1c4f:0002 SiGma Micro Keyboard TRACER Gamma Ivory
Bus 002 Device 003: ID 148f:5572 Ralink Technology, Corp. RT5572 Wireless Adapter
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

```bash
# usb-devices | awk '/0a12/' RS=
T:  Bus=02 Lev=01 Prnt=01 Port=07 Cnt=04 Dev#=  6 Spd=12  MxCh= 0
D:  Ver= 2.00 Cls=e0(wlcon) Sub=01 Prot=01 MxPS=64 #Cfgs=  1
P:  Vendor=0a12 ProdID=0001 Rev=88.91
S:  Product=CSR8510 A10
C:  #Ifs= 2 Cfg#= 1 Atr=e0 MxPwr=100mA
I:  If#=0x0 Alt= 0 #EPs= 3 Cls=e0(wlcon) Sub=01 Prot=01 Driver=btusb
I:  If#=0x1 Alt= 0 #EPs= 2 Cls=e0(wlcon) Sub=01 Prot=01 Driver=btusb
```

```bash
lsusb -v -d 0a12:0001
```

#### hwinfo

```bash
# hwinfo --bluetooth
01: USB 00.0: 11500 Bluetooth Device
  [Created at usb.122]
  Unique ID: 1Nxo.nQKjiuCfL84
  Parent ID: pBe4.2DFUsyrieMD
  SysFS ID: /devices/pci0000:00/0000:00:14.0/usb2/2-8/2-8:1.0
  SysFS BusID: 2-8:1.0
  Hardware Class: bluetooth
  Model: "Cambridge Silicon Radio Bluetooth Dongle (HCI mode)"
  Hotplug: USB
  Vendor: usb 0x0a12 "Cambridge Silicon Radio, Ltd"
  Device: usb 0x0001 "Bluetooth Dongle (HCI mode)"
  Revision: "88.91"
  Driver: "btusb"
  Driver Modules: "btusb"
  Speed: 12 Mbps
  Module Alias: "usb:v0A12p0001d8891dcE0dsc01dp01icE0isc01ip01in00"
  Driver Info #0:
    Driver Status: btusb is active
    Driver Activation Cmd: "modprobe btusb"
  Config Status: cfg=new, avail=yes, need=no, active=unknown
  Attached to: #12 (Hub)
```

#### hciconfig

```bash
# hciconfig -a
hci0:   Type: Primary  Bus: USB
    BD Address: 00:1A:7D:DA:71:11  ACL MTU: 310:10  SCO MTU: 64:8
    UP RUNNING
    RX bytes:682 acl:0 sco:0 events:48 errors:0
    TX bytes:3163 acl:0 sco:0 commands:48 errors:0
    Features: 0xff 0xff 0x8f 0xfe 0xdb 0xff 0x5b 0x87
    Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3
    Link policy: RSWITCH HOLD SNIFF PARK
    Link mode: SLAVE ACCEPT
    Name: 'OP-7020-01'
    Class: 0x0c0104
    Service Classes: Rendering, Capturing
    Device Class: Computer, Desktop workstation
    HCI Version: 4.0 (0x6)  Revision: 0x22bb
    LMP Version: 4.0 (0x6)  Subversion: 0x22bb
    Manufacturer: Cambridge Silicon Radio (10)
```

```bash
# hciconfig hci0 version
hci0:   Type: Primary  Bus: USB
    BD Address: 00:1A:7D:DA:71:11  ACL MTU: 310:10  SCO MTU: 64:8
    HCI Version: 4.0 (0x6)  Revision: 0x22bb
    LMP Version: 4.0 (0x6)  Subversion: 0x22bb
    Manufacturer: Cambridge Silicon Radio (10)
```

```bash
# hciconfig hci0 features
hci0:   Type: Primary  Bus: USB
    BD Address: 00:1A:7D:DA:71:11  ACL MTU: 310:10  SCO MTU: 64:8
    Features page 0: 0xff 0xff 0x8f 0xfe 0xdb 0xff 0x5b 0x87
        <3-slot packets> <5-slot packets> <encryption> <slot offset>
        <timing accuracy> <role switch> <hold mode> <sniff mode>
        <park state> <RSSI> <channel quality> <SCO link> <HV2 packets>
        <HV3 packets> <u-law log> <A-law log> <CVSD> <paging scheme>
        <power control> <transparent SCO> <broadcast encrypt>
        <EDR ACL 2 Mbps> <EDR ACL 3 Mbps> <enhanced iscan>
        <interlaced iscan> <interlaced pscan> <inquiry with RSSI>
        <extended SCO> <EV4 packets> <EV5 packets> <AFH cap. slave>
        <AFH class. slave> <LE support> <3-slot EDR ACL>
        <5-slot EDR ACL> <sniff subrating> <pause encryption>
        <AFH cap. master> <AFH class. master> <EDR eSCO 2 Mbps>
        <EDR eSCO 3 Mbps> <3-slot EDR eSCO> <extended inquiry>
        <LE and BR/EDR> <simple pairing> <encapsulated PDU>
        <non-flush flag> <LSTO> <inquiry TX power> <EPC>
        <extended features>
    Features page 1: 0x03 0x00 0x00 0x00 0x00 0x00 0x00 0x00
```

```bash
hciconfig hci down
hciconfig hci up
hcitool dev
```

#### bluetoothctl

```bash
$ bluetoothctl version
Version 5.50

$ bluetoothctl list
Controller 00:1A:7D:DA:71:11 OP-7020-01 [default]

$ bluetoothctl devices
Device FC:58:FA:FB:68:35 T5S
Device AC:C1:EE:0C:77:AB 小米手机5
Device E0:B6:55:60:54:2D MI BT18 BLE

$ bluetoothctl info FC:58:FA:FB:68:35
Device FC:58:FA:FB:68:35 (public)
    Name: T5S
    Alias: T5S
    Class: 0x00260404
    Icon: audio-card
    Paired: yes
    Trusted: yes
    Blocked: no
    Connected: yes
    LegacyPairing: no
    UUID: Headset                   (00001108-0000-1000-8000-00805f9b34fb)
    UUID: Audio Sink                (0000110b-0000-1000-8000-00805f9b34fb)
    UUID: A/V Remote Control Target (0000110c-0000-1000-8000-00805f9b34fb)
    UUID: A/V Remote Control        (0000110e-0000-1000-8000-00805f9b34fb)
    UUID: Handsfree                 (0000111e-0000-1000-8000-00805f9b34fb)
    RSSI: -47

$ bluetoothctl info
Device FC:58:FA:FB:68:35 (public)
    Name: T5S
    Alias: T5S
    Class: 0x00260404
    Icon: audio-card
    Paired: yes
    Trusted: yes
    Blocked: no
    Connected: yes
    LegacyPairing: no
    UUID: Headset                   (00001108-0000-1000-8000-00805f9b34fb)
    UUID: Audio Sink                (0000110b-0000-1000-8000-00805f9b34fb)
    UUID: A/V Remote Control Target (0000110c-0000-1000-8000-00805f9b34fb)
    UUID: A/V Remote Control        (0000110e-0000-1000-8000-00805f9b34fb)
    UUID: Handsfree                 (0000111e-0000-1000-8000-00805f9b34fb)
    RSSI: -47

$ bluetoothctl show
Controller 00:1A:7D:DA:71:11 (public)
    Name: OP-7020-01
    Alias: OP-7020-01
    Class: 0x001c0104
    Powered: yes
    Discoverable: no
    Pairable: yes
    UUID: Headset AG                (00001112-0000-1000-8000-00805f9b34fb)
    UUID: Generic Attribute Profile (00001801-0000-1000-8000-00805f9b34fb)
    UUID: A/V Remote Control        (0000110e-0000-1000-8000-00805f9b34fb)
    UUID: OBEX File Transfer        (00001106-0000-1000-8000-00805f9b34fb)
    UUID: Generic Access Profile    (00001800-0000-1000-8000-00805f9b34fb)
    UUID: OBEX Object Push          (00001105-0000-1000-8000-00805f9b34fb)
    UUID: PnP Information           (00001200-0000-1000-8000-00805f9b34fb)
    UUID: A/V Remote Control Target (0000110c-0000-1000-8000-00805f9b34fb)
    UUID: IrMC Sync                 (00001104-0000-1000-8000-00805f9b34fb)
    UUID: Audio Source              (0000110a-0000-1000-8000-00805f9b34fb)
    UUID: Audio Sink                (0000110b-0000-1000-8000-00805f9b34fb)
    UUID: Vendor specific           (00005005-0000-1000-8000-0002ee000001)
    UUID: Message Notification Se.. (00001133-0000-1000-8000-00805f9b34fb)
    UUID: Phonebook Access Server   (0000112f-0000-1000-8000-00805f9b34fb)
    UUID: Message Access Server     (00001132-0000-1000-8000-00805f9b34fb)
    UUID: Headset                   (00001108-0000-1000-8000-00805f9b34fb)
    Modalias: usb:v1D6Bp0246d0532
    Discovering: yes
```

#### btmon

```bash
# btmon
< HCI Command: Delete Stored Link Key (0x03|0x0012) plen 7                                                      #59 [hci0] 5.497484
        Address: 00:00:00:00:00:00 (OUI 00-00-00)
        Delete all: 0x01
> HCI Event: Command Complete (0x0e) plen 6                                                                     #60 [hci0] 5.498427
      Delete Stored Link Key (0x03|0x0012) ncmd 1
        Status: Unsupported Feature or Parameter Value (0x11)
        Num keys: 0
= Close Index: 00:1A:7D:DA:71:11
```

#### hcidump

```bash
(hciconfig hci0 down;hciconfig hci0 up);(hcidump &);hciconfig hci0 down
```

## BCM20702 - UDC-324

### Specifications

- 蓝牙版本：4.0
- 无障碍传输距离：20m
- 24 bit CRC 抗干扰
- 自动跳频减少串扰
- 芯片组：博通 20702A3
- 供应商 ID：0x0A5C
- 产品 ID：0x21EC
