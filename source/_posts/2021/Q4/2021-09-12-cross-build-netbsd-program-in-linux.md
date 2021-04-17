---
title: Cross-build NetBSD Program in Linux
description: Cross-build NetBSD Program in Linux
date: 2021-09-12 23:16:33
tags:
  - Operating system
  - NetBSD
categories:
  - Operating system
  - NetBSD
permalink: cross-build-netbsd-program-in-linux
---

# Rust on NetBSD

## The First Steps on NetBSD

### hostname

```bash
root@netbsd-9:~ vi /etc/myname
    netbsd-9
```

### hvn0

```bash
root@netbsd-9:~ dmesg | grep hvn
[     1.299320] hvn0 at vmbus0: Hyper-V NetVSC
[     1.299320] hvn0: NVS 5.0 NDIS 6.30
[     1.299320] hvn0: Ethernet address 60:33:d9:6d:64:12

root@netbsd-9:~ vi /etc/ifconfig.hvn0
up
media manual
```

### disk

```bash
root@netbsd-9:~ df -h
Filesystem         Size       Used      Avail %Cap Mounted on
/dev/dk1            60G       2.2G        55G   3% /
tmpfs              512M       244K       511M   0% /tmp
kernfs             1.0K       1.0K         0B 100% /kern
ptyfs              1.0K       1.0K         0B 100% /dev/pts
procfs             4.0K       4.0K         0B 100% /proc
tmpfs              512M       4.0K       512M   0% /var/shm
```

```bash
root@netbsd-9:~ cat /proc/mounts
/dev/dk1 / ffs rw 0 0
tmpfs /tmp tmpfs rw 0 0
kernfs /kern kernfs rw 0 0
ptyfs /dev/pts ptyfs rw 0 0
procfs /proc proc rw 0 0
tmpfs /var/shm tmpfs rw 0 0
```

### rc.conf

```bash
root@netbsd-9:/etc vi /etc/rc.conf
# Add local overrides below.
#
dhcpcd=YES
dhcpcd_flags="-qM hvn0"
sshd=YES
ntpd=YES
ntpdate=YES
mdnsd=YES
wscons=YES
```

### sshd

```bash
root@netbsd-9:~ vi /etc/sshd/ssh_config
    PermitRootLogin yes
    PasswordAuthentication yes
    UseDNS no

root@netbsd-9:~ /etc/rc.d/sshd reload
```

### repositories.conf

```bash
root@netbsd-9:~ cat << EOF > /usr/pkg/etc/pkgin/repositories.conf
https://cdn.NetBSD.org/pub/pkgsrc/packages/NetBSD/x86_64/9.2/All/
https://cdn.NetBSD.org/pub/pkgsrc/packages/NetBSD/x86_64/9.1/All/
https://cdn.NetBSD.org/pub/pkgsrc/packages/NetBSD/x86_64/9.0/All/
EOF
```

### pkg

```bash
root@netbsd-9:~ vi ~/.profile
# export PS1="\u@\h:\w \\$ "
export PS1=`whoami`@`hostname -s`:${PWD}" $ "

export PKG_PATH=https://cdn.NetBSD.org/pub/pkgsrc/packages/NetBSD/x86_64/9.2/All
export PKG_PATH="${PKG_PATH};https://cdn.NetBSD.org/pub/pkgsrc/packages/NetBSD/x86_64/9.2/All"
export PKG_PATH="${PKG_PATH};https://cdn.NetBSD.org/pub/pkgsrc/packages/NetBSD/x86_64/9.1/All"
export PKG_PATH="${PKG_PATH};https://cdn.NetBSD.org/pub/pkgsrc/packages/NetBSD/x86_64/9.0/All"

. "$HOME/.cargo/env"
```

```bash
pkg_admin fetch-pkg-vulnerabilities
pkg_add -uv tmux pkgin curl ca-certificates

pkg_info tmux

pkg_delete -r jpeg

pkgin update
```

## Copy NetBSD Library Files to Linux

We can copy all files under **/lib** and **/usr/lib**:

```bash
root@netbsd-9:/ du -ms /lib /usr/lib /usr/include
10      /lib
287     /usr/lib
25      /usr/include
```

or a few of them:

```bash
root@linux:/opt du -ms x86_64-unknown-netbsd
8       x86_64-unknown-netbsd

root@linux:/opt/x86_64-unknown-netbsd $ find . ! -type d | xargs du -ks
0       ./lib/libc.so
2032    ./lib/libc.so.12.213
0       ./lib/libgcc_s.so
96      ./lib/libgcc_s.so.1.0
0       ./lib/libm.so
204     ./lib/libm.so.0.12
0       ./lib/libpthread.so
92      ./lib/libpthread.so.1.4
0       ./lib/libutil.so
124     ./lib/libutil.so.7.24
4       ./usr/lib/crt0.o
4       ./usr/lib/crtbegin.o
4       ./usr/lib/crtbeginS.o
0       ./usr/lib/crtbeginT.o
4       ./usr/lib/crtend.o
0       ./usr/lib/crtendS.o
4       ./usr/lib/crti.o
4       ./usr/lib/crtn.o
8       ./usr/lib/gcrt0.o
4648    ./usr/lib/libc.a
96      ./usr/lib/libpthread.a
32      ./usr/lib/librt.a
0       ./usr/lib/librt.so
16      ./usr/lib/librt.so.1.1
224     ./usr/lib/libutil.a
```

### Archive Library Files on NetBSD

```bash
root@netbsd-9:~ tar --no-acls --no-xattrs -cvzf x86_64-unknown-netbsd_9.2-STABLE-rust.tar.gz \
    /lib/libc.so \
    /lib/libc.so.12.213 \
    /lib/libgcc_s.so \
    /lib/libgcc_s.so.1.0 \
    /lib/libm.so \
    /lib/libm.so.0.12 \
    /lib/libpthread.so \
    /lib/libpthread.so.1.4 \
    /lib/libutil.so \
    /lib/libutil.so.7.24 \
    /usr/lib/crt0.o \
    /usr/lib/crtbegin.o \
    /usr/lib/crtbeginS.o \
    /usr/lib/crtbeginT.o \
    /usr/lib/crtend.o \
    /usr/lib/crtendS.o \
    /usr/lib/crti.o \
    /usr/lib/crtn.o \
    /usr/lib/gcrt0.o \
    /usr/lib/libc.a \
    /usr/lib/libpthread.a \
    /usr/lib/librt.a \
    /usr/lib/librt.so \
    /usr/lib/librt.so.1.1 \
    /usr/lib/libutil.a
```

### Extract NetBSD Library Files on Linux

```bash
root@linux:~ mkdir -p /opt/x86_64-unknown-netbsd
root@linux:~ ssh root@netbsd-9.local 'cat x86_64-unknown-netbsd_9.2-STABLE-rust.tar.gz' | tar -xvz -C /opt/x86_64-unknown-netbsd
```

## Install clang and lld on Linux

```bash
sudo apt-get install -y clang lld
```

## Install Rust on Linux

```bash
root@linux:~ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

root@linux:~ . "$HOME/.cargo/env"

root@linux:~ cargo --verbose --version
cargo 1.55.0 (32da73ab1 2021-08-23)
release: 1.55.0
commit-hash: 32da73ab19417aa89686e1d85c1440b72fdf877d
commit-date: 2021-08-23

root@linux:~ rustc --version
rustc 1.55.0 (c8dfcfe04 2021-09-06)
```

## Cargo’s Configuration on Linux

Cargo allows local configuration for a particular package as well as global configuration. It looks for configuration files in the **current directory** and **all parent directories**.

+ .cargo/config.toml
+ (**all parent directories**)/.cargo/config.toml
+ $CARGO_HOME/config.toml which defaults to:
  + Windows: %USERPROFILE%\.cargo\config.toml
  + Unix: $HOME/.cargo/config.toml

Now we can make rust target **x86_64-unknown-netbsd** use directory **/opt/x86_64-unknown-netbsd** as **sysroot**:

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

[target.x86_64-unknown-netbsd]
    # cargo build -v --release --target x86_64-unknown-netbsd
    # NetBSD 9.2_STABLE amd64, gcc 7.5.0
    linker="clang"
    rustflags = ["-Clink-arg=-s", "-Clink-arg=-v", "-Clink-arg=-fuse-ld=lld", "-Clink-arg=--target=x86_64-unknown-netbsd",
"-Clink-arg=--sysroot=/opt/x86_64-unknown-netbsd", "-Clink-arg=-L/opt/x86_64-unknown-netbsd/lib", "-Clink-arg=-L/opt/x86_64-unknown-netbsd/usr/lib"]
```

## Cross-build NetBSD Program in Linux

```bash
root@linux:~ uname -msr
Linux 5.10.0-8-amd64 x86_64

root@linux:~ cargo build -v --release --target x86_64-unknown-netbsd
...
    Finished release [optimized] target(s) in 11.59s

root@linux:~ du -ks target/x86_64-unknown-netbsd/release/hello
240     target/x86_64-unknown-netbsd/release/hello

root@linux:~ file target/x86_64-unknown-netbsd/release/hello
target/x86_64-unknown-netbsd/release/hello: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /libexec/ld.elf_so, for NetBSD 9.2, stripped

root@linux:~ readelf -ld target/x86_64-unknown-netbsd/release/hello | grep -E "interpreter:|NEEDED"
      [Requesting program interpreter: /libexec/ld.elf_so]
 0x0000000000000001 (NEEDED)             Shared library: [libpthread.so.1]
 0x0000000000000001 (NEEDED)             Shared library: [libgcc_s.so.1]
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.12]

root@linux:~ scp target/x86_64-unknown-netbsd/release/hello root@netbsd-9.local:/tmp/

root@linux:~ ssh root@netbsd-9.local /tmp/hello
Hello, world!

root@linux:~ ssh root@netbsd-9.local uname -msr
NetBSD 9.2_STABLE amd64
```
