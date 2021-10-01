---
title: Cross compiling Rust for Windows MSVC target
excerpt: Cross compiling Rust for Windows MSVC target
date: 2021-03-14 15:51:51
tags:
    - Programming
    - Rust
categories: [Programming, Rust]
---

# Cross compiling Rust for Windows MSVC target

Generally, We can compiling Rust applications for the Windows target **x86_64-pc-windows-gnu**. But for the Windows target **x86_64-pc-windows-msvc**, we need **Wine** or on the Windows host. Now, we can use **lld** to create the Windows target **x86_64-pc-windows-gnu**.

## Copy MSVC libraries to the Linux host

### The target directory

```shell
# x86_64-unknown-linux-gnu -> x86_64-pc-windows-msvc
export RUST_MSVC_LIB_PATH=${RUSTUP_HOME:-~/.rustup}/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-pc-windows-msvc/lib

# x86_64-unknown-linux-musl -> x86_64-pc-windows-msvc
export RUST_MSVC_LIB_PATH=${RUSTUP_HOME:-~/.rustup}/toolchains/stable-x86_64-unknown-linux-musl/lib/rustlib/x86_64-pc-windows-msvc/lib
```

### Copy VC runtime libraries

- [C runtime (CRT) and C++ Standard Library (STL) files](https://docs.microsoft.com/en-us/cpp/c-runtime-library/crt-library-features)

```shell
# You need adjust the source directory if needed.
cd "/mnt/c/Program Files (x86)/Microsoft Visual Studio/2019/Enterprise/VC/Tools/MSVC/14.29.30133/lib/x64"
cd "/mnt/c/Program Files (x86)/Microsoft Visual Studio/2019/BuildTools/VC/Tools/MSVC/14.29.30133/lib/x64"

cp vcruntime.lib                    ${RUST_MSVC_LIB_PATH}/vcruntime.lib
cp msvcrt.lib                       ${RUST_MSVC_LIB_PATH}/msvcrt.lib
cp msvcprt.lib                      ${RUST_MSVC_LIB_PATH}/msvcprt.lib
cp iso_stdio_wide_specifiers.lib    ${RUST_MSVC_LIB_PATH}/iso_stdio_wide_specifiers.lib
cp libvcruntime.lib                 ${RUST_MSVC_LIB_PATH}/libvcruntime.lib
cp libcmt.lib                       ${RUST_MSVC_LIB_PATH}/libcmt.lib
cp libcpmt.lib                      ${RUST_MSVC_LIB_PATH}/libcpmt.lib
```

### Copy UCRT libraries

- [C runtime (CRT) and C++ Standard Library (STL) files](https://docs.microsoft.com/en-us/cpp/c-runtime-library/crt-library-features)

```shell
# You need adjust the source directory if needed.
cd "/mnt/c/Program Files (x86)/Windows Kits/10/Lib/10.0.19041.0/ucrt/x64"

cp ucrt.lib     ${RUST_MSVC_LIB_PATH}/ucrt.lib
cp libucrt.lib  ${RUST_MSVC_LIB_PATH}/libucrt.lib
```

### Copy Windows API libraries

```shell
# You need adjust the source directory if needed.
cd "/mnt/c/Program Files (x86)/Windows Kits/10/Lib/10.0.19041.0/um/x64"

cp AdvAPI32.Lib ${RUST_MSVC_LIB_PATH}/advapi32.lib
cp bcrypt.lib   ${RUST_MSVC_LIB_PATH}/bcrypt.lib
cp Crypt32.Lib  ${RUST_MSVC_LIB_PATH}/crypt32.lib
cp kernel32.Lib ${RUST_MSVC_LIB_PATH}/kernel32.lib
cp MsWSock.Lib  ${RUST_MSVC_LIB_PATH}/mswsock.lib
cp odbc32.lib   ${RUST_MSVC_LIB_PATH}/odbc32.lib
cp OneCore_apiset.Lib ${RUST_MSVC_LIB_PATH}/onecore_apiset.lib
cp OneCore.Lib  ${RUST_MSVC_LIB_PATH}/onecore.lib
cp Psapi.Lib    ${RUST_MSVC_LIB_PATH}/psapi.lib
cp User32.Lib   ${RUST_MSVC_LIB_PATH}/user32.lib
cp UserEnv.Lib  ${RUST_MSVC_LIB_PATH}/userenv.lib
cp Uuid.Lib     ${RUST_MSVC_LIB_PATH}/uuid.lib
cp WS2_32.Lib   ${RUST_MSVC_LIB_PATH}/ws2_32.lib
```

### Windows umbrella libraries

- [Windows API sets](https://docs.microsoft.com/en-us/windows/win32/apiindex/windows-apisets)
- [Detect API set availability](https://docs.microsoft.com/en-us/windows/win32/apiindex/detect-api-set-availability)
- [Windows umbrella libraries](https://docs.microsoft.com/en-us/windows/win32/apiindex/windows-umbrella-libraries)
- [Building for OneCore](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-for-onecore)

Library               | Scenario
----------------------|---------
OneCore.lib           | All editions of Windows 7 and later, no UWP support
OneCore_apiset.lib    | This library provides the same coverage as OneCore.lib, but uses API set direct forwarding. Note that using this library will be compatible only with the Windows 10 version as specified by the SDK verion you're targeting, or greater.
OneCoreUAP.lib        | Windows 7 and later, UWP editions (Desktop, IoT, HoloLens, but not Nano Server) of Windows 10
OneCoreUAP_apiset.lib | This library provides the same coverage as OneCoreUAP.lib, but uses the API set direct forwarding technique. Note that using this library will be compatible only with the Windows 10 version as specified by the SDK you're targeting, or greater.

## How we known which libraries need to be copy

During the link time, rust will report which libraries not found, just find them on Windows, and copy them to Linux.

### Rust linked libraries

- <https://github.com/rust-lang/rust/blob/master/library/std/src/sys/windows/mod.rs>

```rust
        #[link(name = "advapi32")]
        #[link(name = "ws2_32")]
        #[link(name = "userenv")]
```

### LLVM linked libraries

- <https://github.com/llvm/llvm-project/blob/main/libcxx/CMakeLists.txt>

```cmake
        target_link_libraries(${target} PRIVATE ucrt${LIB_SUFFIX}) # Universal C runtime
        target_link_libraries(${target} PRIVATE vcruntime${LIB_SUFFIX}) # C++ runtime
        target_link_libraries(${target} PRIVATE msvcrt${LIB_SUFFIX}) # C runtime startup files
        target_link_libraries(${target} PRIVATE msvcprt${LIB_SUFFIX}) # C++ standard library. Required for exception_ptr internals.
        # Required for standards-complaint wide character formatting functions (e.g. `printfw`/`scanfw`)
        target_link_libraries(${target} PRIVATE iso_stdio_wide_specifiers)
```

## Let's have a test

### Hello World

```bash
$ cat << EOF > hello-world.rs
fn main() {
    println!("Hello, world!");
}
EOF
```

### Use DLL C runtime library

```bash
$ rustc -C opt-level=3 -C link-arg="-s" \
    --target=x86_64-pc-windows-msvc -C linker=lld-link -o hello-world-windows-x86_64.exe \
    hello-world.rs

$ du -ks hello-world-windows-x86_64.exe
152     hello-world-windows-x86_64.exe

$ file hello-world-windows-x86_64.exe
hello-world-windows-x86_64.exe: PE32+ executable (console) x86-64, for MS Windows

$ /usr/bin/x86_64-w64-mingw32-objdump -x hello-world-windows-x86_64.exe | grep -E '^\s*DLL Name'
        DLL Name: KERNEL32.dll
        DLL Name: VCRUNTIME140.dll
        DLL Name: api-ms-win-crt-runtime-l1-1-0.dll
        DLL Name: api-ms-win-crt-stdio-l1-1-0.dll
        DLL Name: api-ms-win-crt-math-l1-1-0.dll
        DLL Name: api-ms-win-crt-locale-l1-1-0.dll
        DLL Name: api-ms-win-crt-heap-l1-1-0.dll
```

```powershell
PS C:\> dumpbin /NOLOGO /DEPENDENTS hello-world-windows-x86_64.exe

Dump of file hello-world-windows-x86_64.exe

File Type: EXECUTABLE IMAGE

  Image has the following dependencies:

    KERNEL32.dll
    VCRUNTIME140.dll
    api-ms-win-crt-runtime-l1-1-0.dll
    api-ms-win-crt-stdio-l1-1-0.dll
    api-ms-win-crt-math-l1-1-0.dll
    api-ms-win-crt-locale-l1-1-0.dll
    api-ms-win-crt-heap-l1-1-0.dll

  Summary

        1000 .00cfg
        1000 .data
        1000 .gehcont
        2000 .pdata
        8000 .rdata
        1000 .reloc
       1C000 .text
        1000 .tls
```

### Use static C runtime library

```bash
$ rustc -C opt-level=3 -C link-arg="-s" -C "target-feature=+crt-static" \
    --target=x86_64-pc-windows-msvc -C linker=lld-link -o hello-world-windows-x86_64.exe \
    hello-world.rs

$ du -ks hello-world-windows-x86_64.exe
244     hello-world-windows-x86_64.exe

$ file hello-world-windows-x86_64.exe
hello-world-windows-x86_64.exe: PE32+ executable (console) x86-64, for MS Windows

$ /usr/bin/x86_64-w64-mingw32-objdump -x hello-world-windows-x86_64.exe | grep -E '^\s*DLL Name'
        DLL Name: KERNEL32.dll
```

```powershell
PS C:\> dumpbin /NOLOGO /DEPENDENTS hello-world-windows-x86_64.exe

Dump of file hello-world-windows-x86_64.exe

File Type: EXECUTABLE IMAGE

  Image has the following dependencies:

    KERNEL32.dll

  Summary

        1000 .00cfg
        2000 .data
        1000 .gehcont
        3000 .pdata
       10000 .rdata
        1000 .reloc
       29000 .text
        1000 .tls
        1000 _RDATA
```
