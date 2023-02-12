---
title: 'Run Linux GUI Programs With WSLg Through OCI Image'
excerpt: 'Run Linux GUI Programs With WSLg Through OCI Image'
date: 2023-02-12 13:57:27
tags:
  - Linux
  - Windows
categories:
  - Operating system
---

# Run Linux GUI Programs With WSLg Through OCI Image

+ [Windows Subsystem for Linux GUI](https://github.com/microsoft/wslg)
+ [mounting of /mnt/wslg as user runtime dir breaks all users except that with uid=1000](https://github.com/microsoft/WSL/issues/9025)
+ [Run Linux GUI apps with WSL](https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps)
+ [Fcitx5](https://wiki.archlinux.org/title/Fcitx5)
+ [Alpine Linux 3.17 - 3M](https://dl-cdn.alpinelinux.org/alpine/latest-stable/releases/x86_64/alpine-minirootfs-3.17.2-x86_64.tar.gz)
+ [Ubuntu 20.04 OCI - 28MB](https://partner-images.canonical.com/oci/focal/current/ubuntu-focal-oci-amd64-root.tar.gz)
+ [Ubuntu 22.04 OCI - 30MB](https://partner-images.canonical.com/oci/jammy/current/ubuntu-jammy-oci-amd64-root.tar.gz)
+ [Debian 11 - 33MB](https://github.com/debuerreotype/docker-debian-artifacts/raw/dist-amd64/bullseye/rootfs.tar.xz)
+ [Debian 12 - 30MB](https://github.com/debuerreotype/docker-debian-artifacts/raw/dist-amd64/bookworm/rootfs.tar.xz)

## Install WSL2

```
PS C:\> wsl --list --online
The following is a list of valid distributions that can be installed.
Install using 'wsl.exe --install <Distro>'.

NAME                                   FRIENDLY NAME
Ubuntu                                 Ubuntu
Debian                                 Debian GNU/Linux
kali-linux                             Kali Linux Rolling
Ubuntu-18.04                           Ubuntu 18.04 LTS
Ubuntu-20.04                           Ubuntu 20.04 LTS
Ubuntu-22.04                           Ubuntu 22.04 LTS
OracleLinux_8_5                        Oracle Linux 8.5
OracleLinux_7_9                        Oracle Linux 7.9
SUSE-Linux-Enterprise-Server-15-SP4    SUSE Linux Enterprise Server 15 SP4
openSUSE-Leap-15.4                     openSUSE Leap 15.4
openSUSE-Tumbleweed                    openSUSE Tumbleweed

PS C:\> wsl --install Debian
PS C:\> wsl --install Ubuntu
PS C:\> wsl --install Ubuntu-22.04
PS C:\> wsl --install openSUSE-Leap-15.4
```

## Attaches and mounts disk in all WSL2 distributions

### Create Virtual Disk

```
qemu-img create -f qcow2 -o compression_type=zstd share-disk.qcow2 64G
qemu-img info share-disk.qcow2

qemu-img create -f vhdx share-disk.vhdx 64G
qemu-img info share-disk.vhdx
```


### Mount Virtual Disk - Windows

```
wsl --unmount share-disk.vhdx

wsl --mount --vhd share-disk.vhdx
```

### Mount Virtual Disk - Linux

```
# gdisk -l /dev/sde
GPT fdisk (gdisk) version 1.0.8

Partition table scan:
  MBR: not present
  BSD: not present
  APM: not present
  GPT: not present

Creating new GPT entries in memory.
Disk /dev/sde: 2147483648 sectors, 1024.0 GiB
Model: Virtual Disk
Sector size (logical/physical): 512/4096 bytes
Disk identifier (GUID): B38F5AE6-9ED7-4C65-9F21-6A9206A5AD7D
Partition table holds up to 128 entries
Main partition table begins at sector 2 and ends at sector 33
First usable sector is 34, last usable sector is 2147483614
Partitions will be aligned on 2048-sector boundaries
Total free space is 2147483581 sectors (1024.0 GiB)

Number  Start (sector)    End (sector)  Size       Code  Name
```

```
# mount /dev/sde /mnt/Debian-12-ext4
# cd /mnt/Debian-12-ext4
# ls -ln
total 76
lrwxrwxrwx  1 0 0     7 Feb  8 13:00 bin -> usr/bin
drwxr-xr-x  2 0 0  4096 Oct  3 21:30 boot
drwxr-xr-x  2 0 0  4096 Feb  8 13:00 dev
drwxr-xr-x 29 0 0  4096 Feb 12 08:53 etc
drwxr-xr-x  2 0 0  4096 Oct  3 21:30 home
-rwxr-xr-x  1 0 0     0 Feb 12 08:53 init
lrwxrwxrwx  1 0 0     7 Feb  8 13:00 lib -> usr/lib
lrwxrwxrwx  1 0 0     9 Feb  8 13:00 lib32 -> usr/lib32
lrwxrwxrwx  1 0 0     9 Feb  8 13:00 lib64 -> usr/lib64
lrwxrwxrwx  1 0 0    10 Feb  8 13:00 libx32 -> usr/libx32
drwx------  2 0 0 16384 Feb 12 08:53 lost+found
drwxr-xr-x  2 0 0  4096 Feb  8 13:00 media
drwxr-xr-x  5 0 0  4096 Feb 12 08:53 mnt
drwxr-xr-x  2 0 0  4096 Feb  8 13:00 opt
drwxr-xr-x  2 0 0  4096 Oct  3 21:30 proc
drwx------  2 0 0  4096 Feb 12 08:54 root
drwxr-xr-x  3 0 0  4096 Feb  8 13:00 run
lrwxrwxrwx  1 0 0     8 Feb  8 13:00 sbin -> usr/sbin
drwxr-xr-x  2 0 0  4096 Feb  8 13:00 srv
drwxr-xr-x  2 0 0  4096 Oct  3 21:30 sys
drwxrwxrwt  3 0 0  4096 Feb 12 08:53 tmp
drwxr-xr-x 14 0 0  4096 Feb  8 13:00 usr
drwxr-xr-x 11 0 0  4096 Feb  8 13:00 var

# dmesg
...
[ 3286.554788] scsi 0:0:0:4: Direct-Access     Msft     Virtual Disk     1.0  PQ: 0 ANSI: 5
[ 3286.558599] sd 0:0:0:4: Attached scsi generic sg4 type 0
[ 3286.559426] sd 0:0:0:4: [sde] 2147483648 512-byte logical blocks: (1.10 TB/1.00 TiB)
[ 3286.560482] sd 0:0:0:4: [sde] 4096-byte physical blocks
[ 3286.561402] sd 0:0:0:4: [sde] Write Protect is off
[ 3286.562124] sd 0:0:0:4: [sde] Mode Sense: 0f 00 00 00
[ 3286.563161] sd 0:0:0:4: [sde] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
[ 3286.565743] sd 0:0:0:4: [sde] Attached SCSI disk
[ 3286.578015] WSL (397): Detected ext4 filesystem for device: /dev/sde
[ 3286.614048] EXT4-fs (sde): mounted filesystem with ordered data mode. Quota mode: none.
```

## Debian 12

### Import Debian Image

```
curl -L -o debian-minirootfs-12-x86_64.tar.gz https://github.com/debuerreotype/docker-debian-artifacts/raw/dist-amd64/bookworm/rootfs.tar.xz
wsl --import Debian-12 . debian-minirootfs-12-x86_64.tar.gz
wsl -d Debian-12
```

### Setup Debian

```
libsystemd0-249.14-150400.8.19.1.x86_64
    /usr/lib64/libsystemd.so.0
    /usr/lib64/libsystemd.so.0.32.0

systemd-249.14-150400.8.19.1.x86_64
    /bin/systemctl
    /usr/bin/bootctl
    /usr/bin/busctl
    /usr/bin/hostnamectl
    /usr/bin/journalctl
    /usr/bin/kernel-install
    /usr/bin/localectl
    /usr/bin/loginctl
    /usr/bin/systemctl
    /usr/bin/timedatectl
util-linux-systemd-2.37.2-150400.8.14.1.x86_64
    /usr/bin/logger
    /usr/bin/lslogins

cat << EOF | tee /etc/wsl.conf
[boot]
systemd=true

[network]
generateResolvConf = false
EOF

rm -fr /etc/resolv.conf && cat << EOF | tee /etc/resolv.conf
nameserver 193.181.14.10
nameserver 193.181.14.11
nameserver 193.181.14.12
EOF

chattr +iS /etc/resolv.conf
```

```
cat << EOF | tee ~/.vimrc
set ignorecase
set nocompatible
set showcmd

syntax on
set nowrap
set autoindent
set nu
set paste
set mouse-=a
EOF
```

```
apt update && apt upgrade -y --no-install-recommends; \
apt install -y --no-install-recommends locales whiptail

dpkg-reconfigure locales

apt install -y --no-install-recommends \
    adwaita-icon-theme \
    at-spi2-core \
    ca-certificates \
    emacs-gtk \
    gedit \
    greybird-gtk-theme \
    librsvg2-2 \
    mesa-utils \
    ukui-themes \
    vim-gtk3 \
    wayland-utils \
    weston
```

### Font Configuration

Let's add the Windows fonts & disabling bitmap fonts first.

```
fc-list ':' file | sort
```

```
vi /etc/fonts/fonts.conf

    <!-- add Windows fonts -->
    <dir>/mnt/c/Windows/Fonts/</dir>

    <dir>/usr/share/fonts</dir>
    <dir prefix="xdg">fonts</dir>

    <cachedir>/var/cache/fontconfig</cachedir>
    <cachedir prefix="xdg">fontconfig</cachedir>


    <selectfont>
        <rejectfont>
            <pattern>
                <patelt name="scalable"><bool>false</bool></patelt>
            </pattern>
        </rejectfont>
    </selectfont>
```

```
<?xml version="1.0"?>
<!--
    vi /etc/fonts/fonts.conf
    XMLLINT_INDENT="    " xmllint \-\-format /etc/fonts/fonts.conf
    https://www.freedesktop.org/software/fontconfig/fontconfig-user.html
  -->
<fontconfig>
    <dir>/usr/share/fonts</dir>
    <dir prefix="xdg">fonts</dir>
    <dir>/mnt/c/Windows/Fonts/</dir>

    <cachedir>/var/cache/fontconfig</cachedir>
    <cachedir prefix="xdg">fontconfig</cachedir>

    <config>
        <rescan>
            <int>30</int>
        </rescan>
    </config>

    <selectfont>
        <rejectfont>
            <pattern>
                <patelt name="scalable">
                    <bool>false</bool>
                </patelt>
            </pattern>
        </rejectfont>
    </selectfont>

    <match target="pattern">
        <test qual="any" name="family">
            <string>mono</string>
        </test>
        <edit name="family" mode="assign">
            <string>monospace</string>
        </edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family">
            <string>sans</string>
        </test>
        <edit name="family" mode="assign" binding="same">
            <string>sans-serif</string>
        </edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family">
            <string>sans serif</string>
        </test>
        <edit name="family" mode="assign" binding="same">
            <string>sans-serif</string>
        </edit>
    </match>

    <match target="pattern">
        <test qual="all" name="family" compare="not_eq">
            <string>sans-serif</string>
        </test>
        <test qual="all" name="family" compare="not_eq">
            <string>serif</string>
        </test>
        <test qual="all" name="family" compare="not_eq">
            <string>monospace</string>
        </test>
        <edit name="family" mode="append_last">
            <string>sans-serif</string>
        </edit>
    </match>

    <match target="font" >
        <edit mode="assign" name="autohint">  <bool>false</bool></edit>
        <edit mode="assign" name="hinting">   <bool>true</bool></edit>
        <edit mode="assign" name="hintstyle"> <const>hintslight</const></edit>
        <edit mode="assign" name="antialias"> <bool>true</bool></edit>
        <edit mode="assign" name="lcdfilter"> <const>lcddefault</const></edit>
        <edit mode="assign" name="rgba">      <const>rgb</const></edit>
    </match>

    <alias>
        <family>sans-serif</family>
        <prefer>
            <family>Helvetica</family>
            <family>Myriad Pro</family>
            <family>Calibri</family>
            <family>Verdana</family>
            <family>Adobe Pi Std</family>
            <family>Symbol Std</family>
            <family>Cambria Math</family>
            <family>Microsoft YaHei Light</family>
            <family>Microsoft JhengHei Light</family>
            <family>SimHei</family>
            <family>NSimSun</family>
            <family>SimSun-ExtB</family>
            <family>Adobe Fan Heiti Std</family>
            <family>Adobe Heiti Std</family>
        </prefer>
    </alias>

    <alias>
        <family>serif</family>
        <prefer>
            <family>Times New Roman</family>
            <family>Minion Pro</family>
            <family>Cambria</family>
            <family>Georgia</family>
            <family>Adobe Pi Std</family>
            <family>Symbol Std</family>
            <family>Cambria Math</family>
            <family>Microsoft YaHei Light</family>
            <family>Microsoft JhengHei Light</family>
            <family>NSimSun</family>
            <family>SimSun\-ExtB</family>
            <family>Adobe Ming Std</family>
            <family>Adobe Song Std</family>
        </prefer>
    </alias>

    <alias>
        <family>monospace</family>
        <prefer>
            <family>JetBrains Mono</family>
            <family>Consolas</family>
            <family>Courier Std</family>
            <family>Courier New</family>
            <family>Adobe Pi Std</family>
            <family>Symbol Std</family>
            <family>Cambria Math</family>
            <family>Microsoft YaHei Light</family>
            <family>Microsoft JhengHei Light</family>
            <family>NSimSun</family>
            <family>SimSun-ExtB</family>
            <family>Adobe Ming Std</family>
            <family>Adobe Song Std</family>
        </prefer>
    </alias>
</fontconfig>
```

```
fc-cache -rv

fc-match -s 'monospace' | head
fc-match -s 'serif' | head
fc-match -s 'sans-serif' | head
```

### Wayland Information

```
glxinfo -B
```

```
# wayland-info || weston-info
interface: 'wl_compositor',                              version:  4, name:  1
interface: 'wl_subcompositor',                           version:  1, name:  2
interface: 'wp_viewporter',                              version:  1, name:  3
interface: 'zxdg_output_manager_v1',                     version:  2, name:  4
        xdg_output_v1
                output: 21
                name: 'rdp-2'
                logical_x: 1920, logical_y: 0
                logical_width: 1920, logical_height: 1080
        xdg_output_v1
                output: 11
                name: 'rdp-0'
                logical_x: 0, logical_y: 0
                logical_width: 1920, logical_height: 1080
interface: 'wp_presentation',                            version:  1, name:  5
        presentation clock id: 4 (CLOCK_MONOTONIC_RAW)
interface: 'zwp_relative_pointer_manager_v1',            version:  1, name:  6
interface: 'zwp_pointer_constraints_v1',                 version:  1, name:  7
interface: 'zwp_input_timestamps_manager_v1',            version:  1, name:  8
interface: 'wl_data_device_manager',                     version:  3, name:  9
interface: 'wl_shm',                                     version:  1, name: 10
        formats (fourcc):
        0x36314752 = 'RG16'
                 1 = 'XR24'
                 0 = 'AR24'
interface: 'wl_output',                                  version:  3, name: 11
        x: 0, y: 0, scale: 1,
        physical_width: 0 mm, physical_height: 0 mm,
        make: 'weston', model: 'rdp',
        subpixel_orientation: unknown, output_transform: normal,
        mode:
                width: 1920 px, height: 1080 px, refresh: 60.000 Hz,
                flags: current preferred
interface: 'zwp_input_panel_v1',                         version:  1, name: 12
interface: 'zwp_text_input_manager_v1',                  version:  1, name: 13
interface: 'xdg_wm_base',                                version:  1, name: 14
interface: 'zxdg_shell_v6',                              version:  1, name: 15
interface: 'wl_shell',                                   version:  1, name: 16
interface: 'weston_rdprail_shell',                       version:  1, name: 17
interface: 'weston_screenshooter',                       version:  1, name: 18
interface: 'wl_seat',                                    version:  7, name: 19
        name: RDP CN-00124803
        capabilities: pointer keyboard
        keyboard repeat rate: 40
        keyboard repeat delay: 400
interface: 'zwp_input_method_v1',                        version:  1, name: 20
interface: 'wl_output',                                  version:  3, name: 21
        x: 1920, y: 0, scale: 1,
        physical_width: 508 mm, physical_height: 286 mm,
        make: 'weston', model: 'rdp',
        subpixel_orientation: unknown, output_transform: normal,
        mode:
                width: 1920 px, height: 1080 px, refresh: 60.000 Hz,
                flags: current preferred
```

### Weston Compositor

```
mkdir -p ~/.config/ && vi ~/.config/weston.ini
[libinput]
enable-tap=true

[shell]
cursor-theme=Adwaita
cursor-size=24

[terminal]
font=monospace
font-size=18

[output]
name=rdp-0
scale=2

[output]
name=rdp-2
scale=3
```

### GTK with HiDPI

1. https://docs.gtk.org/gtk3/x11.html
2. https://docs.gtk.org/gtk4/x11.html
3. https://www.qt.io/blog/2016/01/26/high-dpi-support-in-qt-5-6
3. https://wiki.archlinux.org/title/HiDPI

```
export QT_AUTO_SCREEN_SCALE_FACTOR=1

GDK_SCALE=1.7 GDK_DPI_SCALE=1.7 GTK_THEME=Adwaita emacs
GDK_SCALE=1.7 GDK_DPI_SCALE=1.7 GTK_THEME=Greybird emacs
```

### fcitx

#### Installation

```
apt-get install -y --no-install-recommends \
    fcitx \
    fcitx-config-gtk \
    fcitx-frontend-gtk2 \
    fcitx-frontend-gtk3 \
    fcitx-libpinyin \
    fcitx-module-dbus \
    fcitx-module-x11 \
    fcitx-rime \
    fcitx-table-emoji \
    fcitx-table-latex \
    fcitx-table-wbpy \
    fcitx-ui-classic

apt-get install -y --no-install-recommends \
    rime-data-emoji \
    rime-data-luna-pinyin \
    rime-data-terra-pinyin \
    rime-data-wubi

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx

fcitx-autostart
fcitx-config-gtk3

CTRL + ALT: Trigger input method
SPACE:      Toggle English and input method
CTRL + SHIFT: switch input method
```

#### Daily Usage

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx

fcitx-autostart
```

### ibus

#### Installation

```
apt-get install -y --no-install-recommends \
    ibus \
    ibus-gtk \
    ibus-gtk3 \
    ibus-gtk4 \
    ibus-input-pad \
    ibus-libpinyin \
    ibus-rime \
    ibus-table-emoji \
    ibus-table-latex \
    ibus-wayland \
    rime-data-emoji \
    rime-data-luna-pinyin \
    rime-data-terra-pinyin

unset XDG_RUNTIME_DIR
export QT_IM_MODULE=ibus
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus

dbus-launch ibus-daemon -drx

dbus-launch ibus-setup

pkill ibus-daemon
dbus-launch ibus-daemon -drx
```

#### Daily Usage

```
unset XDG_RUNTIME_DIR
export QT_IM_MODULE=ibus
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus

ibus-daemon -drx

Press Ctrl + space on Windows virtual keyboard can switch ibus input methods.
```

## Ubuntu 22.04

### Create WSL Distribution From RootFs

```
curl -LO https://partner-images.canonical.com/oci/jammy/current/ubuntu-jammy-oci-amd64-root.tar.gz
wsl --import Ubuntu-22.04 . ubuntu-jammy-oci-amd64-root.tar.gz
```

### Sets a Distribution as Default

```
PS C:\> wsl -s Ubuntu-22.04
PS C:\> wsl --list -v
  NAME            STATE           VERSION
* Ubuntu-22.04    Stopped         2
  sles-15         Stopped         2
  sles-12         Stopped         2
```

### Use Environment Variables

```
DISPLAY=:0
GPG_TTY=/dev/pts/0
HOSTTYPE=x86_64
LS_OPTIONS=-N --color=tty -T 0
PULSE_SERVER=/mnt/wslg/PulseServer
TERM=xterm-256color
WAYLAND_DISPLAY=wayland-0
WSL2_GUI_APPS_ENABLED=1
WSL_DISTRO_NAME=Ubuntu-22.04

alias beep='echo -en "\007"'
alias ip='ip --color=auto'
```

```
~> type _ls
_ls is a function
_ls ()
{
    local IFS=' ';
    command ls $LS_OPTIONS ${1+"$@"}
}
```

### Update DNS Configuration

```
wsl -d Ubuntu-22.04

cat << EOF | tee /etc/wsl.conf
[network]
generateResolvConf = false
EOF

rm -fr /etc/resolv.conf && cat << EOF | tee /etc/resolv.conf
nameserver 193.181.14.10
nameserver 193.181.14.11
nameserver 193.181.14.12
EOF

chattr +iS /etc/resolv.conf
```

### Configure Locales

```
apt update && apt upgrade -y --no-install-recommends; \
apt install -y --no-install-recommends locales whiptail

# locale -a
C
C.utf8
POSIX

# dpkg-reconfigure locales
Generating locales (this might take a while)...
  en_US.UTF-8... done
  zh_CN.UTF-8... done
Generation complete.
```

### Update VIM Configuration

```
cat << EOF | tee ~/.vimrc
set ignorecase
set nocompatible
set showcmd

syntax on
set nowrap
set autoindent
set nu
set paste
set mouse-=a
EOF
```

### Create User (uid 1000 issue)

```
mount --bind /mnt/wslg/runtime-dir /run/user/$1
umount /run/user/$1
```

```
apt-get install -y --no-install-recommends sudo vim

groupadd -g 1000 dongsheng || addgroup -g 1000 dongsheng
useradd -g dongsheng -G sudo -u 1000 -s /bin/bash -m dongsheng

# id dongsheng
uid=1000(dongsheng) gid=1000(dongsheng) groups=1000(dongsheng),27(sudo)

vi /etc/sudoers
    # User_List Host_List=Runas_Spec? Option_Spec* (Tag_Spec ':')* command_list
    %sudo   ALL=(ALL:ALL) ALL
        ->
    %sudo   ALL=(ALL:ALL) NOPASSWD: ALL
    %wheel  ALL=(ALL:ALL) NOPASSWD: ALL
```

### Configure Default User

```
PS C:\> Get-ItemProperty Registry::HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss\*\ DistributionName | Get-ItemProperty | Format-Table -Property DistributionName,Version,State,Flags,DefaultUid,BasePath

DistributionName Version State Flags DefaultUid BasePath
---------------- ------- ----- ----- ---------- --------
Ubuntu-22.04           2     1    15          0 \\?\C:\opt\wsl2\scratch
sles-15                2     1    15       1000 \\?\C:\opt\wsl2\sles-15
sles-12                2     1    15          0 \\?\C:\opt\wsl2\sles-12

Get-ItemProperty Registry::HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss\*\ DistributionName | Where-Object -Property DistributionName -eq Ubuntu-22.04 | Set-ItemProperty -Name DefaultUid -Value 1000
```

### Install The GUI Editor Program

```
sudo apt-get install -y --no-install-recommends adwaita-icon-theme-full at-spi2-core dbus-x11 emacs-gtk gedit
```

### GTK Configuration

#### GTK 2

GIMP (2.10.32) is the only useful GUI program that still uses GTK 2.

```
# vi ~/.gtkrc-2.0

gtk-icon-theme-name = Adwaita
# gtk-cursor-theme-size = 16
gtk-font-name = "sans-serif 16"
gtk-theme-name = Greybird
```

#### GTK 3

Most gnome programs now use GTK 3.

```
# mkdir -p  ~/.config/gtk-3.0/ && vi ~/.config/gtk-3.0/settings.ini

[Settings]
gtk-cursor-theme-name = Adwaita
gtk-cursor-theme-size = 16

gtk-font-name = sans-serif 16

gtk-icon-theme-name = Adwaita
gtk-theme-name = Greybird
gtk-key-theme-name = Emacs
```

#### GTK 4

Only a few gnome programs use GTK 4 today.

```
# mkdir -p  ~/.config/gtk-4.0/ && vi ~/.config/gtk-4.0/settings.ini

[Settings]
gtk-cursor-theme-name = Adwaita
gtk-cursor-theme-size = 16

gtk-font-name = sans-serif 16

gtk-icon-theme-name = Adwaita
gtk-theme-name = Greybird
gtk-key-theme-name = Emacs
```

### wayland-info

```
interface: 'wl_compositor', version: 4, name: 1
interface: 'wl_subcompositor', version: 1, name: 2
interface: 'wp_viewporter', version: 1, name: 3
interface: 'zxdg_output_manager_v1', version: 2, name: 4
        xdg_output_v1
                output: 24
                name: 'rdp-5'
                logical_x: 1920, logical_y: 0
                logical_width: 1280, logical_height: 720
        xdg_output_v1
                output: 11
                name: 'rdp-0'
                logical_x: 0, logical_y: 0
                logical_width: 1920, logical_height: 1080
interface: 'wp_presentation', version: 1, name: 5
        presentation clock id: 4 (CLOCK_MONOTONIC_RAW)
interface: 'zwp_relative_pointer_manager_v1', version: 1, name: 6
interface: 'zwp_pointer_constraints_v1', version: 1, name: 7
interface: 'zwp_input_timestamps_manager_v1', version: 1, name: 8
interface: 'wl_data_device_manager', version: 3, name: 9
interface: 'wl_shm', version: 1, name: 10
        formats: RGB565 XRGB8888 ARGB8888
interface: 'wl_output', version: 3, name: 11
        x: 0, y: 0, scale: 1,
        physical_width: 0 mm, physical_height: 0 mm,
        make: 'weston', model: 'rdp',
        subpixel_orientation: unknown, output_transform: normal,
        mode:
                width: 1920 px, height: 1080 px, refresh: 60.000 Hz,
                flags: current preferred
interface: 'zwp_input_panel_v1', version: 1, name: 12
interface: 'zwp_text_input_manager_v1', version: 1, name: 13
interface: 'xdg_wm_base', version: 1, name: 14
interface: 'zxdg_shell_v6', version: 1, name: 15
interface: 'wl_shell', version: 1, name: 16
interface: 'weston_rdprail_shell', version: 1, name: 17
interface: 'weston_screenshooter', version: 1, name: 18
interface: 'wl_seat', version: 7, name: 19
        name: RDP CN-00124803
        capabilities: pointer keyboard
        keyboard repeat rate: 40
        keyboard repeat delay: 400
interface: 'zwp_input_method_v1', version: 1, name: 20
interface: 'wl_output', version: 3, name: 24
        x: 1920, y: 0, scale: 3,
        physical_width: 1110 mm, physical_height: 620 mm,
        make: 'weston', model: 'rdp',
        subpixel_orientation: unknown, output_transform: normal,
        mode:
                width: 3840 px, height: 2160 px, refresh: 60.000 Hz,
                flags: current preferred
```

### Font Configuration

```
$ fc-list | awk -F : '{ print $1}' | sort -u
/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSansMono-Bold.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSansMono.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSerif-Bold.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSerif.ttf
```

#### Add Windows Fonts

```
vi /etc/fonts/fonts.conf
    <dir>/mnt/c/Windows/Fonts/</dir>
```

```
fc-cache -rv

fc-match -s 'monospace' | head
fc-match -s 'serif' | head
fc-match -s 'sans-serif' | head

Arial, Tahoma, Calibri, Segoe UI,
Times New Roman, Cambria, Segoe UI Symbol, Segoe UI Emoji
Consolas, Courier New, Segoe UI Symbol, Segoe UI Emoji

Segoe UI Symbol
Segoe UI Emoji
Microsoft YaHei
Microsoft JhengHei

for range in $(fc-match --format='%{charset}\n' "Segoe UI Emoji"); do
    for n in $(seq "0x${range%-*}" "0x${range#*-}"); do
        printf "%04x\n" "$n";
    done;
done | while read -r n_hex; do
    count=$((count + 1));
    printf "%-5s\U$n_hex\t" "$n_hex";
    [ $((count % 10)) = 0 ] && printf "\n";
done

```

#### Disable Embedded Bitmap Fonts

```
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <match target="font">
    <edit name="embeddedbitmap" mode="assign">
      <bool>false</bool>
    </edit>
  </match>
</fontconfig>
```

#### Fix Unrecognized Font Family

```

<!-- by default fontconfig assumes any unrecognized font is sans-serif, so -->
<!-- the fonts above now have /both/ families.  fix this. -->
<!-- note that "delete" applies to the first match -->
<match>
    <test compare="eq" name="family">
        <string>sans-serif</string>
    </test>
    <test compare="eq" name="family">
        <string>monospace</string>
    </test>
    <edit mode="delete" name="family"/>
</match>


<match target="pattern">
        <test qual="all" name="family" compare="not_eq">
                <string>sans-serif</string>
        </test>
        <test qual="all" name="family" compare="not_eq">
                <string>serif</string>
        </test>
        <test qual="all" name="family" compare="not_eq">
                <string>monospace</string>
        </test>
        <edit name="family" mode="append_last">
                <string>sans-serif</string>
        </edit>
</match>
```

#### Setting Default Fonts

```

    <alias>
        <family>sans-serif</family>
        <prefer>
            <family>Arial</family>
            <family>Helvetica</family>
            <family>Symbol</family>
            <family>Microsoft YaHei UI</family>
            <family>SimSun-ExtB</family>
        </prefer>
    </alias>

    <alias>
        <family>serif</family>
        <prefer>
            <family>Times New Roman</family>
            <family>Georgia</family>
            <family>Symbol</family>
            <family>Cambria</family>
            <family>Cambria Math</family>
            <family>Microsoft YaHei UI</family>
            <family>SimSun\-ExtB</family>
        </prefer>
    </alias>

    <alias>
        <family>monospace</family>
        <prefer>
            <family>JetBrains Mono</family>
            <family>Consolas</family>
            <family>Courier New</family>
            <family>Symbol</family>
            <family>Cambria Math</family>
            <family>Microsoft YaHei UI</family>
            <family>SimSun-ExtB</family>
        </prefer>
    </alias>
```

### Input Method (IM) Framework - Intelligent Input Bus (IBus)

```
apt-get install -y --no-install-recommends \
    ibus ibus-gtk ibus-gtk3 ibus-gtk4 ibus-input-pad ibus-libpinyin \
    ibus-wayland
```

```
export NO_AT_BRIDGE=1

export QT_IM_MODULE=ibus
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus

dbus-launch ibus-daemon -drx

dbus-launch ibus-setup


pkill ibus-daemon
dbus-launch ibus-daemon -drx
```

```
libpulse-dev - PulseAudio client development headers and libraries
libpulse-java - PulseAudio sound driver for Java
libao-dev - Cross Platform Audio Output Library Development
libasound2-dev - shared library for ALSA applications -- development files

sudo apt-get install -y --no-install-recommends pulseaudio-utils pulseaudio
pactl list sinks

cat /usr/share/sounds/alsa/*.wav | pacat

sudo apt-get install --no-install-recommends mpg321 cmus moc mp3blaster mpg123


env PULSE_SERVER=server_hostname_or_ip mplayer test.mp3

apt-get install -y --no-install-recommends \
    dbus dbus-x11 dbus-user-session python3-dbus \
    ibus ibus-gtk ibus-gtk3 ibus-gtk4 ibus-input-pad ibus-libpinyin ibus-rime \
    ibus-table-emoji ibus-table-latex ibus-table-wubi ibus-wayland
```

### Input Method (IM) Framework - Fcitx

```
$ lsblk
NAME MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda    8:0    0 363.1M  1 disk
sdb    8:16   0     4G  0 disk [SWAP]
sdc    8:32   0   256G  0 disk /mnt/wslg/distro
                               /
sdd    8:48   0     1T  0 disk /mnt/wsl/Coptwsl2scratchext4vhdx

$ cd /mnt/wsl/Coptwsl2scratchext4vhdx
$ sudo chattr -i etc/wsl.conf
```

```
https://packages.debian.org/bullseye/vainfo
https://devblogs.microsoft.com/commandline/d3d12-gpu-video-acceleration-in-the-windows-subsystem-for-linux-now-available/
sudo apt-get install -y vainfo

vainfo --display drm --device /dev/dri/card0
```

```
C:\Windows\System32\wsl.exe
C:\Users\edngsng\AppData\Local\Microsoft\WindowsApps\wsl.exe

wsl.exe -d Alpine-3.17 -u root mount --bind /home/dongsheng /mnt/wsl/Alpine/


/etc/fstab
UUID=e9999b9b-5a8e-41e1-9881-a2f766838798       /home/<username>/vhd-part1    ext4    defaults     0       2

PS C:\> New-VHD -Path $env:USERPROFILE\wsl2-shared.vhdx -Dynamic -SizeBytes 10GB

wsl -d Ubuntu-20.04 --mount --vhd $env:USERPROFILE\wsl2-shared.vhdx --bare
pwsh.exe -Command "wsl.exe -d Ubuntu-20.04 --mount --vhd C:\Users\<username>\wsl2-shared.vhdx --bare | out-null; wsl.exe -d Ubuntu-20.04"

file:///C:/var/vcs/me/user-config/lang/rust/NetBSD-Cross-Build.md
file:///C:/var/vcs/me/user-config/lang/rust/OpenBSD-Cross-Build.md
file:///C:/var/vcs/me/user-config/lang/compiler/gcc/00-summary.md
https://gcc.gnu.org/pub/gcc/snapshots/LATEST-13/

+ 安装 WSL 的内置发行版
+ 从 OCI image 安装发行版
+ DNS 配置
+ SLES 的 repo 配置
+ 在多个发行版中共享数据盘
+ systemd 与后台服务
+ 更新和定制 Linux 内核
+ WSLg，Wayland 和 Weston
+ 用户配置与 uid 1000 问题
+ 常用 GUI 文本编辑器
+ 增加与配置 Windows 或 macOS 的字体
+ 修正 sans-serif，serif 和 monospace 的字体列表
+ GTK 与 HiDPI 的配置
+ 中文输入法的安装与配置
+ IntelliJ IDEA 的配置
```

```
/dev/sda - Common Base Linux Mariner
           https://aka.ms/cbl-mariner
/dev/sdb - swap

First, realize that there are (for the sake of this discussion) two parts to WSL2:

1. The Virtual Machine Platform where the actual WSL2 virtual machine is running. To my knowledge, there's no way for you to actually see or interact with this virtual machine.

2. The WSL2 distributions that you run. These aren't virtual machines themselves, but are instead separate containers created inside individual namespaces.

Each WSL2 distribution (I call them "instances") has its own individual:

+ Users
+ Mounts
+ PID mapping
+ And more

But it also shares some resources with the parent. As with a Docker container:

+ The same kernel is being used for all WSL2 instances
+ The same memory is being used for all WSL2 instances
+ Of course, the same CPU
+ The same device tree (/dev), which includes /dev/sdb where the swap lives.
+ And, most importantly for your question, the same Swap memory itself is being used for all instances.
This swap is handled by and mounted in the parent VM that you can't access. It's reported in /proc/swaps, but that report comes from the parent VM kernel.
```
