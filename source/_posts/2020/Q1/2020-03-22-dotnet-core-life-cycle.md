---
title: .NET Core Life Cycle
excerpt: .NET Core Life Cycle
date: 2020-03-22 12:46:25
tags:
  - Programming
  - .NET
categories: [Programming, .NET]
---

# .NET Core Life Cycle

## .NET Programming Languages

.NET is a free, cross-platform, open source developer platform, created by Microsoft, for building many different types of applications. With .NET, you can use multiple languages, editors, and libraries to build for web, mobile, desktop, gaming, and IoT. You can write your .NET apps in C#, F#, or Visual Basic. Supported on Windows, Linux, and macOS.

## LTS and Current with .NET Core

.NET Core gives two support options: Long Term Support (LTS) and Current. .NET Core adapted behaviors similar with other open source frameworks and platforms.

Long-term support (LTS) releases have an extended support period. Use [LTS](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) if you need to stay supported on the same version of .NET Core for longer.

LTS is supported for one of these two options – whatever is shorter:
- 3 years after its release or
- 12 months after the next LTS version

Current is supported for one of these three options – whatever is shorter:
- 12 months after the next LTS version or
- 3 months after the next Current version

## .NET Core release lifecycles

- https://dotnet.microsoft.com/platform/support/policy/dotnet-core
- https://github.com/dotnet/core/blob/master/os-lifecycle-policy.md

VERSION       | RELEASE DATE | SUPPORT LEVEL | END OF SUPPORT
--------------|--------------|---------------|---------------
.NET Core 3.1 | 2019-12-03   | LTS           | December 3, 2022
.NET Core 3.0 | 2019-09-23   | Current (EOL) | March 3, 2020
.NET Core 2.2 | 2018-12-04   | Current (EOL) | December 23, 2019
.NET Core 2.1 | 2018-05-30   | LTS           | August 21, 2021

## Download .NET Core

- https://dotnet.microsoft.com/download
- https://dotnet.microsoft.com/download/dotnet-core

The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:

- Host an ASP.NET Core app inside of the IIS worker process (w3wp.exe), called the in-process hosting model.
- Forward web requests to a backend ASP.NET Core app running the Kestrel server, called the out-of-process hosting model.

INTENTION               | OPTION
------------------------|-------
build applications      | .NET SDK
console applications    | .NET Core Runtime
desktop applications    | .NET Desktop Runtime
web/server applications | ASP.NET Core Runtime

## .NET Core 3.1

- https://dotnet.microsoft.com/download/dotnet-core/3.1

### Language support

- C# 8.0
- F# 4.7

### Supported OS versions

OS                              | Version       | Architectures     | Notes
--------------------------------|---------------|-------------------|------
Windows 10 Client               | Version 1607+ | x64, x86          |
Nano Server                     | Version 1803+ | x64, ARM32        |
Windows Server                  | 2012 R2+      | x64, x86          |
Mac OS X                        | 10.13+        | x64               |
Red Hat Enterprise Linux <br/> CentOS <br/> Oracle Linux | 7+ | x64 |
Debian                          | 9+            | x64, ARM32, ARM64 |
Ubuntu                          | 18.04, 16.04  | x64, ARM32, ARM64 |
SUSE Enterprise Linux (SLES)    | 12 SP2+       | x64               |
Alpine Linux                    | 3.8+          | x64, ARM64        |

## .NET Core 5.0

- https://dotnet.microsoft.com/download/dotnet-core/5.0

### Language support

- C# 8.0
- F# 5.0

### Supported OS versions

- https://support.microsoft.com/en-us/help/13853/windows-lifecycle-fact-sheet
- https://docs.microsoft.com/en-us/windows-server/get-started/windows-server-release-info

OS                              | Version       | Architectures     | Notes
--------------------------------|---------------|-------------------|------
Windows 10 Client               | Version 1607+ | x64, x86          |
Nano Server                     | Version 1803+ | x64, ARM32        |
Windows Server                  | 2012 R2+      | x64, x86          |
Mac OS X                        | 10.13+        | x64               |
Red Hat Enterprise Linux <br/> CentOS <br/> Oracle Linux | 7+ | x64 |
Debian                          | 9+            | x64, ARM32, ARM64 |
Ubuntu                          | 18.04, 16.04  | x64, ARM32, ARM64 |
SUSE Enterprise Linux (SLES)    | 12 SP2+       | x64               |
Alpine Linux                    | 3.8+          | x64, ARM64        |
