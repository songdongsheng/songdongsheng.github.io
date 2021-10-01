---
title: Systemd analyze
excerpt: Systemd analyze
date: 2019-11-09 15:18:23
tags:
  - Linux
categories: [Operating system, Linux]
---

# Systemd analyze

[systemd-analyze](https://www.freedesktop.org/software/systemd/man/systemd-analyze.html) may be used to determine system boot-up performance statistics and retrieve other state and tracing information from the system and service manager, and to verify the correctness of unit files. It is also used to access special functions useful for advanced system manager debugging.

## Sample - SSD

```
$ systemd-analyze blame
          6.939s NetworkManager-wait-online.service
          6.360s plymouth-quit-wait.service
          5.626s rabbitmq-server.service
          4.604s docker.service
          3.851s mysql.service
          3.682s postgresql@11-main.service
          1.250s keyboard-setup.service
          1.189s motd-news.service
          1.038s networking.service
           996ms apparmor.service
           972ms plymouth-start.service
           921ms networkd-dispatcher.service
           898ms ebtables.service
           886ms plymouth-read-write.service
           883ms systemd-backlight@backlight:intel_backlight.service
           821ms udisks2.service
           787ms ModemManager.service
           769ms grub-common.service
           761ms accounts-daemon.service
           728ms dev-sda3.device
           629ms NetworkManager.service
           605ms systemd-logind.service
           595ms alsa-restore.service
           566ms systemd-resolved.service
           489ms sysstat.service
           488ms pppd-dns.service
           479ms gpu-manager.service
           474ms dnscrypt-proxy.service
           396ms postfix@-.service
           314ms systemd-rfkill.service
           294ms rsyslog.service
           289ms libvirtd.service
           283ms chrony.service
           276ms nfs-server.service
           266ms nfs-mountd.service
           226ms user@121.service
           219ms systemd-journal-flush.service
           217ms wpa_supplicant.service
           192ms run-rpc_pipefs.mount
           165ms swapfile.swap
           165ms ssh.service
           164ms lvm2-monitor.service
           159ms systemd-tmpfiles-setup-dev.service
           155ms proc-fs-nfsd.mount
           148ms polkit.service
           145ms packagekit.service
           142ms systemd-udev-trigger.service
           142ms systemd-journald.service
           141ms gdm.service
           133ms rpcbind.service
           130ms systemd-modules-load.service
           126ms containerd.service
           124ms qemu-kvm.service
           123ms dns-clean.service
           114ms upower.service
           111ms apport.service
           104ms systemd-sysctl.service
            98ms setvtrgb.service
            88ms dev-mqueue.mount
            87ms systemd-random-seed.service
            82ms libvirt-guests.service
            79ms kmod-static-nodes.service
            77ms speech-dispatcher.service
            77ms c.mount
            77ms openvpn.service
            72ms console-setup.service
            67ms systemd-tmpfiles-setup.service
            57ms nfs-idmapd.service
            51ms systemd-udevd.service
            51ms nginx.service
            49ms user@1000.service
            37ms nfs-blkmap.service
            34ms systemd-networkd.service
            33ms systemd-remount-fs.service
            27ms blk-availability.service
            25ms systemd-update-utmp.service
            24ms systemd-user-sessions.service
            22ms colord.service
            21ms systemd-networkd-wait-online.service
            20ms nfs-config.service
            19ms systemd-tmpfiles-clean.service
            16ms dev-hugepages.mount
            15ms systemd-update-utmp-runlevel.service
            15ms repmgrd.service
            11ms sys-kernel-debug.mount
            11ms ureadahead-stop.service
             9ms sys-fs-fuse-connections.mount
             7ms systemd-backlight@leds:dell::kbd_backlight.service
             3ms sys-kernel-config.mount
             3ms postfix.service
             2ms postgresql.service
             1ms docker.socket
```

## Sample - HDD

```
$ systemd-analyze blame
         28.145s docker.service
         22.532s systemd-networkd-wait-online.service
         14.102s udisks2.service
         12.167s NetworkManager-wait-online.service
          8.956s systemd-journal-flush.service
          6.451s dev-mapper-os\x2droot.device
          5.882s libvirtd.service
          3.481s ModemManager.service
          3.314s boot.mount
          3.224s NetworkManager.service
          3.006s accounts-daemon.service
          2.438s lvm2-pvscan@8:19.service
          2.337s avahi-daemon.service
          2.203s sysstat.service
          2.201s pppd-dns.service
          2.198s rsyslog.service
          2.026s wpa_supplicant.service
          2.023s systemd-logind.service
          2.023s switcheroo-control.service
          2.022s alsa-restore.service
          1.758s apparmor.service
          1.534s systemd-networkd.service
          1.371s systemd-tmpfiles-setup.service
          1.312s binfmt-support.service
          1.168s systemd-udevd.service
          1.011s systemd-fsck@dev-disk-by\x2duuid-6FDA\x2d0B26.service
           904ms boot-efi.mount
           900ms nfs-mountd.service
           800ms proc-fs-nfsd.mount
           771ms ssh.service
           681ms lvm2-monitor.service
           647ms nfs-server.service
           628ms gdm.service
           535ms systemd-remount-fs.service
           499ms polkit.service
           477ms run-rpc_pipefs.mount
           473ms dev-mapper-os\x2dswap.swap
           426ms networking.service
           393ms keyboard-setup.service
           376ms systemd-udev-trigger.service
           374ms systemd-journald.service
           369ms nfs-idmapd.service
           325ms systemd-sysusers.service
           323ms colord.service
           304ms systemd-modules-load.service
           302ms libvirt-guests.service
           293ms systemd-tmpfiles-setup-dev.service
           268ms systemd-user-sessions.service
           252ms upower.service
           234ms openvpn.service
           233ms containerd.service
           206ms systemd-sysctl.service
           204ms console-setup.service
           202ms systemd-timesyncd.service
           183ms systemd-rfkill.service
           142ms systemd-random-seed.service
           138ms kmod-static-nodes.service
           130ms nfs-config.service
           117ms systemd-tmpfiles-clean.service
           110ms user@1000.service
           102ms ifupdown-pre.service
            96ms dev-mqueue.mount
            89ms blk-availability.service
            89ms sys-kernel-debug.mount
            89ms dev-hugepages.mount
            87ms systemd-update-utmp.service
            48ms nfs-blkmap.service
            23ms proc-sys-fs-binfmt_misc.mount
             9ms user-runtime-dir@1000.service
             7ms docker.socket
             7ms systemd-update-utmp-runlevel.service
             3ms sys-fs-fuse-connections.mount
```
