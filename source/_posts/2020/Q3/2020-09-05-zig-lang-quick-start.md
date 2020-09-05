---
title: Zig Programming Language Quick Start
description: Zig Programming Language Quick Start
date: 2020-09-05 21:49:41
tags:
    - Programming
    - Zig
    - Linux
    - Windows
categories: [Programming, Zig]
permalink: zig-lang-quick-start
---

# Zig Programming Language Quick Start

## Introduction

Zig is a general-purpose programming language and toolchain for maintaining **robust**, **optimal**, and **reusable** software.

* **Robust** - behavior is correct even for edge cases such as out of memory.
* **Optimal** - write programs the best way they can behave and perform.
* **Reusable** - the same code works in many environments which have different constraints.
* **Maintainable** - precisely communicate intent to the compiler and other programmers. The language imposes a low overhead to reading code and is resilient to changing requirements and environments.

## Support Table

### WebAssembly Support
| &nbsp;    | free standing | emscripten    | WASI |
|-----------|---------------|---------------|------|
| wasm32    | Tier 2        | Tier 3        | Tier 2
| wasm64    | Tier 4        | Tier 4        | Tier 4

### Tier System

Zig uses a Tier System to communicate the level of support for different targets.

| &nbsp;    | free standing | Linux 3.16+   | macOS 10.13+  | Windows 8.1+  | FreeBSD 12.0+ | NetBSD 8.0+ |
|-----------|---------------|---------------|---------------|---------------|---------------|-------------|
| x86_64    | Tier 1        | Tier 1        | Tier 2        | Tier 2        | Tier 2        | Tier 2      |
| arm64     | Tier 1        | Tier 2        | N/A           | Tier 3        | Tier 3        | Tier 3      |
| arm32     | Tier 1        | Tier 2        | N/A           | Tier 3        | Tier 3        | Tier 3      |
| mips32 LE | Tier 1        | Tier 2        | N/A           | N/A           | Tier 3        | Tier 3      |
| mips64    | Tier 3        | Tier 3        | N/A           | N/A           | Tier 3        | Tier 3      |
| riscv64   | Tier 1        | Tier 2        | N/A           | N/A           | Tier 3        | Tier 3      |
| bpf       | Tier 3        | Tier 3        | N/A           | N/A           | Tier 3        | Tier 3      |
| s390x     | Tier 3        | Tier 3        | N/A           | N/A           | Tier 3        | Tier 3      |
| powerpc64 | Tier 3        | Tier 3        | Tier 4        | N/A           | Tier 3        | Tier 3      |
| ...       | ...           | ...           | ...           | ...           | ...           | ...         |

## Documentation & Download

### Documentation

* [Download & Documentation](https://ziglang.org/download/)
* [Language Reference](https://ziglang.org/documentation/master/)
* [Standard Library Documentation](https://ziglang.org/documentation/0.7.0/std/)

### Download

* [zig-freebsd-x86_64](https://ziglang.org/download/0.7.0/zig-freebsd-x86_64-0.7.0.tar.xz)
* [zig-linux-aarch64](https://ziglang.org/download/0.7.0/zig-linux-aarch64-0.7.0.tar.xz)
* [zig-linux-armv7a](https://ziglang.org/download/0.7.0/zig-linux-armv7a-0.7.0.tar.xz)
* [zig-linux-riscv64](https://ziglang.org/download/0.7.0/zig-linux-riscv64-0.7.0.tar.xz)
* [zig-linux-x86_64](https://ziglang.org/download/0.7.0/zig-linux-x86_64-0.7.0.tar.xz)
* [zig-windows-x86_64](https://ziglang.org/download/0.7.0/zig-windows-x86_64-0.7.0.zip)

## Targets

Zig supports generating code for all targets that LLVM supports.

```bash
$ zig targets

$ llc --version
$ llc -march=x86-64 -mattr=help
$ llc -march=riscv64 -mattr=help
$ llc -march=aarch64 -mattr=help
$ llc -march=mips -mattr=help
$ llc -march=arm -mattr=help
$ llc -march=wasm32 -mattr=help
$ llc -march=ppc64le -mattr=help
```

## Hello World

```bash
$ cat << EOF > HelloWorld.zig
const std = @import("std");

pub fn main() !void {
    const stdout = std.io.getStdOut().writer();
    try stdout.print("Hello, {}!\n", .{"world"});
}
EOF
```

### wasm32-wasi

```bash
# https://github.com/wasmerio/wasmer
# https://github.com/bytecodealliance/wasmtime
$ zig build-exe -target wasm32-wasi -O ReleaseSmall --strip HelloWorld.zig && wasmer HelloWorld.wasm && wasmtime HelloWorld.wasm
Hello, world!
Hello, world!
$ file HelloWorld.wasm && du -ks HelloWorld.wasm
HelloWorld.wasm: WebAssembly (wasm) binary module version 0x1 (MVP)
4       HelloWorld.wasm
```

### x86_64-linux-gnu

```bash
$ zig build-exe HelloWorld.zig
$ ./HelloWorld
Hello, world!
$ du -ks HelloWorld
832     HelloWorld
$ file HelloWorld
HelloWorld: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, with debug_info, not stripped

$ zig build-exe -target x86_64-linux-gnu -mcpu core_avx2 -O ReleaseSmall -fPIC --strip HelloWorld.zig && ./HelloWorld
Hello, world!
$ file HelloWorld && du -ks HelloWorld
HelloWorld: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped
8       HelloWorld
```

### mips-linux-gnu

```bash
$ zig build-exe -target mips-linux-gnu -mcpu mips32r2 -O ReleaseSmall --strip HelloWorld.zig && qemu-mips-static HelloWorld
$ file HelloWorld && du -ks HelloWorld
HelloWorld: ELF 32-bit MSB executable, MIPS, MIPS32 rel2 version 1 (SYSV), statically linked, not stripped
12      HelloWorld


$ zig build-exe -target mips-linux-gnu -mcpu mips32r6 -O ReleaseSmall --strip HelloWorld.zig && qemu-mips-static ./HelloWorld
$ file HelloWorld && du -ks HelloWorld
HelloWorld: ELF 32-bit MSB executable, MIPS, MIPS32 rel6 version 1 (SYSV), statically linked, not stripped
12      HelloWorld
```

### arm-linux-gnueabihf

```bash
$ zig build-exe -target arm-linux-gnueabihf -mcpu arm1176jzf_s -O ReleaseSafe HelloWorld.zig  && qemu-arm-static ./HelloWorld
$ file HelloWorld && du -ks HelloWorld
HelloWorld: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), statically linked, with debug_info, not stripped
780     HelloWorld


$ zig build-exe -target arm-linux-gnueabihf  -mcpu cortex_a7 -O ReleaseSafe  HelloWorld.zig  && qemu-arm-static ./HelloWorld
$ file HelloWorld && du -ks HelloWorld
HelloWorld: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), statically linked, with debug_info, not stripped
772     HelloWorld
```

### aarch64-linux-gnu

```bash
$ zig build-exe -target aarch64-linux-gnu -mcpu cortex_a72 -O ReleaseSafe -fPIC  HelloWorld.zig && qemu-aarch64-static HelloWorld
Hello, world!
$ file HelloWorld && du -ks HelloWorld
HelloWorld: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, with debug_info, not stripped
596     HelloWorld
```

### powerpc64le-linux-gnu

```bash
$ zig build-exe -target powerpc64le-linux-gnu -mcpu pwr9 -O ReleaseSafe -fPIC HelloWorld.zig && qemu-ppc64le-static HelloWorld
Hello, world!
$ file HelloWorld && du -ks HelloWorld
HelloWorld: ELF 64-bit LSB executable, 64-bit PowerPC or cisco 7500, version 1 (SYSV), statically linked, with debug_info, not stripped
632     HelloWorld
```

### riscv64-linux-gnu

```bash
$ zig build-exe -target riscv64-linux-gnu -O ReleaseSafe HelloWorld.zig && qemu-riscv64-static HelloWorld
Hello, world!
$ file HelloWorld && du -ks HelloWorld
HelloWorld: ELF 64-bit LSB executable, UCB RISC-V, version 1 (SYSV), statically linked, with debug_info, not stripped
728     HelloWorld
```
