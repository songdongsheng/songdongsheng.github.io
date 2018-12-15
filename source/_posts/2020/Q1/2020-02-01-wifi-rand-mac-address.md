---
title: Wi-Fi MAC addres Randomization
description: Wi-Fi MAC addres Randomization
date: 2020-02-01 09:41:02
tags:
  - Linux
categories: [Operating system, Linux]
permalink: wifi-rand-mac-address
---

# Wi-Fi MAC addres Randomization

The NetworkManager 1.2.0 added MAC address randomization for Wi-Fi. The NetworkManager release 1.4.0 adds new features to change the current MAC address of your Ethernet or Wi-Fi card. This is also called MAC address “spoofing” or “cloning”.

NetworkManager supports two types MAC Address Randomization: randomization during scanning, and for network connections. Both modes can be configured by modifying **/etc/NetworkManager/NetworkManager.conf**.

MAC randomization can be used for increased privacy by not disclosing your real MAC address to the network.

## Randomization during Wi-Fi scanning

During Wi-Fi scanning, NetworkManager resets the MAC address frequently to a randomly generated address. This default behavior can be disabled with a global configuration option in NetworkManager.conf

```
[main]
plugins=ifupdown,keyfile

[ifupdown]
managed=false

[device]
wifi.scan-rand-mac-address=no
```

## MAC randomization for network connections

MAC randomization for network connections can be set to different modes for both wireless and ethernet interfaces. In terms of MAC randomization the most important modes are stable and random. stable generates a random MAC address when you connect to a new network and associates the two permanently. This means that you will use the same MAC address every time you connect to that network. In contrast, random will generate a new MAC address every time you connect to a network, new or previously known. You can configure the MAC randomization by adding the desired configuration under /etc/NetworkManager/conf.d:

```
[device-mac-randomization]
# "yes" is already the default for scanning
wifi.scan-rand-mac-address=yes

[connection-mac-randomization]
# Randomize MAC for every ethernet connection
ethernet.cloned-mac-address=random
# Generate a random MAC for each WiFi and associate the two permanently.
wifi.cloned-mac-address=stable
```

## Troubleshooting

With Wi-Fi MAC addres Randomization, some Wi-Fi device can't establish connection with any AP.

Disabling MAC address randomization may be needed to get (stable) link connection and/or networks that restrict devices based on their MAC Address or have a limit network capacity.
