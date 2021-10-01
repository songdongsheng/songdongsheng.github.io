---
title: .NET on Linux
excerpt: .NET on Linux
date: 2021-04-05 23:53:39
tags:
    - Programming
    - .NET
    - Linux
categories: [Programming, .NET]
---

# .NET on Linux

## Supported Versions

Version         | Status    | Latest release | General available | End of support
----------------|-----------|----------------|-------------------|---------------
.NET 5.0        | Current   | 5.0.5          | 2020-11-10        | &nbsp;
.NET Core 3.1   | LTS       | 3.1.13         | 2019-12-03        | 2022-12-03
.NET Core 2.1   | LTS       | 2.1.26         | 2018-05-30        | 2021-08-21

## Supported Linux

* Alpine 3.11, 3.12
* Debian 9, 10
* Fedora 32, 33
* RHEL 7, 8
* SLES 12, 15
* Ubuntu 18.04, 20.04

## Download .NET

* https://dotnet.microsoft.com/download
* https://dotnet.microsoft.com/download/dotnet
* https://dotnet.microsoft.com/download/dotnet-framework

## .NET in Docker

```shell
docker pull mcr.microsoft.com/dotnet/sdk:5.0
docker pull mcr.microsoft.com/dotnet/sdk:3.1
docker pull mcr.microsoft.com/dotnet/sdk:2.1
docker pull mcr.microsoft.com/dotnet/framework/sdk:4.8
```

## Install .NET on Linux

* https://docs.microsoft.com/en-us/dotnet/core/additional-tools/uninstall-tool
* https://docs.microsoft.com/en-us/dotnet/core/install/linux
* https://docs.microsoft.com/en-us/dotnet/core/install/linux-alpine
* https://docs.microsoft.com/en-us/dotnet/core/install/linux-debian
* https://docs.microsoft.com/en-us/dotnet/core/install/linux-fedora
* https://docs.microsoft.com/en-us/dotnet/core/install/linux-rhel
* https://docs.microsoft.com/en-us/dotnet/core/install/linux-sles
* https://docs.microsoft.com/en-us/dotnet/core/install/linux-ubuntu

## Hello World in 10 minutes

### Check everything installed correctly

```shell
$ dotnet  --info
.NET SDK (reflecting any global.json):
 Version:   5.0.202
 Commit:    db7cc87d51

Runtime Environment:
 OS Name:     ubuntu
 OS Version:  20.04
 OS Platform: Linux
 RID:         ubuntu.20.04-x64
 Base Path:   /opt/dotnet-sdk-5/sdk/5.0.202/

Host (useful for support):
  Version: 5.0.5
  Commit:  2f740adc14

.NET SDKs installed:
  3.1.407 [/opt/dotnet-sdk-5/sdk]
  5.0.202 [/opt/dotnet-sdk-5/sdk]

.NET runtimes installed:
  Microsoft.AspNetCore.App 3.1.13 [/opt/dotnet-sdk-5/shared/Microsoft.AspNetCore.App]
  Microsoft.AspNetCore.App 5.0.5 [/opt/dotnet-sdk-5/shared/Microsoft.AspNetCore.App]
  Microsoft.NETCore.App 3.1.13 [/opt/dotnet-sdk-5/shared/Microsoft.NETCore.App]
  Microsoft.NETCore.App 5.0.5 [/opt/dotnet-sdk-5/shared/Microsoft.NETCore.App]

To install additional .NET runtimes or SDKs:
  https://aka.ms/dotnet-download
```

### Create your app

```shell
$ dotnet new console -o myApp

Welcome to .NET 5.0!
---------------------
SDK Version: 5.0.202

Telemetry
---------
The .NET tools collect usage data in order to help us improve your experience. It is collected by Microsoft and shared w
ith the community. You can opt-out of telemetry by setting the DOTNET_CLI_TELEMETRY_OPTOUT environment variable to '1' o
r 'true' using your favorite shell.

Read more about .NET CLI Tools telemetry: https://aka.ms/dotnet-cli-telemetry

----------------
Installed an ASP.NET Core HTTPS development certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
Learn about HTTPS: https://aka.ms/dotnet-https
----------------
Write your first app: https://aka.ms/dotnet-hello-world
Find out what's new: https://aka.ms/dotnet-whats-new
Explore documentation: https://aka.ms/dotnet-docs
Report issues and find source on GitHub: https://github.com/dotnet/core
Use 'dotnet --help' to see available commands or visit: https://aka.ms/dotnet-cli
--------------------------------------------------------------------------------------
Getting ready...
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on myApp/myApp.csproj...
  Determining projects to restore...
  Restored /tmp/x/myApp/myApp.csproj (in 75 ms).
Restore succeeded.
```

### Run your app

```shell
$ find myApp/ -type f
myApp/Program.cs
myApp/myApp.csproj

$ dotnet run
Hello World!
```

### Build your app

```shell
$ dotnet build -c Release
Microsoft (R) Build Engine version 16.9.0+57a23d249 for .NET
Copyright (C) Microsoft Corporation. All rights reserved.

  Determining projects to restore...
  Restored /tmp/x/myApp/myApp.csproj (in 78 ms).
  myApp -> /tmp/x/myApp/bin/Release/net5.0/myApp.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:01.34

$ find /tmp/x/myApp/bin/Release/ -type f
/tmp/x/myApp/bin/Release/net5.0/myApp.deps.json
/tmp/x/myApp/bin/Release/net5.0/myApp.runtimeconfig.json
/tmp/x/myApp/bin/Release/net5.0/myApp.pdb
/tmp/x/myApp/bin/Release/net5.0/myApp.dll
/tmp/x/myApp/bin/Release/net5.0/myApp
/tmp/x/myApp/bin/Release/net5.0/ref/myApp.dll
/tmp/x/myApp/bin/Release/net5.0/myApp.runtimeconfig.dev.json
```

### Publish your app

```shell
$ dotnet publish -c Release
Microsoft (R) Build Engine version 16.9.0+57a23d249 for .NET
Copyright (C) Microsoft Corporation. All rights reserved.

  Determining projects to restore...
  All projects are up-to-date for restore.
  myApp -> /tmp/x/myApp/bin/Release/net5.0/myApp.dll
  myApp -> /tmp/x/myApp/bin/Release/net5.0/publish/

$ find bin/Release/net5.0/publish/ -type f
bin/Release/net5.0/publish/myApp.deps.json
bin/Release/net5.0/publish/myApp.runtimeconfig.json
bin/Release/net5.0/publish/myApp.pdb
bin/Release/net5.0/publish/myApp.dll
bin/Release/net5.0/publish/myApp
```

### Publish the .NET runtime with your app

```shell
$ dotnet publish -c Release --runtime linux-x64
Microsoft (R) Build Engine version 16.9.0+57a23d249 for .NET
Copyright (C) Microsoft Corporation. All rights reserved.

  Determining projects to restore...


  Restored /tmp/x/myApp/myApp.csproj (in 1.43 min).
  myApp -> /tmp/x/myApp/bin/Release/net5.0/linux-x64/myApp.dll
  myApp -> /tmp/x/myApp/bin/Release/net5.0/linux-x64/publish/

$ /tmp/x/myApp/bin/Release/net5.0/linux-x64/myApp
Hello World!

$ du -ks /tmp/x/myApp/bin/Release/net5.0/linux-x64
147756  /tmp/x/myApp/bin/Release/net5.0/linux-x64

$ find /tmp/x/myApp/bin/Release/net5.0/linux-x64 -type f | wc -l
374
```

### Publish Single File with your app

```shell
$ dotnet publish -c Release --runtime linux-x64 -p:PublishSingleFile=true
Microsoft (R) Build Engine version 16.9.0+57a23d249 for .NET
Copyright (C) Microsoft Corporation. All rights reserved.

  Determining projects to restore...
  All projects are up-to-date for restore.
  myApp -> /tmp/x/myApp/bin/Release/net5.0/linux-x64/myApp.dll
  myApp -> /tmp/x/myApp/bin/Release/net5.0/linux-x64/publish/

$ /tmp/x/myApp/bin/Release/net5.0/linux-x64/myApp
Hello World!

$ du -ks /tmp/x/myApp/bin/Release/net5.0/linux-x64
145140  /tmp/x/myApp/bin/Release/net5.0/linux-x64

$ find /tmp/x/myApp/bin/Release/net5.0/linux-x64 -type f | wc -l
190
```
