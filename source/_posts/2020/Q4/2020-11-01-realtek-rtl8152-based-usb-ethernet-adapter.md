---
title: Realtek RTL8152 Based USB Ethernet Adapter
excerpt: Realtek RTL8152 Based USB Ethernet Adapter
date: 2020-11-01 12:16:54
tags:
    - Operating system
    - Linux
    - Windows
categories: [Operating system]
---

# Realtek RTL8152 Based USB Ethernet Adapter

## dmesg

```
# dmesg -w

[ 8640.849899] usb 2-2: new high-speed USB device number 4 using xhci_hcd
[ 8640.998148] usb 2-2: New USB device found, idVendor=214b, idProduct=7250, bcdDevice= 1.00
[ 8640.998152] usb 2-2: New USB device strings: Mfr=0, Product=1, SerialNumber=0
[ 8640.998154] usb 2-2: Product: USB2.0 HUB
[ 8640.999460] hub 2-2:1.0: USB hub found
[ 8640.999950] hub 2-2:1.0: 4 ports detected
[ 8641.285910] usb 2-2.1: new high-speed USB device number 5 using xhci_hcd
[ 8641.386819] usb 2-2.1: New USB device found, idVendor=0bda, idProduct=8152, bcdDevice=20.00
[ 8641.386822] usb 2-2.1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[ 8641.386823] usb 2-2.1: Product: USB 10/100 LAN
[ 8641.386825] usb 2-2.1: Manufacturer: Realtek
[ 8641.386826] usb 2-2.1: SerialNumber: 00E04C3608EC
[ 8641.412053] usbcore: registered new interface driver r8152
[ 8641.417333] usbcore: registered new interface driver cdc_ether
[ 8641.502276] usb 2-2.1: reset high-speed USB device number 5 using xhci_hcd
[ 8641.638858] r8152 2-2.1:1.0: skip request firmware
[ 8641.666572] r8152 2-2.1:1.0 eth0: v1.11.11
[ 8641.707491] r8152 2-2.1:1.0 enx00e04c3608ec: renamed from eth0
```

## ifconfig

```
# ifconfig enx00e04c3608ec
enx00e04c3608ec: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 00:e0:4c:36:08:ec  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

## ethtool

```
# ethtool enx00e04c3608ec
Settings for enx00e04c3608ec:
	Supported ports: [ TP MII ]
	Supported link modes:   10baseT/Half 10baseT/Full
	                        100baseT/Half 100baseT/Full
	Supported pause frame use: No
	Supports auto-negotiation: Yes
	Supported FEC modes: Not reported
	Advertised link modes:  10baseT/Half 10baseT/Full
	                        100baseT/Half 100baseT/Full
	Advertised pause frame use: Symmetric Receive-only
	Advertised auto-negotiation: Yes
	Advertised FEC modes: Not reported
	Speed: 10Mb/s
	Duplex: Half
	Port: MII
	PHYAD: 32
	Transceiver: internal
	Auto-negotiation: on
	Supports Wake-on: pumbg
	Wake-on: g
	Current message level: 0x00007fff (32767)
			       drv probe link timer ifdown ifup rx_err tx_err tx_queued intr tx_done rx_status pktdata hw wol
	Link detected: no
```

```
# ethtool -i enx00e04c3608ec
driver: r8152
version: v1.11.11
firmware-version:
expansion-rom-version:
bus-info: usb-0000:00:14.0-2.1
supports-statistics: yes
supports-test: no
supports-eeprom-access: no
supports-register-dump: no
supports-priv-flags: no
```

```
# ethtool -T enx00e04c3608ec
Time stamping parameters for enx00e04c3608ec:
Capabilities:
	software-receive      (SOF_TIMESTAMPING_RX_SOFTWARE)
	software-system-clock (SOF_TIMESTAMPING_SOFTWARE)
PTP Hardware Clock: none
Hardware Transmit Timestamp Modes: none
Hardware Receive Filter Modes: none
```

## hwinfo

```
  net interface: name = enx00e04c3608ec, path = /class/net/enx00e04c3608ec
    type = 1
    carrier = 0
    hw_addr = 00:e0:4c:36:08:ec
    net device: path = /devices/pci0000:00/0000:00:14.0/usb2/2-2/2-2.1/2-2.1:1.0
    net driver: name = r8152, path = /bus/usb/drivers/r8152

37: USB 00.0: 0200 Ethernet controller
  [Created at usb.122]
  Unique ID: WFGf.TaH2R0dMoZ8
  Parent ID: hSuP.V+vVO9032M2
  SysFS ID: /devices/pci0000:00/0000:00:14.0/usb2/2-2/2-2.1/2-2.1:1.0
  SysFS BusID: 2-2.1:1.0
  Hardware Class: network
  Model: "Realtek RTL8152 Fast Ethernet Adapter"
  Hotplug: USB
  Vendor: usb 0x0bda "Realtek Semiconductor Corp."
  Device: usb 0x8152 "RTL8152 Fast Ethernet Adapter"
  Revision: "20.00"
  Serial ID: "00E04C3608EC"
  Driver: "r8152"
  Driver Modules: "r8152"
  Device File: enx00e04c3608ec
  Speed: 480 Mbps
  HW Address: 00:e0:4c:36:08:ec
  Permanent HW Address: 00:e0:4c:36:08:ec
  Link detected: no
  Module Alias: "usb:v0BDAp8152d2000dc00dsc00dp00icFFiscFFip00in00"
  Driver Info #0:
    Driver Status: r8152 is active
    Driver Activation Cmd: "modprobe r8152"
  Config Status: cfg=new, avail=yes, need=no, active=unknown
  Attached to: #39 (Hub)

52: None 00.0: 10701 Ethernet
  [Created at net.126]
  Unique ID: GeD9.ndpeucax6V1
  Parent ID: WFGf.TaH2R0dMoZ8
  SysFS ID: /class/net/enx00e04c3608ec
  SysFS Device Link: /devices/pci0000:00/0000:00:14.0/usb2/2-2/2-2.1/2-2.1:1.0
  Hardware Class: network interface
  Model: "Ethernet network interface"
  Driver: "r8152"
  Driver Modules: "r8152"
  Device File: enx00e04c3608ec
  HW Address: 00:e0:4c:36:08:ec
  Permanent HW Address: 00:e0:4c:36:08:ec
  Link detected: no
  Config Status: cfg=new, avail=yes, need=no, active=unknown
  Attached to: #37 (Ethernet controller)
```

## lsusb

```shell
# lsusb
Bus 001 Device 002: ID 8087:8000 Intel Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 002 Device 003: ID 0c45:64d2 Microdia Integrated Webcam
Bus 002 Device 005: ID 0bda:8152 Realtek Semiconductor Corp. RTL8152 Fast Ethernet Adapter
Bus 002 Device 004: ID 214b:7250  USB2.0 HUB
Bus 002 Device 002: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

## usb-devices

```shell
$ usb-devices | awk '/214b/' RS=
T:  Bus=02 Lev=01 Prnt=01 Port=01 Cnt=02 Dev#=  4 Spd=480 MxCh= 4
D:  Ver= 2.00 Cls=09(hub  ) Sub=00 Prot=01 MxPS=64 #Cfgs=  1
P:  Vendor=214b ProdID=7250 Rev=01.00
S:  Product=USB2.0 HUB
C:  #Ifs= 1 Cfg#= 1 Atr=e0 MxPwr=100mA
I:  If#=0x0 Alt= 0 #EPs= 1 Cls=09(hub  ) Sub=00 Prot=00 Driver=hub
```

```shell
$ usb-devices | awk '/0bda/' RS=
T:  Bus=02 Lev=02 Prnt=04 Port=00 Cnt=01 Dev#=  5 Spd=480 MxCh= 0
D:  Ver= 2.10 Cls=00(>ifc ) Sub=00 Prot=00 MxPS=64 #Cfgs=  2
P:  Vendor=0bda ProdID=8152 Rev=20.00
S:  Manufacturer=Realtek
S:  Product=USB 10/100 LAN
S:  SerialNumber=00E04C3608EC
C:  #Ifs= 1 Cfg#= 1 Atr=a0 MxPwr=100mA
I:  If#=0x0 Alt= 0 #EPs= 3 Cls=ff(vend.) Sub=ff Prot=00 Driver=r8152
```

## lsusb verbose

```shell
# lsusb -v -d 0bda:8152
Bus 002 Device 005: ID 0bda:8152 Realtek Semiconductor Corp. RTL8152 Fast Ethernet Adapter
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               2.10
  bDeviceClass            0
  bDeviceSubClass         0
  bDeviceProtocol         0
  bMaxPacketSize0        64
  idVendor           0x0bda Realtek Semiconductor Corp.
  idProduct          0x8152 RTL8152 Fast Ethernet Adapter
  bcdDevice           20.00
  iManufacturer           1 Realtek
  iProduct                2 USB 10/100 LAN
  iSerial                 3 00E04C3608EC
  bNumConfigurations      2
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength       0x0027
    bNumInterfaces          1
    bConfigurationValue     1
    iConfiguration          0
    bmAttributes         0xa0
      (Bus Powered)
      Remote Wakeup
    MaxPower              100mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           3
      bInterfaceClass       255 Vendor Specific Class
      bInterfaceSubClass    255 Vendor Specific Subclass
      bInterfaceProtocol      0
      iInterface              0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x81  EP 1 IN
        bmAttributes            2
          Transfer Type            Bulk
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0200  1x 512 bytes
        bInterval               0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x02  EP 2 OUT
        bmAttributes            2
          Transfer Type            Bulk
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0200  1x 512 bytes
        bInterval               0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x83  EP 3 IN
        bmAttributes            3
          Transfer Type            Interrupt
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0002  1x 2 bytes
        bInterval               8
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength       0x0050
    bNumInterfaces          2
    bConfigurationValue     2
    iConfiguration          0
    bmAttributes         0xa0
      (Bus Powered)
      Remote Wakeup
    MaxPower              100mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           1
      bInterfaceClass         2 Communications
      bInterfaceSubClass      6 Ethernet Networking
      bInterfaceProtocol      0
      iInterface              5 CDC Communications Control
      CDC Header:
        bcdCDC               1.10
      CDC Union:
        bMasterInterface        0
        bSlaveInterface         1
      CDC Ethernet:
        iMacAddress                      3 00E04C3608EC
        bmEthernetStatistics    0x00000000
        wMaxSegmentSize               1514
        wNumberMCFilters            0x0000
        bNumberPowerFilters              0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x83  EP 3 IN
        bmAttributes            3
          Transfer Type            Interrupt
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0010  1x 16 bytes
        bInterval               8
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       0
      bNumEndpoints           0
      bInterfaceClass        10 CDC Data
      bInterfaceSubClass      0
      bInterfaceProtocol      0
      iInterface              0
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        1
      bAlternateSetting       1
      bNumEndpoints           2
      bInterfaceClass        10 CDC Data
      bInterfaceSubClass      0
      bInterfaceProtocol      0
      iInterface              4 Ethernet Data
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x81  EP 1 IN
        bmAttributes            2
          Transfer Type            Bulk
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0200  1x 512 bytes
        bInterval               0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x02  EP 2 OUT
        bmAttributes            2
          Transfer Type            Bulk
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0200  1x 512 bytes
        bInterval               0
Binary Object Store Descriptor:
  bLength                 5
  bDescriptorType        15
  wTotalLength       0x000c
  bNumDeviceCaps          1
  USB 2.0 Extension Device Capability:
    bLength                 7
    bDescriptorType        16
    bDevCapabilityType      2
    bmAttributes   0x00000002
      HIRD Link Power Management (LPM) Supported
can't get debug descriptor: Resource temporarily unavailable
Device Status:     0x0000
  (Bus Powered)
```

```shell
# lsusb -v -d 214b:7250
Bus 002 Device 004: ID 214b:7250  USB2.0 HUB
Device Descriptor:
  bLength                18
  bDescriptorType         1
  bcdUSB               2.00
  bDeviceClass            9 Hub
  bDeviceSubClass         0
  bDeviceProtocol         1 Single TT
  bMaxPacketSize0        64
  idVendor           0x214b
  idProduct          0x7250
  bcdDevice            1.00
  iManufacturer           0
  iProduct                1 USB2.0 HUB
  iSerial                 0
  bNumConfigurations      1
  Configuration Descriptor:
    bLength                 9
    bDescriptorType         2
    wTotalLength       0x0019
    bNumInterfaces          1
    bConfigurationValue     1
    iConfiguration          0
    bmAttributes         0xe0
      Self Powered
      Remote Wakeup
    MaxPower              100mA
    Interface Descriptor:
      bLength                 9
      bDescriptorType         4
      bInterfaceNumber        0
      bAlternateSetting       0
      bNumEndpoints           1
      bInterfaceClass         9 Hub
      bInterfaceSubClass      0
      bInterfaceProtocol      0 Full speed (or root) hub
      iInterface              0
      Endpoint Descriptor:
        bLength                 7
        bDescriptorType         5
        bEndpointAddress     0x81  EP 1 IN
        bmAttributes            3
          Transfer Type            Interrupt
          Synch Type               None
          Usage Type               Data
        wMaxPacketSize     0x0001  1x 1 bytes
        bInterval              12
Hub Descriptor:
  bLength               9
  bDescriptorType      41
  nNbrPorts             4
  wHubCharacteristic 0x00e0
    Ganged power switching
    Ganged overcurrent protection
    TT think time 32 FS bits
    Port indicators
  bPwrOn2PwrGood       50 * 2 milli seconds
  bHubContrCurrent    100 milli Ampere
  DeviceRemovable    0x00
  PortPwrCtrlMask    0xff
 Hub Port Status:
   Port 1: 0000.0503 highspeed power enable connect
   Port 2: 0000.0100 power
   Port 3: 0000.0100 power
   Port 4: 0000.0100 power
Device Qualifier (for other device speed):
  bLength                10
  bDescriptorType         6
  bcdUSB               2.00
  bDeviceClass            9 Hub
  bDeviceSubClass         0
  bDeviceProtocol         0 Full speed (or root) hub
  bMaxPacketSize0        64
  bNumConfigurations      1
can't get debug descriptor: Resource temporarily unavailable
Device Status:     0x0003
  Self Powered
  Remote Wakeup Enabled
```
