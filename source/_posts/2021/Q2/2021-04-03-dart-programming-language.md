---
title: Dart programming language
excerpt: Dart programming language
date: 2021-04-03 22:00:31
tags:
    - Programming
    - Dart
categories: [Programming, Dart]
---

# Dart programming language

## Dart overview

Dart is a client-optimized language for developing fast apps on any platform. Its goal is to offer the most productive programming language for multi-platform development, paired with a flexible execution runtime platform for app frameworks.

* Optimized for UI
* Productive development
* Fast on all platforms

## Learning Dart

* [Dart language specification](https://dart.dev/guides/language/spec)
* [API reference](https://api.dart.dev/)
* [Core libraries](https://dart.dev/guides/libraries)
* [A tour of the Dart language](https://dart.dev/guides/language/language-tour)
* [A tour of the core libraries](https://dart.dev/guides/libraries/library-tour)
* [Effective Dart](https://dart.dev/guides/language/effective-dart)
* [Tools & techniques](https://dart.dev/tools)
* [Command-line & server apps](https://dart.dev/server)

## Get the Dart SDK

Stable, beta, and dev channel releases are available at URLs like the following:

```
https://storage.googleapis.com/dart-archive/channels/<stable|beta|dev>/release/<version>/sdk/dartsdk-<platform>-<architecture>-release.zip
```

* [Dart SDK archive](https://dart.dev/tools/sdk/archive)
* [Stable channel - 2.12.3 - Windows](https://storage.googleapis.com/dart-archive/channels/stable/release/2.12.3/sdk/dartsdk-windows-x64-release.zip)
* [Stable channel - 2.12.3 - Linux](https://storage.googleapis.com/dart-archive/channels/stable/release/2.12.3/sdk/dartsdk-linux-x64-release.zip)

## HelloWorld.dart

```dart
$ cat << EOF > HelloWorld.dart
void main() {
  print('Hello, World!');
}
EOF
```

## Dart on Windows

```shell-session
C:\>dart run HelloWorld.dart
Hello, World!

C:\>dart compile exe -o HelloWorld.exe HelloWorld.dart
Info: Compiling with sound null safety
Generated: helloworld.exe

C:\>helloworld.exe
Hello, World!

C:\>du -ks helloworld.exe
4836    helloworld.exe

C:\>file helloworld.exe
helloworld.exe: PE32+ executable (console) x86-64, for MS Windows

C:\>dumpbin /NOLOGO /DEPENDENTS helloworld.exe
Dump of file helloworld.exe

File Type: EXECUTABLE IMAGE

  Image has the following dependencies:

    IPHLPAPI.DLL
    PSAPI.DLL
    WS2_32.dll
    RPCRT4.dll
    SHLWAPI.dll
    ADVAPI32.dll
    SHELL32.dll
    dbghelp.dll
    CRYPT32.dll
    KERNEL32.dll

  Summary

       41000 .data
       16000 .pdata
       CB000 .rdata
        A000 .reloc
      2AB000 .text
```

## Dart on Linux

```shell-session
$ dart run HelloWorld.dart
Hello, World!

$ dart compile exe -o HelloWorld HelloWorld.dart
Info: Compiling with sound null safety
Generated: HelloWorld

$ ./HelloWorld
Hello, World!

$ du -ks HelloWorld
5736    HelloWorld

$ file HelloWorld
HelloWorld: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, stripped

$ readelf -hld HelloWorld
ld-linux-x86-64.so.2
libc.so.6
libdl.so.2
libm.so.6
libpthread.so.0
```
