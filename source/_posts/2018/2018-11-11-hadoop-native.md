---
title: Hadoop Native
description: Hadoop Native for Linux and Windows
date: 2018-11-11 14:48:33
tags:
  - Cloud
  - Hadoop
categories: [Cloud, Hadoop]
permalink: hadoop-native
---
# Linux kernel 3.10 or later
## setup
### yum
> yum install -y cmake gcc-c++ protobuf-compiler
```
cmake-2.8.12.2-2.el7.x86_64
gcc-c++-4.8.5-39.el7.x86_64
protobuf-compiler-2.5.0-8.el7.x86_64
```

### jdk 1.8 & maven 3.6
```
export JAVA_HOME=/opt/jdk-8
export PATH=/opt/jdk-8/bin:/opt/maven-3/bin:/opt/cmake-3/bin:/usr/sbin:/usr/bin:/sbin:/bin

mkdir -p /opt/jdk-8 && curl -sSL https://cdn.azul.com/zulu/bin/zulu8.44.0.11-ca-jdk8.0.242-linux_x64.tar.gz \
    | tar -xvz --strip-components=1 -C /opt/jdk-8

mkdir -p /opt/maven-3 && curl -sSL http://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz \
    | tar -xvz --strip-components=1 -C /opt/maven-3

# mvn --version
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: /opt/maven-3
Java version: 1.8.0_242, vendor: Azul Systems, Inc., runtime: /opt/jdk-8/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.19.0-8-amd64", arch: "amd64", family: "unix"
```

## Hadoop 2.7.7
### Build
```
mvn -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -DskipTests -Pdist clean package

mvn clean
while true; do
    mvn -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -DskipTests package
    [ $? -eq 0 ] && break;
    sleep 300;
done
```

### Binaries
+ [Linux-x64/hadoop-2.7.7-native-c7.tar.gz](2.7.7/Linux-x64/hadoop-2.7.7-native-c7.tar.gz)
```
-rw-r--r-- 1 root root 1235702 Nov  3 20:53 libhadoop.a
lrwxrwxrwx 1 root root      18 Nov  3 20:53 libhadoop.so -> libhadoop.so.1.0.0
-rwxr-xr-x 1 root root  724424 Nov  3 20:53 libhadoop.so.1.0.0
-rw-r--r-- 1 root root  433452 Nov  3 20:53 libhdfs.a
lrwxrwxrwx 1 root root      16 Nov  3 20:53 libhdfs.so -> libhdfs.so.0.0.0
-rwxr-xr-x 1 root root  272064 Nov  3 20:53 libhdfs.so.0.0.0
```

## Hadoop 2.8.5
### Build
```
mvn -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -DskipTests -Pdist clean package

mvn clean
while true; do
    mvn -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -DskipTests package
    [ $? -eq 0 ] && break;
    sleep 300;
done
```

### Binaries
+ [Linux-x64/hadoop-2.8.5-native-c7.tar.gz](2.8.5/Linux-x64/hadoop-2.8.5-native-c7.tar.gz)

## Hadoop 3.0.3
### CMake
    cmake_minimum_required(VERSION 3.1 FATAL_ERROR)

### Binaries
+ [Linux-x64/hadoop-3.0.3-native-c7.tar.gz](3.0.3/Linux-x64/hadoop-3.0.3-native-c7.tar.gz)
+ [3.0.3/Windows-x64/hadoop.dll](3.0.3/Windows-x64/hadoop.dll)
+ [3.0.3/Windows-x64/winutils.exe](3.0.3/Windows-x64/winutils.exe)

## Hadoop 3.1.1
### CMake
    cmake_minimum_required(VERSION 3.1 FATAL_ERROR)

**CMake 3.1 or higher is required. You are running version 2.8.12.2**

    mkdir -p /opt/cmake-3 && curl -sSL https://cmake.org/files/v3.16/cmake-3.16.4-Linux-x86_64.tar.gz \
        | tar -xvz --strip-components=1 -C /opt/cmake-3
    export PATH=/opt/cmake-3/bin:$PATH

### Build
```
mvn -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -DskipTests -Pdist clean package

mvn clean
while true; do
    mvn -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -DskipTests package
    [ $? -eq 0 ] && break;
    sleep 300;
done
```

### Binaries
+ [Linux-x64/hadoop-3.1.1-native-c7.tar.gz](3.1.1/Linux-x64/hadoop-3.1.1-native-c7.tar.gz)
+ [3.1.1/Windows-x64/hadoop.dll](3.1.1/Windows-x64/hadoop.dll)
+ [3.1.1/Windows-x64/winutils.exe](3.1.1/Windows-x64/winutils.exe)

# Windows 10 1709 or later
## setup
### Microsoft Visual C++ 2017
```
https://github.com/protocolbuffers/protobuf/releases/download/v3.6.1/protoc-3.6.1-win32.zip
https://github.com/protocolbuffers/protobuf/releases/download/v2.5.0/protoc-2.5.0-win32.zip
https://github.com/protocolbuffers/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz

https://cmake.org/files/v3.12/cmake-3.12.4-win64-x64.zip
https://aka.ms/vs/15/release/vs_enterprise.exe
http://www.apache.org/dist/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.zip
http://www.apache.org/dist/ant/binaries/apache-ant-1.10.5-bin.zip

http://www.apache.org/dist/hadoop/common/hadoop-3.1.1/hadoop-3.1.1-src.tar.gz
http://www.apache.org/dist/hadoop/common/hadoop-3.0.3/hadoop-3.0.3-src.tar.gz
http://www.apache.org/dist/hadoop/common/hadoop-2.9.1/hadoop-2.9.1-src.tar.gz
http://www.apache.org/dist/hadoop/common/hadoop-2.8.5/hadoop-2.8.5-src.tar.gz
http://www.apache.org/dist/hadoop/common/hadoop-2.7.7/hadoop-2.7.7-src.tar.gz

org.apache.maven.plugin.MojoExecutionException: protoc version is 'libprotoc 2.6.1', expected version is '2.5.0'

GOROOT=C:\opt\go-1.11.2
GOPATH=C:\var\vcs\git\scratch\lang\go

C:\>cmd
Microsoft Windows [Version 10.0.17134.376]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\>go version
go version go1.11.2 windows/amd64

C:\>protoc --version
libprotoc 2.5.0

C:\>cmake --version
cmake version 3.12.4
CMake suite maintained and supported by Kitware (kitware.com/cmake).

C:\>cl
Microsoft (R) C/C++ Optimizing Compiler Version 19.15.26732.1 for x64
Copyright (C) Microsoft Corporation.  All rights reserved.

C:\>link
Microsoft (R) Incremental Linker Version 14.15.26732.1
Copyright (C) Microsoft Corporation.  All rights reserved.
```

```
%comspec% /k "C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\VC\Auxiliary\Build\vcvars64.bat"

SET PATH=C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\VC\Tools\MSVC\14.15.26726\bin\HostX64\x64;%PATH%
SET PATH=C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\VC\VCPackages;%PATH%
SET PATH=C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\CommonExtensions\Microsoft\TeamFoundation\Team Explorer;%PATH%
SET PATH=C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE;%PATH%
SET PATH=C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\Tools;%PATH%
SET PATH=C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\MSBuild\15.0\bin\Roslyn;%PATH%
SET PATH=C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\MSBuild\15.0\bin;%PATH%
SET PATH=C:\Program Files (x86)\Microsoft SDKs\TypeScript\3.0;%PATH%
SET PATH=C:\Program Files (x86)\Microsoft SDKs\Windows\v10.0A\bin\NETFX 4.7.2 Tools\x64%PATH%
SET PATH=C:\Program Files (x86)\Windows Kits\10\bin\10.0.17134.0\x64;%PATH%
SET PATH=C:\Program Files (x86)\Windows Kits\10\bin\x64;%PATH%
SET PATH=C:\WINDOWS\Microsoft.NET\Framework64\v4.0.30319;%PATH%

SET LIB=C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\VC\Tools\MSVC\14.15.26726\ATLMFC\lib\x64
SET LIB=C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\VC\Tools\MSVC\14.15.26726\lib\x64;%LIB%
SET LIB=C:\Program Files (x86)\Windows Kits\10\lib\10.0.17134.0\ucrt\x64;%LIB%
SET LIB=C:\Program Files (x86)\Windows Kits\10\lib\10.0.17134.0\um\x64;%LIB%

SET INCLUDE=C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\VC\Tools\MSVC\14.15.26726\ATLMFC\include
SET INCLUDE=C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\VC\Tools\MSVC\14.15.26726\include;%INCLUDE%
SET INCLUDE=C:\Program Files (x86)\Windows Kits\10\include\10.0.17134.0\ucrt;%INCLUDE%
SET INCLUDE=C:\Program Files (x86)\Windows Kits\10\include\10.0.17134.0\shared;%INCLUDE%
SET INCLUDE=C:\Program Files (x86)\Windows Kits\10\include\10.0.17134.0\um;%INCLUDE%
```

### jdk 1.8 & maven 3.6
```
SET M2_HOME=C:\opt\apache-maven-3.6.0
SET MAVEN_OPTS=-Duser.language=en -Duser.country=US -Dfile.encoding=UTF-8

C:\>java -version
java version "1.8.0_192"
Java(TM) SE Runtime Environment (build 1.8.0_192-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.192-b12, mixed mode)

C:\>ant -version
Apache Ant(TM) version 1.10.5 compiled on July 10 2018

C:\>mvn --version
Apache Maven 3.6.0 (97c98ec64a1fdfee7767ce5ffb20918da4f719f3; 2018-10-25T02:41:47+08:00)
Maven home: C:\opt\apache-maven-3.6.0\bin\..
Java version: 1.8.0_192, vendor: Oracle Corporation, runtime: C:\opt\jdk-8u192\jre
Default locale: en_US, platform encoding: UTF-8
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"

org.codehaus.mojo:native-maven-plugin
javah The command line is too long.

<localRepository>C:/m2</localRepository>

mvn -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -DskipTests package

C:\hadoop-3.0.3-src\hadoop-hdfs-project\hadoop-hdfs-native-client\src
cmake -G "Visual Studio 15 2017 Win64"
Visual Studio 10 2010 Win64

msbuild native.sln   /p:Configuration=Release /p:Platform=x64 /p:PlatformToolset=v141
msbuild winutils.sln /p:Configuration=Release /p:Platform=x64 /p:PlatformToolset=v141

C:\hadoop-3.0.3-src\hadoop-common-project\hadoop-common\src\main\native
C:\hadoop-3.0.3-src\hadoop-common-project\hadoop-common\src\main\winutils

C:\hadoop-3.0.3-src\hadoop-common-project\hadoop-common\target\bin
C:\hadoop-3.0.3-src\hadoop-hdfs-project\hadoop-hdfs-native-client\target\bin

C:\hadoop-3.1.1-src\hadoop-common-project\hadoop-common\src\main\native
C:\hadoop-3.1.1-src\hadoop-common-project\hadoop-common\src\main\winutils

C:\hadoop-3.1.1-src\hadoop-common-project\hadoop-common\target\bin
C:\hadoop-3.1.1-src\hadoop-hdfs-project\hadoop-hdfs-native-client\target\bin


11/04/2018  01:01 PM            87,552 hadoop.dll
11/04/2018  12:51 PM            30,096 hadoop.lib
11/04/2018  01:01 PM           774,144 hadoop.pdb
11/04/2018  01:09 PM            68,096 hdfs.dll
11/04/2018  01:09 PM           425,710 hdfs.lib
11/04/2018  01:09 PM           585,728 hdfs.pdb
11/04/2018  01:01 PM         1,461,772 libwinutils.lib
11/04/2018  01:01 PM           119,296 winutils.exe
11/04/2018  01:01 PM         1,298,432 winutils.pdb

winutils.exe
    -> vcruntime140.dll
hadoop.dll
    -> vcruntime140.dll
hdfs.dll
    -> vcruntime140.dll
    -> jvm.dll: JNI_CreateJavaVM, JNI_GetCreatedJavaVMs
```
