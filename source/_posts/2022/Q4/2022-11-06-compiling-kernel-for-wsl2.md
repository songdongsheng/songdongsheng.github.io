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

sudo podman run --rm -it --pull always -h debian-stable \
    -w /root -v $(pwd):/xyz --network=host \
    -e "PATH=/usr/sbin:/usr/bin:/sbin:/bin" \
    -e NO_PROXY="localhost,::1/128,f000::/4,127.0.0.0/8,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16" \
    public.ecr.aws/docker/library/debian:stable

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
# time git clone --depth 100 -b linux-msft-wsl-6.6.y https://github.com/microsoft/WSL2-Linux-Kernel.git
...
real    2m55.718s
user    1m36.683s
sys     0m16.305s

# du -ms WSL2-Linux-Kernel/
2041    WSL2-Linux-Kernel/

# cd WSL2-Linux-Kernel/ && git describe --tags
linux-msft-wsl-6.6.36.3
```


### Use Stable Linux Kernel

```bash
rm -fr ~/Linux-6.x/Microsoft && mkdir -p $_ && cd $_/..
curl -sSL https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.9.10.tar.xz | tar --strip-components=1 -xJ -f -
curl -sSL -o arch/x86/configs/config-wsl https://raw.githubusercontent.com/microsoft/WSL2-Linux-Kernel/linux-msft-wsl-6.6.y/arch/x86/configs/config-wsl

# du -ms
1570    .
```

## Make Configure

We can turn on certain Linux kernel features as needed, which is also the value of compiling the kernel ourselves.

```bash
cat << EOF >> arch/x86/configs/config-wsl

# Processor type and features/vsyscall table for legacy applications/Emulate execution only
CONFIG_LEGACY_VSYSCALL_XONLY=y

# Compile-time checks and compiler options
# BTF: .tmp_vmlinux.btf: pahole (pahole) is not available
# BTF = BPF Type Format, Use BTF in BPF rograms
# http://vger.kernel.org/~acme/perf/btf-perf-pahole-lsfmm-san-juan-2019/
# CONFIG_DEBUG_INFO_BTF=y
# CONFIG_DEBUG_INFO_BTF_MODULES=y

CONFIG_PREEMPT_DYNAMIC=y
CONFIG_PREEMPT_RCU=y

CONFIG_KVM=y
CONFIG_KVM_INTEL=y
CONFIG_KVM_AMD=y

CONFIG_TLS=y
CONFIG_IP_SCTP=y

CONFIG_CRYPTO_ZSTD=y
CONFIG_KERNEL_ZSTD=y
CONFIG_MODULE_COMPRESS_ZSTD=y
CONFIG_SQUASHFS_ZSTD=y

# Enable the block layer/ Partition Types/Advanced partition selection
CONFIG_BSD_DISKLABEL=y

# Device Drivers/Block devices
CONFIG_ATA_OVER_ETH=y
CONFIG_BLK_DEV_NBD=y
CONFIG_BLK_DEV_RBD=y
CONFIG_BLK_DEV_UBLK=y
CONFIG_ZRAM=y

# File systems/Miscellaneous filesystems
CONFIG_BTRFS_FS=y
CONFIG_ECRYPT_FS=y
CONFIG_FUSE_FS=y
CONFIG_HFS_FS=y
CONFIG_HFSPLUS_FS=y
CONFIG_UFS_FS=y
CONFIG_UFS_FS_WRITE=y

# File systems/Network File Systems
CONFIG_CIFS=y
CONFIG_NFS_DISABLE_UDP_SUPPORT=y
CONFIG_NFS_V4_2=y
CONFIG_NFS_V4_2_READ_PLUS=y

CONFIG_SUNRPC=y
CONFIG_SUNRPC_BACKCHANNEL=y
CONFIG_SUNRPC_GSS=y
CONFIG_RPCSEC_GSS_KRB5=y
CONFIG_RPCSEC_GSS_KRB5_ENCTYPES_AES_SHA2=y
EOF
```

```bash
scripts/config --file arch/x86/configs/config-wsl --disable SYSTEM_REVOCATION_KEYS; \
scripts/config --file arch/x86/configs/config-wsl --disable SYSTEM_TRUSTED_KEYRING

make KCONFIG_CONFIG=arch/x86/configs/config-wsl menuconfig
```

## Make Kernel

### WSL2 Linux Kernel

```bash
# time make KCONFIG_CONFIG=arch/x86/configs/config-wsl -j8 bzImage
...
real    19m6.172s
user    135m7.511s
sys     17m11.616s

# du -ks arch/x86/boot/bzImage
15620   arch/x86/boot/bzImage

# du -ms .
6024    .

# cp arch/x86/boot/bzImage /mnt/c/Users/<seuUser>/vmlinuz-6.6.36.3-WSL2
# cp arch/x86/configs/config-wsl /mnt/c/Users/<seuUser>/vmlinuz-6.6.36.3-WSL2.config
# vi /mnt/c/Users/<seuUser>/.wslconfig

time make KCONFIG_CONFIG=arch/x86/configs/config-wsl -j8 modules
time make KCONFIG_CONFIG=arch/x86/configs/config-wsl -j8 tarxz-pkg
```

### Stable Linux Kernel

```bash
# time make KCONFIG_CONFIG=arch/x86/configs/config-wsl -j8 bzImage
...
Kernel: arch/x86/boot/bzImage is ready  (#1)

real    19m57.919s
user    142m41.987s
sys     12m48.108s

# du -ms .
4467    .

# du -ks arch/x86/boot/bzImage
13992   arch/x86/boot/bzImage

# cp arch/x86/boot/bzImage /mnt/c/Users/<seuUser>/vmlinuz-6.9.10-WSL2
# cp arch/x86/configs/config-wsl  /mnt/c/Users/<seuUser>/vmlinuz-6.9.10-WSL2.config
# vi /mnt/c/Users/<seuUser>/.wslconfig

time make KCONFIG_CONFIG=arch/x86/configs/config-wsl -j8 modules
time make KCONFIG_CONFIG=arch/x86/configs/config-wsl -j8 tarxz-pkg
```

## Update %UserProfile%\.wslconfig

+ https://learn.microsoft.com/en-us/windows/wsl/wsl-config#configure-global-options-with-wslconfig

```bash
[wsl2]
# An absolute Windows path to a custom Linux kernel
# kernel=C:\\Users\\<seuUser>\\vmlinuz-6.6.36.3-WSL2
# kernel=C:\\Users\\<seuUser>\\vmlinuz-6.9.10-WSL2
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

# taskkill /F /T /IM wslservice.exe
```

```bash
wsl
```

## Check WSL2

### WSL2 Linux Kernel

```bash
# cat /proc/version
Linux version 6.6.36.3-microsoft-standard-WSL2+ (root@debian-stable) (gcc (Debian 12.2.0-14) 12.2.0, GNU ld (GNU Binutils for Debian) 2.40) #1 SMP PREEMPT_DYNAMIC Fri Jul 19 07:04:59 UTC 2024
```

### Stable Linux Kernel

```bash
# cat /proc/version
Linux version 6.9.10-microsoft-standard-WSL2 (root@debian-stable) (gcc (Debian 12.2.0-14) 12.2.0, GNU ld (GNU Binutils for Debian) 2.40) #1 SMP PREEMPT_DYNAMIC Fri Jul 19 02:25:29 UTC 2024
```
