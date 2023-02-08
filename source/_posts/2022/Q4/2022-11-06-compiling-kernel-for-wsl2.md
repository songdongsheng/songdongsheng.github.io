---
title: 'Compiling the Linux Kernel for WSL2'
excerpt: 'Compiling the Linux Kernel for WSL2'
date: 2022-11-06 14:43:24
tags:
  - Operating system
  - Linux
  - Windows
categories:
  - Operating system
  - Linux
---

# Compiling the Linux Kernel for WSL2

## WSL2 architecture

WSL 2 not only loads a native Linux Kernel, the image of the Linux Kernel is in the directory `C:\Windows\System32\lxss\tools`, but it also gives us the option of **loading a customized Linux kernel**. That’s right, we can compile and customize our own kernel to be loaded into WSL 2.

![WSL2 architecture](wsl2-architecture.png)

## Preparation

### Start build container

```bash
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
sudo podman run --rm -it --pull always -h debian-testing \
    -w /root -v $(pwd):/xyz \
    -e "PATH=/usr/sbin:/usr/bin:/sbin:/bin" \
    -e NO_PROXY="localhost,::1/128,f000::/4,127.0.0.0/8,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16" \
    docker.io/library/debian:testing

echo "precedence ::ffff:0:0/96  100" >> /etc/gai.conf; \
apt-get update && apt-get dist-upgrade -y && \
apt-get install -y whiptail && \
apt-get install -y bc bison build-essential curl dwarves file flex \
    git less libelf-dev libncurses-dev libssl-dev procps \
    python3 python3-pip python3-psutil python3-virtualenv \
    vim-tiny zstd
```

### Use WSL2 Linux Kernel

```bash
# time git clone --depth 100 -b linux-msft-wsl-5.15.y https://github.com/microsoft/WSL2-Linux-Kernel.git
...
real    4m30.254s
user    3m2.417s
sys     0m26.460s

# du -ms WSL2-Linux-Kernel/
1699    WSL2-Linux-Kernel/

# cd WSL2-Linux-Kernel/ && git describe --tags
rolling-lts/wsl/5.15.68.1
linux-msft-wsl-5.15.74.2
```


### Use Stable Linux Kernel

```bash
rm -fr ~/Linux-6.1/Microsoft && mkdir -p $_ && cd $_/..
curl -sSL https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.8.tar.xz | tar --strip-components=1 -xJ -f -
curl -sSL -o Microsoft/config-wsl https://raw.githubusercontent.com/microsoft/WSL2-Linux-Kernel/linux-msft-wsl-5.15.y/Microsoft/config-wsl

# du -ms
1435    .
```

## Make Configure

We can turn on certain Linux kernel features as needed, which is also the value of compiling the kernel ourselves.

```bash
cat << EOF >> Microsoft/config-wsl

# Processor type and features/vsyscall table for legacy applications/Emulate execution only
CONFIG_LEGACY_VSYSCALL_XONLY=y

# Compile-time checks and compiler options
# BTF: .tmp_vmlinux.btf: pahole (pahole) is not available
# BTF = BPF Type Format, Use BTF in BPF rograms
# http://vger.kernel.org/~acme/perf/btf-perf-pahole-lsfmm-san-juan-2019/
# CONFIG_DEBUG_INFO_BTF is not set
# CONFIG_DEBUG_INFO_NONE=y

CONFIG_CRYPTO_ZSTD=y
CONFIG_KERNEL_ZSTD=y
CONFIG_MODULE_COMPRESS_ZSTD=y
CONFIG_SQUASHFS_ZSTD=y

# Enable the block layer/ Partition Types/Advanced partition selection
CONFIG_BSD_DISKLABEL=y

# File systems/Miscellaneous filesystems
CONFIG_ECRYPT_FS=y
CONFIG_HFSPLUS_FS=y
CONFIG_HFS_FS=y
CONFIG_UFS_FS=y
CONFIG_UFS_FS_WRITE=y

# Device Drivers/Block devices
CONFIG_BLK_DEV_NBD=y
CONFIG_BLK_DEV_RBD=y

# File systems/Network File Systems
CONFIG_NFS_V4=y
CONFIG_NFS_V4_1=y
CONFIG_NFS_V4_2=y
EOF
```

```bash
scripts/config --file Microsoft/config-wsl --disable SYSTEM_REVOCATION_KEYS
scripts/config --file Microsoft/config-wsl --disable SYSTEM_TRUSTED_KEYS

make KCONFIG_CONFIG=Microsoft/config-wsl menuconfig
```

## Make Kernel

### WSL2 Linux Kernel

```bash
# time make KCONFIG_CONFIG=Microsoft/config-wsl -j8 bzImage
...
real    21m21.836s
user    136m32.000s
sys     13m32.470s

# du -ks arch/x86/boot/bzImage
12148   arch/x86/boot/bzImage

# cp arch/x86/boot/bzImage ~/vmlinuz-5.15.74.2-WSL2-msft
# cp vmlinuz-5.15.74.2-WSL2-msft /mnt/c/Users/<seuUser>/

# du -ms .
5675    .

time make KCONFIG_CONFIG=Microsoft/config-wsl -j8 modules
time make KCONFIG_CONFIG=Microsoft/config-wsl -j8 tarxz-pkg
```

### Stable Linux Kernel

```bash
# time make KCONFIG_CONFIG=Microsoft/config-wsl -j8 bzImage
...
real    29m29.063s
user    171m39.765s
sys     19m43.272s

# du -ms .
5528    .

# du -ks arch/x86/boot/bzImage
13520   arch/x86/boot/bzImage
13960   arch/x86/boot/bzImage

# cp arch/x86/boot/bzImage ~/vmlinuz-6.1.8-WSL2
# cp vmlinuz-6.1.8-WSL2 /mnt/c/Users/<seuUser>/

time make KCONFIG_CONFIG=Microsoft/config-wsl -j8 modules
time make KCONFIG_CONFIG=Microsoft/config-wsl -j8 tarxz-pkg
```

## Update %UserProfile%\.wslconfig

+ https://learn.microsoft.com/en-us/windows/wsl/wsl-config#configure-global-options-with-wslconfig

```bash
[wsl2]
# An absolute Windows path to a custom Linux kernel
# kernel=C:\\Users\\<seuUser>\\vmlinuz-5.15.74.2-WSL2-msft
# kernel=C:\\Users\\<seuUser>\\vmlinuz-6.0.19-WSL2
# kernel=C:\\Users\\<seuUser>\\vmlinuz-6.1.8-WSL2
# 50% of total memory on Windows or 8GB, whichever is less
# memory=8GB
# Sets additional kernel parameters, in this case enabling older Linux base images such as Centos 6
# kernelCommandLine = vsyscall=emulate systemd.unified_cgroup_hierarchy=1 cgroup_no_v1=all
# kernelCommandLine = vsyscall=emulate systemd.unified_cgroup_hierarchy=1 cgroup_no_v1=named
```

## Restart WSL2

```bash
wsl --version
wsl --list --verbose
wsl --shutdown
wsl
```

## Check WSL2

### WSL2 Linux Kernel

```bash
# cat /proc/version
Linux version 5.15.74.2-microsoft-standard-WSL2+ (root@debian-testing) (gcc (Debian 12.2.0-3) 12.2.0, GNU ld (GNU Binutils for Debian) 2.39) #1 SMP Sat Nov 5 13:19:06 UTC 2022
```

### Stable Linux Kernel

```bash
# cat /proc/version
Linux version 6.1.8-microsoft-standard-WSL2 (root@debian-testing) (gcc (SUSE Linux) 11.3.0, GNU ld (GNU Binutils; SUSE Linux Enterprise 15) 2.39.0.20220810-150100.7.40) #1 SMP PREEMPT_DYNAMIC Wed Jan 25 23:17:47 CST 2023
```
