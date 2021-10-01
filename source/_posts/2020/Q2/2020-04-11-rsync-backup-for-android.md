---
title: Rsync backup for Android device
excerpt: Rsync backup for Android device
date: 2020-04-11 09:58:11
tags:
  - Programming
  - C/C++
categories: [Programming, C/C++]
---

# Rsync backup for Android device

After [compile rsync by NDK](/2020/04/06/ndk-rsync-compile/), we can do rsync backup for Android device.

## Debian Linux preparation

```bash
sudo apt-get update && sudo apt-get dist-upgrade --no-install-recommends -y && \
    sudo apt-get install --no-install-recommends -y android-tools-adb rsync

```

## Android device preparation

1. Enter "Developer Mode"
2. Enable "USB Debugging"
3. Plug in your Android device via USB cable to your backup target machine
4. Allows "USB debugging" on the Android device
5. Root is recommended. You can run rsync process as normal user, but would be restricted in the files are permitted to read from your Android device.

## Environmental verification

```bash
$ dmesg -w
[90130.442467] usb 2-12: new high-speed USB device number 18 using xhci_hcd
[90130.464457] usb 2-12: New USB device found, idVendor=2717, idProduct=ff48, bcdDevice= 3.18
[90130.464462] usb 2-12: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[90130.464465] usb 2-12: Product: MI 5
[90130.464467] usb 2-12: Manufacturer: Xiaomi
[90130.464469] usb 2-12: SerialNumber: 19975a8b
[90132.762782] usb 2-12: USB disconnect, device number 18

$ lsusb | grep Xiaomi
Bus 002 Device 021: ID 2717:ff48 Xiaomi Inc. Mi/Redmi series (MTP + ADB)

$ adb devices
List of devices attached
* daemon not running; starting now at tcp:5037
* daemon started successfully
19975a8b	device

$ adb shell df -h
Filesystem       Size  Used Avail Use% Mounted on
rootfs           1.2G  6.1M  1.2G   1% /
tmpfs            1.2G  732K  1.2G   1% /dev
/dev/block/dm-0  2.8G  2.6G  190M  94% /system
tmpfs            1.2G     0  1.2G   0% /mnt
none             1.2G     0  1.2G   0% /sys/fs/cgroup
/dev/block/sde38 806M  480M  310M  61% /cust
/dev/block/sda12  27M  1.4M   25M   6% /persist
/dev/block/sda13 248M  204K  243M   1% /cache
/dev/block/sde32  12M  4.1M  7.3M  36% /dsp
/dev/block/sde35 192M   98M   94M  51% /firmware
/dev/block/sde26 1.0G  112K  1.0G   1% /bt_firmware
/dev/block/dm-1   54G   43G   11G  80% /data
/data/media       54G   43G   11G  80% /storage/emulated

/sdcard -> /storage/self/primary
storage/self/primary -> /mnt/user/0/primary
/mnt/user/0/primary -> /storage/emulated/0
```

## Backup preparation

### Push rsync to Android device

```bash
adb push rsync-3.1.3.aarch64-android /data/local/tmp/rsync
adb shell chmod 755 /data/local/tmp/rsync
adb shell /data/local/tmp/rsync --version
```

Please use the correct [rsync binary](/2020/04/06/ndk-rsync-compile/) for the Android device. If you got error messages like the following during backup:

```bash
/system/bin/sh: /data/local/tmp/rsync: not executable: 64-bit ELF file
```

or

```bash
rsync: safe_read failed to read 1 bytes [Receiver]: Connection reset by peer (104)
rsync error: error in rsync protocol data stream (code 12) at io.c(285) [Receiver=3.1.3]
```

Then you must have chosen the wrong rsync binary.

### Push rsyncd.conf to Android device

```bash
adb shell 'exec > /data/local/tmp/rsyncd.conf && echo address = 127.0.0.1 && echo port = 1873 && echo "[root]" && echo path = / && echo use chroot = false && echo read only = false'
```

or

```bash
cat << EOF > rsyncd.conf
address = 127.0.0.1
port = 1873

[root]
path = /
chroot = false
read only = false

# If Android device rooted
#uid = root
#gid = root
EOF

adb push rsyncd.conf /data/local/tmp/rsyncd.conf
```

### Open up a shell on Android device

```bash
adb -s A5RNW18316011440 shell

# If Android device rooted
# su -
```

### Run the rsync server process on Android device

If Android device rooted:

```bash
adb -s A5RNW18316011440 shell su - -c "/data/local/tmp/rsync --daemon --no-detach --config=/data/local/tmp/rsyncd.conf --log-file=/proc/self/fd/2"
```

Otherwise:

```bash
adb shell '/data/local/tmp/rsync --daemon --no-detach --config=/data/local/tmp/rsyncd.conf --log-file=/proc/self/fd/2'
# adb shell '/data/local/tmp/rsync --daemon --config=/data/local/tmp/rsyncd.conf &'
```

### Forward the target machine tcp port to Android device

```bash
adb forward tcp:1873 tcp:1873
```

## Backup files from Android device to target machine

```bash
$ rsync -vrt --progress --stats \
    rsync://localhost:1873/root/ \
    ~/xiaomi-backup/
```
