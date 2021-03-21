---
title: Cross compiling Rust and Running with QEMU or Wine
description: Cross compiling Rust and Running with QEMU or Wine
date: 2021-03-20 11:16:42
tags:
    - Programming
    - Rust
categories: [Programming, Rust]
permalink: rust-cross-compiling-and-running
---

# Cross compiling Rust and Running with QEMU or Wine

## Setup Linux

```shell
sudo apt-get update && \
sudo apt-get dist-upgrade -y && \
sudo apt-get install -y --no-install-recommends \
    binutils-aarch64-linux-gnu \
    binutils-arm-linux-gnueabihf \
    binutils-mips64-linux-gnuabi64 \
    binutils-x86-64-linux-gnu \
    lld-11 \
    musl-tools \
    qemu-user-static \
    wine64
```

## Setup Rust

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

rustup self update

rustup update

rustup target add \
    aarch64-unknown-linux-musl \
    arm-unknown-linux-musleabihf \
    mips64-unknown-linux-muslabi64 \
    wasm32-wasi \
    x86_64-pc-windows-msvc \
    x86_64-unknown-linux-musl
```

## Setup WebAssembly interpreter

- [Bytecode Alliance Wasmtime](https://github.com/bytecodealliance/wasmtime/releases/latest)
- [Wasmer](https://github.com/wasmerio/wasmer/releases/latest)
- [Wasm3](https://github.com/wasm3/wasm3/releases/latest)

```shell
curl -sSL https://github.com/wasmerio/wasmer/releases/download/1.0.2/wasmer-linux-amd64.tar.gz \
    | sudo tar -xvz --strip-components=1 -C /usr/bin --wildcards */wasmer

sudo curl -sSL -o /usr/bin/wasm3 https://github.com/wasm3/wasm3/releases/download/v0.4.9/wasm3-cosmopolitan.com
sudo chmod +x /usr/bin/wasm3

curl -sSL https://github.com/bytecodealliance/wasmtime/releases/download/v0.25.0/wasmtime-v0.25.0-x86_64-linux.tar.xz \
    | sudo tar -xvJ --strip-components=1 -C /usr/bin --wildcards */wasmtime

$ wasmer --version
wasmer-headless 1.0.2

$ wasm3 --version
Wasm3 v0.4.9 on x86_64
Build: Mar 12 2021 00:17:49, GCC 9.3.0

$ wasmtime --version
wasmtime 0.25.0
```

## Hello World

```bash
cat << EOF > hello-world.rs
fn main() {
    println!("Hello, world!");
}
EOF
```

## Linux arm64

```shell
$ rustc -C opt-level=3 -C link-arg="-s" \
    --target=aarch64-unknown-linux-musl -C linker=aarch64-linux-gnu-ld -o hello-world-linux-aarch64 \
    hello-world.rs

$ du -ks hello-world-linux-aarch64
316     hello-world-linux-aarch64

$ file hello-world-linux-aarch64
hello-world-linux-aarch64: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, stripped

$ qemu-aarch64-static hello-world-linux-aarch64
Hello, world!
```

## Linux armhf

```shell
$ rustc -C opt-level=3 -C link-arg="-s" \
    --target=arm-unknown-linux-musleabihf -C linker=arm-linux-gnueabihf-ld -o hello-world-linux-armhf \
    hello-world.rs

$ du -ks hello-world-linux-armhf
304     hello-world-linux-armhf

$ file hello-world-linux-armhf
hello-world-linux-armhf: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), statically linked, stripped

$ qemu-arm-static hello-world-linux-armhf
Hello, world!
```

## Linux mips64

```shell
$ rustc -C opt-level=3 -C link-arg="-s" \
    --target=mips64-unknown-linux-muslabi64 -C linker=mips64-linux-gnuabi64-ld -o hello-world-linux-mips64 \
    hello-world.rs

$ du -ks hello-world-linux-mips64
444     hello-world-linux-mips64

$ file hello-world-linux-mips64
hello-world-linux-mips64: ELF 64-bit MSB executable, MIPS, MIPS64 rel2 version 1 (SYSV), statically linked, stripped

$ qemu-mips64-static hello-world-linux-mips64
Hello, world!
```

## Linux x86_64

### General

```shell
$ rustc -C opt-level=3 -C link-arg="-s"  \
    --target=x86_64-unknown-linux-musl -C linker="x86_64-linux-gnu-ld" -o hello-world-linux-amd64 \
    hello-world.rs

$ du -ks hello-world-linux-amd64
380     hello-world-linux-amd64

$ file hello-world-linux-amd64
hello-world-linux-amd64: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, stripped

$ qemu-x86_64-static hello-world-linux-amd64
Hello, world!
```

### ASLR

- [RFE: x86_64-unknown-linux-musl static-pie target for ASLR](https://github.com/rust-lang/rust/issues/70693)
- [Enabling static-pie for musl](https://github.com/rust-lang/rust/pull/70740)

```shell
$ cat << EOF > aslr.rs
fn main() {
    println!("main = {:#x}", &main as *const _ as usize);
}
EOF

$ rustc -C opt-level=3 -C link-arg="-s" \
    --target=x86_64-unknown-linux-musl -C linker="x86_64-linux-gnu-ld" -o aslr-linux-amd64 \
    aslr.rs

$ file aslr-linux-amd64
aslr-linux-amd64: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, stripped

$ du -ks aslr-linux-amd64
380     aslr-linux-amd64

$ ./aslr-linux-amd64
main = 0x7ff5cb823008

$ ./aslr-linux-amd64
main = 0x7fd5aa251008
```

## WebAssembly System Interface

```shell
$ rustc -C opt-level=3 -C link-arg="-s" --target=wasm32-wasi -o hello-world-wasm32-wasi hello-world.rs

$ du -ks hello-world-wasm32-wasi
76      hello-world-wasm32-wasi

$ file hello-world-wasm32-wasi
hello-world-wasm32-wasi: WebAssembly (wasm) binary module version 0x1 (MVP)

$ wasmer run hello-world-wasm32-wasi
Hello, world!

$ wasm3 hello-world-wasm32-wasi
Hello, world!

$ wasmtime run hello-world-wasm32-wasi
Hello, world!
```

## Windows x86_64

### Use DLL C runtime library

```shell
$ rustc -C opt-level=3 -C link-arg="-s" \
    --target=x86_64-pc-windows-msvc -C linker=lld-link-11 -o hello-world-windows-x86_64.exe \
    hello-world.rs

$ du -ks hello-world-windows-x86_64.exe
144     hello-world-windows-x86_64.exe

$ file hello-world-windows-x86_64.exe
hello-world-windows-x86_64.exe: PE32+ executable (console) x86-64, for MS Windows

$ wine64 hello-world-windows-x86_64.exe
Hello, world!

$ /usr/bin/x86_64-w64-mingw32-objdump -x hello-world-windows-x86_64.exe | grep -E '^\s*DLL Name'
        DLL Name: KERNEL32.dll
        DLL Name: VCRUNTIME140.dll
        DLL Name: api-ms-win-crt-runtime-l1-1-0.dll
        DLL Name: api-ms-win-crt-stdio-l1-1-0.dll
        DLL Name: api-ms-win-crt-math-l1-1-0.dll
        DLL Name: api-ms-win-crt-locale-l1-1-0.dll
        DLL Name: api-ms-win-crt-heap-l1-1-0.dll
```

### Use static C runtime library

```shell
$ rustc -C opt-level=3 -C link-arg="-s" -C "target-feature=+crt-static" \
    --target=x86_64-pc-windows-msvc -C linker=lld-link-11 -o hello-world-windows-x86_64.exe \
    hello-world.rs

$ du -ks hello-world-windows-x86_64.exe
236     hello-world-windows-x86_64.exe

$ file hello-world-windows-x86_64.exe
hello-world-windows-x86_64.exe: PE32+ executable (console) x86-64, for MS Windows

$ wine64 hello-world-windows-x86_64.exe
Hello, world!

$ /usr/bin/x86_64-w64-mingw32-objdump -x hello-world-windows-x86_64.exe | grep -E '^\s*DLL Name'
        DLL Name: KERNEL32.dll
```
