---
title: Rust installation
description: Rust installation
date: 2020-01-27 21:24:53
tags:
  - Rust
categories: [Programming, Rust]
permalink: rust-installation
---

# Install Rust

## Rust's core design principles

- Practicality — it should be a language that can be and is used in the real world.
- Pragmatism — it should admit concessions to human usability and integration into systems as they exist today.
- Memory-safety — it must enforce memory safety, and not admit segmentation faults and other such memory-access violations.
- Performance — it must be in the same performance class as C++. **favored run-time over compile-time**.
- Concurrency — it must provide modern solutions to writing concurrent code.

## Release channels and riding the trains

Rust development operates on a train schedule. That is, all development is
done on the master branch of the Rust repository. Releases follow a software
release train model, which has been used by Cisco IOS and other software
projects. There are three release channels for Rust:

- Nightly
- Beta
- Stable

Every six weeks, it’s time to prepare a new release! The beta branch of the
Rust repository branches off from the master branch used by nightly.

Six weeks after the first beta was created, it’s time for a stable release!
The stable branch is produced from the beta branch.

This is called the “train model” because every six weeks, a release “leaves
the station”.

### Rust stable channel

```bash
curl -sSLO https://static.rust-lang.org/dist/channel-rust-stable.toml.sha256
curl -sSLO https://static.rust-lang.org/dist/channel-rust-stable.toml

$ cat channel-rust-stable.toml | grep x86_64-unknown-linux-gnu.tar.xz | awk -F '"' '{print $2}'
$ cat channel-rust-stable.toml | grep x86_64-pc-windows-msvc.tar.xz | awk -F '"' '{print $2}'

while read triple rest; do
    [ -z "$triple" ] || cat channel-rust-stable.toml | grep $triple.tar.xz | awk -F '"' '{print $2}' | grep rustc-1
done << EOF
aarch64-unknown-linux-gnu
armv7-unknown-linux-gnueabihf
riscv64gc-unknown-linux-gnu
s390x-unknown-linux-gnu
x86_64-pc-windows-gnu
x86_64-unknown-linux-gnu
x86_64-unknown-linux-musl

aarch64-apple-ios
aarch64-linux-android
mips64el-unknown-linux-gnuabi64
x86_64-apple-darwin
x86_64-pc-windows-msvc
x86_64-unknown-freebsd
x86_64-unknown-netbsd
EOF

https://static.rust-lang.org/dist/2020-03-12/rustc-1.42.0-aarch64-unknown-linux-gnu.tar.xz
https://static.rust-lang.org/dist/2020-03-12/rustc-1.42.0-armv7-unknown-linux-gnueabihf.tar.xz
https://static.rust-lang.org/dist/2020-03-12/rustc-1.42.0-mips64el-unknown-linux-gnuabi64.tar.xz
https://static.rust-lang.org/dist/2020-03-12/rustc-1.42.0-s390x-unknown-linux-gnu.tar.xz
https://static.rust-lang.org/dist/2020-03-12/rustc-1.42.0-x86_64-apple-darwin.tar.xz
https://static.rust-lang.org/dist/2020-03-12/rustc-1.42.0-x86_64-pc-windows-gnu.tar.xz
https://static.rust-lang.org/dist/2020-03-12/rustc-1.42.0-x86_64-pc-windows-msvc.tar.xz
https://static.rust-lang.org/dist/2020-03-12/rustc-1.42.0-x86_64-unknown-freebsd.tar.xz
https://static.rust-lang.org/dist/2020-03-12/rustc-1.42.0-x86_64-unknown-linux-gnu.tar.xz
https://static.rust-lang.org/dist/2020-03-12/rustc-1.42.0-x86_64-unknown-linux-musl.tar.xz
https://static.rust-lang.org/dist/2020-03-12/rustc-1.42.0-x86_64-unknown-netbsd.tar.xz
```

## Using rustup

### Linux user

```bash
# curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \
    | sh -s -- --profile minimal --default-toolchain nightly

# export CARGO_HTTP_PROXY="socks5h://proxy.example.com"
# export CARGO_HOME=/opt/rust
# export CARGO_HTTP_MULTIPLEXING=true

$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

$ cargo install cargo-binutils
$ rustup component add llvm-tools-preview
$ cargo objdump -- -version
$ cargo nm --bin app --release
$ cargo nm --bin app --release -- -print-size -size-sort
$ cargo objdump --bin app --release -- -disassemble -no-show-raw-insn
$ cargo size --bin app --release -- -A -x
```

### Windows user

Windows builds of rustup additionally require an installation of [Visual
Studio 2019 or the Visual C++ Build Tools 2019](https://visualstudio.microsoft.com/downloads/). For Visual Studio, make
sure to check the "C++ tools" and "Windows 10 SDK" option

```bash
curl -sSL -o rustup-init.exe https://win.rustup.rs/
rustup-init.exe
```

## Update

```bash
rustup self update
rustup update
```

## Verify

```bash
cargo --version
rustc --version
```

## Generating a new project

```bash
cargo new hello-rust
cd hello-rust/
cargo run
```

## Working with nightly Rust

```bash
rustup toolchain install nightly
rustup run nightly rustc --version

cargo new hello-rust
cd hello-rust/
cargo +nightly run
```

## Cross-compilation

```bash
sudo apt-get install -y \
    g++ \
    g++-aarch64-linux-gnu \
    g++-arm-linux-gnueabi \
    g++-mingw-w64-x86-64-posix \
    g++-riscv64-linux-gnu \
    g++-s390x-linux-gnu \
    musl-tools \
    opensbi \
    ovmf \
    qemu-efi-aarch64 \
    qemu-efi-arm \
    qemu-system-arm \
    qemu-system-misc \
    u-boot-qemu

# https://developer.arm.com/ip-products/processors/cortex-a [a7, a9, a15, a72]
# https://developer.arm.com/ip-products/processors/cortex-r [r5, r5f]
# https://developer.arm.com/ip-products/processors/cortex-m [m3/thumbv7m-none-eabi, m4/thumbv7em-none-eabi]
# https://en.wikipedia.org/wiki/Comparison_of_ARMv7-A_cores
# https://en.wikipedia.org/wiki/Comparison_of_ARMv8-A_cores
# https://en.wikipedia.org/wiki/RISC-V
# https://docs.rust-embedded.org/faq.html

rustup target add \
    aarch64-unknown-linux-gnu \
    armv7-unknown-linux-gnueabihf \
    riscv64gc-unknown-linux-gnu \
    s390x-unknown-linux-gnu \
    x86_64-pc-windows-gnu \
    x86_64-unknown-linux-gnu \
    x86_64-unknown-linux-musl

rustup target list

rustc \
    --target=riscv64gc-unknown-linux-gnu \
    -C linker=riscv64-linux-gnu-gcc \
    main.rs

cat > ~/.cargo/config << EOF
[target.aarch64-unknown-linux-gnu]
linker = "aarch64-linux-gnu-gcc"

[target.riscv64gc-unknown-linux-gnu]
linker = "riscv64-linux-gnu-gcc"

[target.s390x-unknown-linux-gnu]
linker = "s390x-linux-gnu-gcc"

[target.x86_64-pc-windows-gnu]
linker = "x86_64-w64-mingw32-gcc"

[target.x86_64-unknown-linux-gnu]
linker = "x86_64-linux-gnu-gcc"

[target.x86_64-unknown-linux-musl]
linker = "musl-gcc"

# Cortex-A7, A9, A15
[target.armv7-unknown-linux-gnueabihf]
linker = "arm-linux-gnueabihf-gcc"

# Cortex-M3
[target.thumbv7m-none-eabi]
linker = "arm-linux-gnueabi-gcc"

# Cortex-M4 and Cortex-M7
[target.thumbv7em-none-eabi]
linker = "arm-linux-gnueabi-gcc"

# Cortex-R4 and Cortex-R5
[target.armv7r-none-eabi]
linker = "arm-linux-gnueabi-gcc"

# RV32I base instruction set with M, A and C extensions
# XuanTie 902 (RV32EMC)
# XuanTie 910 (RV64GCV)
[target.riscv32imac-unknown-none-elf]
linker = "riscv32imac-none-gcc"
EOF

cargo build --target=aarch64-unknown-linux-gnu
cargo build --target=armv7-unknown-linux-gnueabihf
cargo build --target=riscv64gc-unknown-linux-gnu
cargo build --target=s390x-unknown-linux-gnu
cargo build --target=x86_64-pc-windows-gnu
cargo build --target=x86_64-unknown-linux-gnu
cargo build --target=x86_64-unknown-linux-musl
```

## Environment variables

- RUSTUP\_HOME (default: ~/.rustup or %USERPROFILE%/.rustup) Sets the root rustup folder, used for storing installed toolchains and configuration options.
- RUSTUP\_TOOLCHAIN (default: none) If set, will override the toolchain used for all rust tool invocations. A toolchain with this name should be installed, or invocations will fail.
- RUSTUP\_DIST\_SERVER (default: https://static.rust-lang.org) Sets the root URL for downloading static resources related to Rust. You can change this to instead use a local mirror, or to test the binaries from the staging directory.
- RUSTUP\_DIST\_ROOT (default: https://static.rust-lang.org/dist) Deprecated. Use RUSTUP\_DIST\_SERVER instead.
- RUSTUP\_UPDATE\_ROOT (default https://static.rust-lang.org/rustup) Sets the root URL for downloading self-updates.
- RUSTUP\_IO\_THREADS unstable (defaults to reported cpu count). Sets the number of threads to perform close IO in. Set to disabled to force single-threaded IO for troubleshooting, or an arbitrary number to override automatic detection.
- RUSTUP\_TRACE\_DIR unstable (default: no tracing) Enables tracing and determines the directory that traces will be written too. Traces are of the form PID.trace. Traces can be read by the Catapult project tracing viewer.
- RUSTUP\_UNPACK\_RAM unstable (default 400M, min 100M) Caps the amount of RAM rustup will use for IO tasks while unpacking.
- RUSTUP\_NO\_BACKTRACE Disables backtraces on non-panic errors even when RUST\_BACKTRACE is set.

## Working with network proxies

```bash
export https_proxy=socks5://proxy.example.com:1080
export https_proxy=http://proxy.example.com:8080
```
