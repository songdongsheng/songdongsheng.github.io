---
title: Realtek RTL8761B based Bluetooth adapter
description: Realtek RTL8761B based Bluetooth adapter
date: 2020-11-15 22:11:51
tags:
    - Operating system
    - Linux
    - Windows
categories: [Operating system]
permalink: realtek-rtl8761b-based-bluetooth-adapter
---

# Realtek RTL8761B based Bluetooth adapter

## General Specification

| &nbsp;                  |                                                                          &nbsp; |
| :---------------------- | ------------------------------------------------------------------------------: |
| Chip Set                |                                                                Realtek RTL8761B |
| Product ID              |                                                                      FSC-BT836B |
| Dimension               |                                                             13mm x 26.9mm x 2mm |
| Bluetooth Specification |                                                      Bluetooth V5.0 (Dual Mode) |
| Power Supply            |                                                                     3.3 Volt DC |
| Output Power            |                                                              10 dBm (Class 1.5) |
| Sensitivity             |                                                                -94.5dBm@0.1%BER |
| Frequency Band          |                                                  2.402 GHz ~ 2.480 GHz ISM band |
| Modulation              |                                                            FHSS,GFSK,DPSK,DQPSK |
| Baseband                |                                                               Crystal OSC 40MHz |
| Hopping & channels      | 1600 hops/sec, 1 MHz channel space, 79 Channels (BT 5.0 to 2 MHz channel space) |
| RF Input Impedance      |                                                                         50 ohms |
| Antenna                 |                                                         Integrated chip antenna |
| Interface               |                           Data: UART (Standard), I2C; Others: PIO, AIO, PWM.USB |
| Profile                 |                                                                  SPP, HID, GATT |
| Advanced Feature        |                                                                        MFi, OTA |
| Temperature             |                                                                 -20ºC to +70 ºC |
| Humidity                |                                                        10% ~ 95% Non-Condensing |
| Environmental           |                                                                  RoHS Compliant |

## Linux driver & firmware

### Linux-bluetooth sub-system

-   [Bluetooth: btrtl: Add support for RTL8761B](https://www.spinics.net/lists/linux-bluetooth/msg84458.html)
-   Fri, 10 Apr 2020 22:54:20 +0800

### Realtek bluetooth firmware

```shell
# cat PKGBUILD
# Maintainer: Jack Chen <redchenjs@live.com>

pkgname=rtl8761b-fw
pkgver=20200610
pkgrel=1
pkgdesc="Realtek bluetooth firmware for RTL8761B based devices"
arch=('any')
url="https://www.xmpow.com/pages/download"
license=('unknown')
source=(
    "https://mpow.s3-us-west-1.amazonaws.com/mpow_MPBH456AB_driver+for+Linux.tgz"
)
sha512sums=(
    'ebdf480a2e7746853b2ad1f422befcc86ec39c676147350b5440000e6e3c1e4cad3c4154c8fbdc72de86103269c336fc196b1173703767640a15d55d297928a2'
)

package() {
    cd "$srcdir/20200610_LINUX_BT_DRIVER/rtkbt-firmware/lib/firmware/rtlbt"

    install -Dm644 rtl8761b_fw "$pkgdir/usr/lib/firmware/rtl_bt/rtl8761b_fw.bin"
    install -Dm644 rtl8761b_config "$pkgdir/usr/lib/firmware/rtl_bt/rtl8761b_config.bin"
}
```

```shell
# find /lib/firmware | grep rtl8761b | xargs ls -ln
-rw-r--r-- 1 0 0    25 Oct 20 23:00 /lib/firmware/rtl_bt/rtl8761b_config.bin
-rw-r--r-- 1 0 0 35644 Oct 20 23:00 /lib/firmware/rtl_bt/rtl8761b_fw.bin
```

```shell
# find /lib/firmware | grep rtl8761b | xargs sha256sum
6ddeb15f23588053e00cb08d25588bd7cf98d60fa93d9478efcef4ae8064a7ac  /lib/firmware/rtl_bt/rtl8761b_config.bin
65e638f295e892ba0341ea825d48b6d392c28169098565affb77f06ba1597dce  /lib/firmware/rtl_bt/rtl8761b_fw.bin
```

## dmesg

```
# dmesg -w

[ 3466.994606] usb 2-1: new full-speed USB device number 6 using xhci_hcd
[ 3467.143775] usb 2-1: New USB device found, idVendor=0bda, idProduct=8771, bcdDevice= 2.00
[ 3467.143780] usb 2-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[ 3467.143783] usb 2-1: Product: Bluetooth Radio
[ 3467.143785] usb 2-1: Manufacturer: Realtek
[ 3467.143787] usb 2-1: SerialNumber: 00E04C239987
[ 3467.146180] Bluetooth: hci0: RTL: examining hci_ver=0a hci_rev=000b lmp_ver=0a lmp_subver=8761
[ 3467.147148] Bluetooth: hci0: RTL: rom_version status=0 version=1
[ 3467.147152] Bluetooth: hci0: RTL: loading rtl_bt/rtl8761b_fw.bin
[ 3467.147246] Bluetooth: hci0: RTL: loading rtl_bt/rtl8761b_config.bin
[ 3467.147307] Bluetooth: hci0: RTL: cfg_sz 25, total sz 21389
[ 3467.260167] Bluetooth: hci0: RTL: fw version 0x0d99646b
[ 3531.453265] Bluetooth: hci0: Ignoring error of Inquiry Cancel command
[ 3545.075429] input: Redmi K30 Pro (AVRCP) as /devices/virtual/input/input19
```

## bluetoothctl

```shell
# bluetoothctl version
Version 5.53

# bluetoothctl list
Controller 8C:88:2B:30:4B:09 E7440-01 [default]

# bluetoothctl show
Controller 8C:88:2B:30:4B:09 (public)
	Name: E7440-01
	Alias: E7440-01
	Class: 0x001c010c
	Powered: yes
	Discoverable: yes
	DiscoverableTimeout: 0x00000000
	Pairable: yes
	UUID: Message Notification Se.. (00001133-0000-1000-8000-00805f9b34fb)
	UUID: A/V Remote Control        (0000110e-0000-1000-8000-00805f9b34fb)
	UUID: OBEX Object Push          (00001105-0000-1000-8000-00805f9b34fb)
	UUID: Message Access Server     (00001132-0000-1000-8000-00805f9b34fb)
	UUID: IrMC Sync                 (00001104-0000-1000-8000-00805f9b34fb)
	UUID: PnP Information           (00001200-0000-1000-8000-00805f9b34fb)
	UUID: Vendor specific           (00005005-0000-1000-8000-0002ee000001)
	UUID: Headset AG                (00001112-0000-1000-8000-00805f9b34fb)
	UUID: Headset                   (00001108-0000-1000-8000-00805f9b34fb)
	UUID: A/V Remote Control Target (0000110c-0000-1000-8000-00805f9b34fb)
	UUID: Generic Attribute Profile (00001801-0000-1000-8000-00805f9b34fb)
	UUID: Phonebook Access Server   (0000112f-0000-1000-8000-00805f9b34fb)
	UUID: Audio Sink                (0000110b-0000-1000-8000-00805f9b34fb)
	UUID: Generic Access Profile    (00001800-0000-1000-8000-00805f9b34fb)
	UUID: Audio Source              (0000110a-0000-1000-8000-00805f9b34fb)
	UUID: OBEX File Transfer        (00001106-0000-1000-8000-00805f9b34fb)
	Modalias: usb:v1D6Bp0246d0535
	Discovering: yes
Advertising Features:
	ActiveInstances: 0x00
	SupportedInstances: 0x05
	SupportedIncludes: tx-power
	SupportedIncludes: appearance
	SupportedIncludes: local-name
	SupportedSecondaryChannels: 1M
	SupportedSecondaryChannels: 2M
	SupportedSecondaryChannels: Coded

# bluetoothctl devices
Device 6C:7F:E4:9C:57:4E 6C-7F-E4-9C-57-4E
Device E0:B6:55:60:54:2D MI BT18 BLE
Device AC:C1:EE:0C:77:AB 小米手机5
Device 9C:28:F7:97:DF:48 Redmi K30 Pro

# bluetoothctl info 9C:28:F7:97:DF:48
Device 9C:28:F7:97:DF:48 (public)
	Name: Redmi K30 Pro
	Alias: Redmi K30 Pro
	Class: 0x005a020c
	Icon: phone
	Paired: yes
	Trusted: yes
	Blocked: no
	Connected: yes
	LegacyPairing: no
	UUID: OBEX Object Push          (00001105-0000-1000-8000-00805f9b34fb)
	UUID: Headset                   (00001108-0000-1000-8000-00805f9b34fb)
	UUID: Audio Source              (0000110a-0000-1000-8000-00805f9b34fb)
	UUID: A/V Remote Control Target (0000110c-0000-1000-8000-00805f9b34fb)
	UUID: Headset AG                (00001112-0000-1000-8000-00805f9b34fb)
	UUID: PANU                      (00001115-0000-1000-8000-00805f9b34fb)
	UUID: NAP                       (00001116-0000-1000-8000-00805f9b34fb)
	UUID: Handsfree Audio Gateway   (0000111f-0000-1000-8000-00805f9b34fb)
	UUID: Phonebook Access Server   (0000112f-0000-1000-8000-00805f9b34fb)
	UUID: Message Access Server     (00001132-0000-1000-8000-00805f9b34fb)
	UUID: PnP Information           (00001200-0000-1000-8000-00805f9b34fb)
	UUID: Generic Access Profile    (00001800-0000-1000-8000-00805f9b34fb)
	UUID: Generic Attribute Profile (00001801-0000-1000-8000-00805f9b34fb)
	UUID: Unknown                   (00009955-0000-1000-8000-00805f9b34fb)
	Modalias: bluetooth:v038Fp1200d1436
	RSSI: -76
```

## hciconfig

```shell
# hciconfig -a
hci0:	Type: Primary  Bus: USB
	BD Address: 8C:88:2B:30:4B:09  ACL MTU: 1021:5  SCO MTU: 255:11
	UP RUNNING PSCAN ISCAN INQUIRY
	RX bytes:45254864 acl:73336 sco:0 events:1907 errors:0
	TX bytes:32585 acl:140 sco:0 commands:691 errors:0
	Features: 0xff 0xff 0xff 0xfe 0xdb 0xfd 0x7b 0x87
	Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3
	Link policy: RSWITCH HOLD SNIFF PARK
	Link mode: SLAVE ACCEPT
	Name: 'E7440-01'
	Class: 0x1c010c
	Service Classes: Rendering, Capturing, Object Transfer
	Device Class: Computer, Laptop
	HCI Version: 5.1 (0xa)  Revision: 0xd99
	LMP Version: 5.1 (0xa)  Subversion: 0x646b
	Manufacturer: Realtek Semiconductor Corporation (93)
```

```shell
# hciconfig hci0 version
hci0:	Type: Primary  Bus: USB
	BD Address: 8C:88:2B:30:4B:09  ACL MTU: 1021:5  SCO MTU: 255:11
	HCI Version: 5.1 (0xa)  Revision: 0xd99
	LMP Version: 5.1 (0xa)  Subversion: 0x646b
	Manufacturer: Realtek Semiconductor Corporation (93)
```

```shell
# hciconfig hci0 features
hci0:	Type: Primary  Bus: USB
	BD Address: 8C:88:2B:30:4B:09  ACL MTU: 1021:5  SCO MTU: 255:11
	Features page 0: 0xff 0xff 0xff 0xfe 0xdb 0xfd 0x7b 0x87
		<3-slot packets> <5-slot packets> <encryption> <slot offset>
		<timing accuracy> <role switch> <hold mode> <sniff mode>
		<park state> <RSSI> <channel quality> <SCO link> <HV2 packets>
		<HV3 packets> <u-law log> <A-law log> <CVSD> <paging scheme>
		<power control> <transparent SCO> <broadcast encrypt>
		<EDR ACL 2 Mbps> <EDR ACL 3 Mbps> <enhanced iscan>
		<interlaced iscan> <interlaced pscan> <inquiry with RSSI>
		<extended SCO> <EV4 packets> <EV5 packets> <AFH cap. slave>
		<AFH class. slave> <LE support> <3-slot EDR ACL>
		<5-slot EDR ACL> <pause encryption> <AFH cap. master>
		<AFH class. master> <EDR eSCO 2 Mbps> <EDR eSCO 3 Mbps>
		<3-slot EDR eSCO> <extended inquiry> <LE and BR/EDR>
		<simple pairing> <encapsulated PDU> <err. data report>
		<non-flush flag> <LSTO> <inquiry TX power> <EPC>
		<extended features>
	Features page 1: 0x03 0x00 0x00 0x00 0x00 0x00 0x00 0x00
	Features page 2: 0x5f 0x02 0x00 0x00 0x00 0x00 0x00 0x00
```

## hwinfo

```shell
# hwinfo --bluetooth
02: USB 00.0: 11500 Bluetooth Device
  [Created at usb.122]
  Unique ID: FKGF.huCbZCwfOy3
  Parent ID: pBe4.2DFUsyrieMD
  SysFS ID: /devices/pci0000:00/0000:00:14.0/usb2/2-1/2-1:1.0
  SysFS BusID: 2-1:1.0
  Hardware Class: bluetooth
  Model: "Realtek Bluetooth Radio"
  Hotplug: USB
  Vendor: usb 0x0bda "Realtek Semiconductor Corp."
  Device: usb 0x8771 "Bluetooth Radio"
  Revision: "2.00"
  Serial ID: "00E04C239987"
  Driver: "btusb"
  Driver Modules: "btusb"
  Speed: 12 Mbps
  Module Alias: "usb:v0BDAp8771d0200dcE0dsc01dp01icE0isc01ip01in00"
  Driver Info #0:
    Driver Status: btusb is active
    Driver Activation Cmd: "modprobe btusb"
  Config Status: cfg=new, avail=yes, need=no, active=unknown
  Attached to: #8 (Hub)
```

## lsusb

```shell
# lsusb
Bus 001 Device 002: ID 8087:8000 Intel Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 002 Device 002: ID 0c45:64d2 Microdia Integrated Webcam
Bus 002 Device 006: ID 0bda:8771 Realtek Semiconductor Corp. Bluetooth Radio
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

## usb-devices

```shell
# usb-devices | awk '/0bda/' RS=
T:  Bus=02 Lev=01 Prnt=01 Port=00 Cnt=01 Dev#=  6 Spd=12  MxCh= 0
D:  Ver= 1.10 Cls=e0(wlcon) Sub=01 Prot=01 MxPS=64 #Cfgs=  1
P:  Vendor=0bda ProdID=8771 Rev=02.00
S:  Manufacturer=Realtek
S:  Product=Bluetooth Radio
S:  SerialNumber=00E04C239987
C:  #Ifs= 2 Cfg#= 1 Atr=e0 MxPwr=500mA
I:  If#=0x0 Alt= 0 #EPs= 3 Cls=e0(wlcon) Sub=01 Prot=01 Driver=btusb
I:  If#=0x1 Alt= 0 #EPs= 2 Cls=e0(wlcon) Sub=01 Prot=01 Driver=btusb
```

## lsusb verbose

```shell
# lsusb -v -d 0bda:8771

Bus 002 Device 006: ID 0bda:8771 Realtek Semiconductor Corp. Bluetooth Radio
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               1.10
  bDeviceClass          224 Wireless
  bDeviceSubClass         1 Radio Frequency
  bDeviceProtocol         1 Bluetooth
  bMaxPacketSize0        64
  idVendor           0x0bda Realtek Semiconductor Corp.
  idProduct          0x8771
  bcdDevice            2.00
  iManufacturer           1 Realtek
  iProduct                2 Bluetooth Radio
  iSerial                 3 00E04C239987
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength       0x00b1
    bNumInterfaces          2
    bConfigurationValue     1
    iConfiguration          0
    bmAttributes         0xe0
      Self Powered
      Remote Wakeup
    MaxPower              500mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           3
      bInterfaceClass       224 Wireless
      bInterfaceSubClass      1 Radio Frequency
      bInterfaceProtocol      1 Bluetooth
      iInterface              4 Bluetooth Radio
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x81  EP 1 IN
        bmAttributes            3
          Transfer Type            Interrupt
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0010  1x 16 bytes
        bInterval               1
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x02  EP 2 OUT
        bmAttributes            2
          Transfer Type            Bulk
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0040  1x 64 bytes
        bInterval               0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x82  EP 2 IN
        bmAttributes            2
          Transfer Type            Bulk
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0040  1x 64 bytes
        bInterval               0
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       0
      bNumEndpoints           2
      bInterfaceClass       224 Wireless
      bInterfaceSubClass      1 Radio Frequency
      bInterfaceProtocol      1 Bluetooth
      iInterface              4 Bluetooth Radio
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x03  EP 3 OUT
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0000  1x 0 bytes
        bInterval               1
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x83  EP 3 IN
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0000  1x 0 bytes
        bInterval               1
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       1
      bNumEndpoints           2
      bInterfaceClass       224 Wireless
      bInterfaceSubClass      1 Radio Frequency
      bInterfaceProtocol      1 Bluetooth
      iInterface              4 Bluetooth Radio
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x03  EP 3 OUT
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0009  1x 9 bytes
        bInterval               1
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x83  EP 3 IN
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0009  1x 9 bytes
        bInterval               1
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       2
      bNumEndpoints           2
      bInterfaceClass       224 Wireless
      bInterfaceSubClass      1 Radio Frequency
      bInterfaceProtocol      1 Bluetooth
      iInterface              4 Bluetooth Radio
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x03  EP 3 OUT
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0011  1x 17 bytes
        bInterval               1
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x83  EP 3 IN
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0011  1x 17 bytes
        bInterval               1
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       3
      bNumEndpoints           2
      bInterfaceClass       224 Wireless
      bInterfaceSubClass      1 Radio Frequency
      bInterfaceProtocol      1 Bluetooth
      iInterface              4 Bluetooth Radio
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x03  EP 3 OUT
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0019  1x 25 bytes
        bInterval               1
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x83  EP 3 IN
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0019  1x 25 bytes
        bInterval               1
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       4
      bNumEndpoints           2
      bInterfaceClass       224 Wireless
      bInterfaceSubClass      1 Radio Frequency
      bInterfaceProtocol      1 Bluetooth
      iInterface              4 Bluetooth Radio
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x03  EP 3 OUT
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0021  1x 33 bytes
        bInterval               1
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x83  EP 3 IN
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0021  1x 33 bytes
        bInterval               1
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       5
      bNumEndpoints           2
      bInterfaceClass       224 Wireless
      bInterfaceSubClass      1 Radio Frequency
      bInterfaceProtocol      1 Bluetooth
      iInterface              4 Bluetooth Radio
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x03  EP 3 OUT
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0031  1x 49 bytes
        bInterval               1
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x83  EP 3 IN
        bmAttributes            1
          Transfer Type            Isochronous
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0031  1x 49 bytes
        bInterval               1
can't get debug descriptor: Resource temporarily unavailable
Device Status:     0x0001
  Self Powered
```
