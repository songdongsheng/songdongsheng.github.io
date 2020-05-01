---
title: FreeBSD 13 Summary
description: FreeBSD 13 Summary
date: 2021-03-07 11:45:21
tags:
    - Operating system
    - FreeBSD
categories: [Operating system, FreeBSD]
permalink: freebsd-13-summary
---

# FreeBSD 13 Summary

## FreeBSD Platforms

* https://www.freebsd.org/platforms/

Platform Name           | Platform Target       | Support Tier
------------------------|-----------------------|-------------
64-bit x86              | amd64                 | Tier 1
64-bit ARMv8            | aarch64               | Tier 1
32-bit ARMv6            | armv6                 | Tier 2
32-bit ARMv7            | armv7                 | Tier 2
32-bit MIPS hard-float  | mipshf                | Tier 2
64-bit MIPS hard-float  | mips64hf              | Tier 2
64-bit PowerPC          | powerpc64,powerpc64le | Tier 2
64-bit RISC-V           | riscv64               | Tier 2

## FreeBSD Documentation

* [FreeBSD Release Schedule](https://www.freebsd.org/releng/)
* [FreeBSD Handbook](https://docs.freebsd.org/en/books/handbook/)
* [FreeBSD Developers' Handbook](https://docs.freebsd.org/en/books/developers-handbook/)
* [FreeBSD Manual Pages](https://www.freebsd.org/cgi/man.cgi)
* [FreeBSD Committer's Guide](https://docs.freebsd.org/en/articles/committers-guide/)
* [FreeBSD Porter's Handbook](https://docs.freebsd.org/en/books/porters-handbook/)

## FreeBSD Git Infrastructure

`${repo} can be replaced by doc, ports and src. (*)`

* [FreeBSD Git repositories](https://cgit.freebsd.org/)
* https://cgit.freebsd.org/${repo}
* https://git.freebsd.org/${repo}.git

## FreeBSD Download

* https://www.freebsd.org/releng/
* https://www.freebsd.org/where/
* https://download.freebsd.org/ftp/releases/iso-images/
* https://download.freebsd.org/ftp/releases/VM-IMAGES/

```shell
$ cat << EOF | https_proxy="http://proxy.example.com:8080" aria2c -c -i -
https://download.freebsd.org/ftp/releases/iso-images/13.0/freebsd-13.0-rc5-amd64-mini-memstick.img
https://download.freebsd.org/ftp/releases/iso-images/13.0/freebsd-13.0-rc5-arm-armv6-rpi-b.img.xz
https://download.freebsd.org/ftp/releases/iso-images/13.0/freebsd-13.0-rc5-arm64-aarch64-mini-memstick.img
https://download.freebsd.org/ftp/releases/iso-images/13.0/freebsd-13.0-rc5-arm64-aarch64-rpi.img.xz
https://download.freebsd.org/ftp/releases/iso-images/13.0/freebsd-13.0-rc5-riscv-riscv64-mini-memstick.img
https://download.freebsd.org/ftp/releases/VM-IMAGES/13.0-RC5/riscv64/Latest/FreeBSD-13.0-RC5-riscv-riscv64.qcow2.xz
https://download.freebsd.org/ftp/releases/VM-IMAGES/13.0-RC5/aarch64/Latest/FreeBSD-13.0-RC5-arm64-aarch64.qcow2.xz
https://download.freebsd.org/ftp/releases/VM-IMAGES/13.0-RC5/amd64/Latest/FreeBSD-13.0-RC5-amd64.qcow2.xz
EOF
```

## FreeBSD Update

* https://www.freebsd.org/cgi/man.cgi?query=freebsd-update

```shell
# Based on the currently installed world and the configuration options set,
# fetch all available binary updates.
$ sudo freebsd-update fetch

# Install the most recently fetched updates or upgrade.
$ sudo freebsd-update install

# Fetch files necessary for upgrading to a new release.
$ sudo freebsd-update upgrade

# Install the most recently fetched updates or upgrade.
$ sudo freebsd-update install
```

## Runtime Libraries and API Changes

* Removed the [cap_random(3)](https://www.freebsd.org/cgi/man.cgi?query=cap_random) function as it has been superseded by [getrandom(2)](https://www.freebsd.org/cgi/man.cgi?query=getrandom).
* New [aio_readv(2)](https://www.freebsd.org/cgi/man.cgi?query=aio_readv) and [aio_writev(2)](https://www.freebsd.org/cgi/man.cgi?query=aio_writev) system calls provide vectored analogues of [aio_read(2)](https://www.freebsd.org/cgi/man.cgi?query=aio_read) and [aio_write(2)](https://www.freebsd.org/cgi/man.cgi?query=aio_write).
* powerpc64 switched to ELFv2 ABI at the same time it switched to LLVM. This brings us to a parity with modern Linux distributions. This also makes the binaries from previous FreeBSD versions incompatible with 13.0-RELEASE.

## Kernel Changes

* The GENERIC kernels for amd64 and i386 now include [aesni(4)](https://www.freebsd.org/cgi/man.cgi?query=aesni) to support accelerated software cryptography for [geli(8)](https://www.freebsd.org/cgi/man.cgi?query=geli) by default.
* The GENERIC kernel for aarch64 now includes [armv8crypto(4)](https://www.freebsd.org/cgi/man.cgi?query=armv8crypto) to support accelerated software cryptography for [geli(8)](https://www.freebsd.org/cgi/man.cgi?query=geli) by default. Now aarch64 supports AES-XTS and AES-GCM.
* Added Safe Memory Reclamation (SMR) to the kernel, a light weight variant of epoch reclamation closely coupled to [uma(9)](https://www.freebsd.org/cgi/man.cgi?query=uma). This has been applied in parts of the VM subsystem and VFS layer to improve scalability on high core count systems.
* The suite of VirtIO device drivers now support the VirtIO V1 spec. This improves FreeBSD’s compatibility as a guest operating system with various hypervisors and emulators including the ability to run on the [Q35 chipset](https://wiki.qemu.org/images/4/4e/Q35.pdf) under QEMU.
* For [iscsi(4)](https://www.freebsd.org/cgi/man.cgi?query=iscsi) and [ctld(8)](https://www.freebsd.org/cgi/man.cgi?query=ctld), support for specifying network QoS in the form of DiffServ Codepoints (DSCP) and Ethernet Priority Code Point (PCP) was added.
* The kernel now provides a default implementation for the SEEK_DATA and SEEK_HOLE ioctl(2)'s for filesystems which do not support sparse files.
* The NFS client and server now support NFS over TLS. The additional userland daemons are not built by default but can be enabled by building a new world that includes a KTLS-enabled OpenSSL via the WITH_OPENSSL_KTLS option.
* A new nfsv4_server_only variable can be set to YES in /etc/rc.conf to only enable support for NFSv4. This avoids the need to run [rpcbind(8)](https://www.freebsd.org/cgi/man.cgi?query=rpcbind) on an NFS server.
* The NFS client and server now support NFSv4.2 (RFC 7862) and Extended Attributes (RFC 8276).
* The ZFS implementation is now provided by OpenZFS.
* Updated the [fusefs(5)](https://www.freebsd.org/cgi/man.cgi?query=fusefs) protocol to 7.28 along with adding support for FUSE_COPY_FILE_RANGE and FUSE_LSEEK.
* The kernel now supports in-kernel framing and encryption of Transport Layer Security (TLS) data on TCP sockets for TLS versions 1.0 through 1.3. Transmit offload via in-kernel crypto drivers is supported for MtE cipher suites using AES-CBC as well as AEAD cipher suites using AES-GCM. Receive offload via in-kernel crypto drivers is supported for AES-GCM cipher suites for TLS 1.2. Using KTLS requires the use of a KTLS-aware userland SSL library. The OpenSSL library included in the base system does not enable KTLS support by default, but support can be enabled by building with the WITH_OPENSSL_KTLS option.
* [tcp(4)](https://www.freebsd.org/cgi/man.cgi?query=tcp) now supports Proportional Rate Reduction (as described by RFC6937) to improve SACK loss recovery during burst loss and ACK thinning scenarios. This feature is enabled by default.
* Merged the [ping(8)](https://www.freebsd.org/cgi/man.cgi?query=ping) and [ping6(8)](https://www.freebsd.org/cgi/man.cgi?query=ping6) utilities. [ping(8)](https://www.freebsd.org/cgi/man.cgi?query=ping) supports both IPv4 and IPv6.
* The amd64 architecture now supports **57-bit** virtual addresses (LA57) on supported CPUs. This permits user processes to use up to** 56 bits of virtual address space**. This also includes support for **five layer nested page tables** used by bhyve.
* The 64-bit ARM architecture known as arm64 or AArch64 is promoted to Tier-1 status for FreeBSD 13.
* All powerpc architectures switched to LLVM and are currently mostly similar to amd64 toolchain-wise.
* powerpc64le (64-bit little endian port) was introduced, targeting POWER8 and newer processors.
* The [bhyve(8)](https://www.freebsd.org/cgi/man.cgi?query=bhyve) utility now supports VirtIO-9p (aka VirtFS) filesystem sharing.
