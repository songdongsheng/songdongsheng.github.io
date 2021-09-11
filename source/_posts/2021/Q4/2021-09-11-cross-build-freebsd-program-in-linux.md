---
title: Cross-build FreeBSD Program in Linux
description: Cross-build FreeBSD Program in Linux
date: 2021-09-11 11:28:47
tags:
  - Operating system
  - FreeBSD
categories:
  - Operating system
  - FreeBSD
permalink: cross-build-freebsd-program-in-linux
---

# Rust on FreeBSD

## The First Steps on FreeBSD

### hostname

```bash
root@freebsd-13:~ # vi /etc/rc.conf
    hostname="freebsd-13"
```

### hvn0

```bash
root@freebsd-13:~ # vi /etc/rc.conf

    #defaultrouter="192.168.1.1"
    #ifconfig_hn0="inet 192.168.1.130/24"

    ifconfig_hn0="DHCP"
    ifconfig_hn0_ipv6="inet6 accept_rtadv"


root@freebsd-13:~ # dmesg | grep hn0
    hn0: <Hyper-V Network Interface> on vmbus0
    hn0: Ethernet address: 5c:a4:fe:98:5f:41
    hn0: link state changed to UP
```

### disk

```bash
root@freebsd-13:~ # df -h
Filesystem            Size    Used   Avail Capacity  Mounted on
zroot/ROOT/default     20G    3.1G     17G    16%    /
devfs                 1.0K    1.0K      0B   100%    /dev
/dev/da0p1            260M    1.8M    258M     1%    /boot/efi
zroot/tmp              17G    8.8M     17G     0%    /tmp
zroot/var/log          17G    180K     17G     0%    /var/log
zroot                  17G     96K     17G     0%    /zroot
zroot/var/audit        17G     96K     17G     0%    /var/audit
zroot/usr/ports        18G    711M     17G     4%    /usr/ports
zroot/var/tmp          17G     96K     17G     0%    /var/tmp
zroot/usr/src          17G     96K     17G     0%    /usr/src
zroot/var/mail         17G    120K     17G     0%    /var/mail
zroot/var/crash        17G     96K     17G     0%    /var/crash
zroot/usr/home         17G     83M     17G     0%    /usr/home
```

```bash
root@freebsd-13:~ # mount
zroot/ROOT/default on / (zfs, NFS exported, local, noatime, nfsv4acls)
devfs on /dev (devfs)
/dev/da0p1 on /boot/efi (msdosfs, local)
zroot/tmp on /tmp (zfs, local, noatime, nosuid, nfsv4acls)
zroot/var/log on /var/log (zfs, local, noatime, noexec, nosuid, nfsv4acls)
zroot on /zroot (zfs, local, noatime, nfsv4acls)
zroot/var/audit on /var/audit (zfs, local, noatime, noexec, nosuid, nfsv4acls)
zroot/usr/ports on /usr/ports (zfs, local, noatime, nosuid, nfsv4acls)
zroot/var/tmp on /var/tmp (zfs, local, noatime, nosuid, nfsv4acls)
zroot/usr/src on /usr/src (zfs, local, noatime, nfsv4acls)
zroot/var/mail on /var/mail (zfs, local, nfsv4acls)
zroot/var/crash on /var/crash (zfs, local, noatime, noexec, nosuid, nfsv4acls)
zroot/usr/home on /usr/home (zfs, local, noatime, nfsv4acls)
```

### rc.conf

```bash
root@freebsd-13:~ # vi /etc/rc.conf
hostname="freebsd-13"

# defaultrouter="192.168.1.1"
# ifconfig_hn0="inet 192.168.1.130/24"

ifconfig_hn0="DHCP"
ifconfig_hn0_ipv6="inet6 accept_rtadv"

sshd_enable="YES"
ntpdate_enable="YES"
# Set dumpdev to "AUTO" to enable crash dumps, "NO" to disable
dumpdev="NO"
zfs_enable="YES"

sendmail_enable="NO"
sendmail_submit_enable="NO"
sendmail_outbound_enable="NO"
sendmail_msp_queue_enable="NO"

nfs_server_enable="YES"
nfsv4_server_only="YES"
nfsv4_server_enable="YES"
mountd_enable="YES"
nfs_client_enable="YES"

dbus_enable="YES"
avahi_daemon_enable="YES"

linux_enable="YES"
linux_mounts_enable="YES"
```

### sshd

```bash
root@freebsd-13:~ # touch $HOME/.hushlogin

root@freebsd-13:~ # vi /etc/ssh/sshd_config
    PermitRootLogin yes
    PasswordAuthentication yes
    UseDNS no

root@freebsd-13:~ # /etc/rc.d/sshd reload
Performing sanity check on sshd configuration.
```

### pkg

```bash
root@freebsd-13:~ # vi ~/.profile
# export PS1="\u@\h:\w \\$ "

. "$HOME/.cargo/env"
```

```bash
root@freebsd-13:~ # pkg update
Updating FreeBSD repository catalogue...
Fetching packagesite.txz: 100%    6 MiB   1.1MB/s    00:06
Processing entries: 100%
FreeBSD repository update completed. 30745 packages processed.
All repositories are up to date.

root@freebsd-13:~ # pkg info

root@freebsd-13:~ # pkg upgrade -y
```

### cc

```bash
root@freebsd-13:~ # cat << EOF | cc -std=c11 -march=haswell -O2 -s -o hello -x c -
#include <stdio.h>
int main()
{
    printf("Hello, World!\n");
    return 0;
}
EOF

root@freebsd-13:~ # du -ks hello
5       hello

root@freebsd-13:~ # file hello
hello: ELF 64-bit LSB executable, x86-64, version 1 (FreeBSD), dynamically linked, interpreter /libexec/ld-elf.so.1, for FreeBSD 13.0 (1300139), FreeBSD-style, stripped

root@freebsd-13:~ # readelf -hld hello | grep -E "NEEDED|interpreter"
      [Requesting program interpreter: /libexec/ld-elf.so.1]
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.7]

root@freebsd-13:~ # ldd hello
hello:
        libc.so.7 => /lib/libc.so.7 (0x800245000)
```

### c++

```bash
root@freebsd-13:~ # cat << EOF | c++ -std=c++17 -march=haswell -O2 -s -o hello.c++ -x c++ -
#include <iostream>
int main()
{
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
EOF

root@freebsd-13:~ # du -ks hello.c++
9       hello.c++

root@freebsd-13:~ # file hello.c++
hello.c++: ELF 64-bit LSB executable, x86-64, version 1 (FreeBSD), dynamically linked, interpreter /libexec/ld-elf.so.1, for FreeBSD 13.0 (1300139), FreeBSD-style, stripped

root@freebsd-13:~ # readelf -hld hello.c++  | grep -E "NEEDED|interpreter"
      [Requesting program interpreter: /libexec/ld-elf.so.1]
 0x0000000000000001 (NEEDED)             Shared library: [libc++.so.1]
 0x0000000000000001 (NEEDED)             Shared library: [libcxxrt.so.1]
 0x0000000000000001 (NEEDED)             Shared library: [libm.so.5]
 0x0000000000000001 (NEEDED)             Shared library: [libgcc_s.so.1]
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.7]

root@freebsd-13:~ # ldd hello.c++
hello.c++:
        libc++.so.1 => /usr/lib/libc++.so.1 (0x800246000)
        libcxxrt.so.1 => /lib/libcxxrt.so.1 (0x800318000)
        libm.so.5 => /lib/libm.so.5 (0x80033b000)
        libgcc_s.so.1 => /lib/libgcc_s.so.1 (0x80036e000)
        libc.so.7 => /lib/libc.so.7 (0x800387000)
```

## Copy FreeBSD Library Files to Linux

We can copy all files under **/lib** and **/usr/lib**:

```bash
root@freebsd-13:~ # du -ms /lib /usr/lib /usr/include
9       /lib
208     /usr/lib
16      /usr/include
```

or a few of them:

```bash
root@linux:/opt $ du -ms x86_64-unknown-freebsd/
31      x86_64-unknown-freebsd/
3       x86_64-unknown-freebsd/lib
2       x86_64-unknown-freebsd/usr/lib
27      x86_64-unknown-freebsd/usr/include

root@linux:/opt/x86_64-unknown-freebsd $ find . ! -type d | xargs du -ks
1936    ./lib/libc.so.7
128     ./lib/libcxxrt.so.1
128     ./lib/libgcc_s.so.1
384     ./lib/libm.so.5
256     ./lib/libthr.so.3
192     ./lib/libutil.so.9
26860   ./usr/include
16      ./usr/lib/Scrt1.o
16      ./usr/lib/crt1.o
8       ./usr/lib/crtbegin.o
8       ./usr/lib/crtbeginS.o
4       ./usr/lib/crtend.o
4       ./usr/lib/crtendS.o
4       ./usr/lib/crti.o
4       ./usr/lib/crtn.o
896     ./usr/lib/libc++.so.1
0       ./usr/lib/libc.so
48      ./usr/lib/libc_nonshared.a
0       ./usr/lib/libexecinfo.so
16      ./usr/lib/libexecinfo.so.1
0       ./usr/lib/libgcc_s.so
0       ./usr/lib/libm.so
640     ./usr/lib/libprocstat.a
0       ./usr/lib/libprocstat.so
64      ./usr/lib/libprocstat.so.1
0       ./usr/lib/libpthread.so
0       ./usr/lib/librt.so
48      ./usr/lib/librt.so.1
0       ./usr/lib/libthr.so
0       ./usr/lib/libutil.so
```

### Archive Library Files on FreeBSD

```bash
root@freebsd-13:~ # tar --no-acls --no-xattrs --no-fflags -cvzf x86_64-unknown-freebsd_13.0-RELEASE-p4-rust.tar.gz \
   /lib/libc.so.7 \
   /lib/libcxxrt.so.1 \
   /lib/libgcc_s.so.1 \
   /lib/libm.so.5 \
   /lib/libthr.so.3 \
   /lib/libutil.so.9 \
   /usr/lib/Scrt1.o \
   /usr/lib/crt1.o \
   /usr/lib/crtbegin.o \
   /usr/lib/crtbeginS.o \
   /usr/lib/crtend.o \
   /usr/lib/crtendS.o \
   /usr/lib/crti.o \
   /usr/lib/crtn.o \
   /usr/lib/libc++.so.1 \
   /usr/lib/libc.so \
   /usr/lib/libc_nonshared.a \
   /usr/lib/libexecinfo.so \
   /usr/lib/libexecinfo.so.1 \
   /usr/lib/libgcc_s.so \
   /usr/lib/libm.so \
   /usr/lib/libprocstat.a \
   /usr/lib/libprocstat.so \
   /usr/lib/libprocstat.so.1 \
   /usr/lib/libpthread.so \
   /usr/lib/librt.so \
   /usr/lib/librt.so.1 \
   /usr/lib/libthr.so \
   /usr/lib/libutil.so
```

### Extract FreeBSD Library Files on Linux

```bash
root@linux:~ $ mkdir -p /opt/x86_64-unknown-freebsd
root@linux:~ ssh root@freebsd-13.local 'cat x86_64-unknown-freebsd_13.0-RELEASE-p4-rust.tar.gz' | tar -xvz -C /opt/x86_64-unknown-freebsd
```

## Install clang and lld on Linux

```bash
sudo apt-get install -y clang lld
```

## Install Rust on Linux

```bash
root@linux:~ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

root@linux:~ . "$HOME/.cargo/env"

root@linux:~ $ cargo --verbose --version
cargo 1.55.0 (32da73ab1 2021-08-23)
release: 1.55.0
commit-hash: 32da73ab19417aa89686e1d85c1440b72fdf877d
commit-date: 2021-08-23

root@linux:~ $ rustc --version
rustc 1.55.0 (c8dfcfe04 2021-09-06)
```

## Cargo’s Configuration on Linux

Cargo allows local configuration for a particular package as well as global configuration. It looks for configuration files in the **current directory** and **all parent directories**.

+ .cargo/config.toml
+ (**all parent directories**)/.cargo/config.toml
+ $CARGO_HOME/config.toml which defaults to:
  + Windows: %USERPROFILE%\.cargo\config.toml
  + Unix: $HOME/.cargo/config.toml, ${CARGO_HOME:-${HOME}/.cargo}/config.toml

Now we can make rust target **x86_64-unknown-FreeBSD** use directory **/opt/x86_64-unknown-FreeBSD** as **sysroot**:

```toml
[http]
ssl-version = "tlsv1.3"

[profile.release]
lto = "fat"
opt-level = "s"
codegen-units = 1

[target.'cfg(target_arch = "x86_64")']
rustflags = ["-Ctarget-cpu=x86-64-v3"]

[target.'cfg(any(target_family = "windows", target_env = "msvc", target_env = "musl")']
rustflags = ["-Ctarget-feature=+crt-static"]

[target.x86_64-unknown-freebsd]
    # 13.0-RELEASE-p4, FreeBSD clang version 11.0.1
    linker="clang"
    rustflags = ["-Clink-arg=-s", "-Clink-arg=-v", "-Clink-arg=-fuse-ld=lld", "-Clink-arg=--target=x86_64-unknown-freebsd",
"-Clink-arg=--sysroot=/opt/x86_64-unknown-freebsd"]
```

## Cross-build FreeBSD Program in Linux

```bash
root@linux:~ uname -msr
Linux 5.10.0-8-amd64 x86_64

root@linux:~ cargo build -v --release --target x86_64-unknown-freebsd
...
    Finished release [optimized] target(s) in 8.67s

root@linux:~ $ du -ks target/x86_64-unknown-freebsd/release/hello
236     target/x86_64-unknown-freebsd/release/hello

root@linux:~ $ file target/x86_64-unknown-freebsd/release/hello
target/x86_64-unknown-freebsd/release/hello: ELF 64-bit LSB pie executable, x86-64, version 1 (FreeBSD), dynamically linked, interpreter /libexec/ld-elf.so.1, FreeBSD-style, stripped

root@linux:~ $ readelf -ld target/x86_64-unknown-freebsd/release/hello | grep -E "interpreter:|NEEDED"
      [Requesting program interpreter: /libexec/ld-elf.so.1]
 0x0000000000000001 (NEEDED)             Shared library: [libthr.so.3]
 0x0000000000000001 (NEEDED)             Shared library: [libgcc_s.so.1]
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.7]

root@linux:~ $ scp target/x86_64-unknown-freebsd/release/hello root@freebsd-13.local:/tmp/

root@linux:~ $ ssh root@freebsd-13.local /tmp/hello
Hello, world!

root@linux:~ $ ssh root@freebsd-13.local uname -msr
FreeBSD 13.0-RELEASE-p4 amd64
```
