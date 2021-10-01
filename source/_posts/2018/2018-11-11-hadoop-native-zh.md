---
title: 为 Hadoop 编译 Linux 和 Windows 本机程序
excerpt: 为 Hadoop 编译 Linux 和 Windows 本机程序
date: 2018-11-11 19:19:10
tags:
  - Cloud
  - Hadoop
categories: [Cloud, Hadoop]
---

# 概述
## 系统环境
+ 64 位英文操作系统
+ Hadoop 2 需要 CMake 2.6 或更新的版本
+ Hadoop 3 需要 CMake 3.1 或更新的版本
    ```
    mkdir -p /opt/cmake-3 && curl -sSL https://cmake.org/files/v3.16/cmake-3.16.4-Linux-x86_64.tar.gz \
        | tar -xvz --strip-components=1 -C /opt/cmake-3
    export PATH=/opt/cmake-3/bin:$PATH
    ```
+ Protocol Buffers 编译器 2.5.0，不支持其它版本
+ JDK 1.8
+ Maven 3.6
+ Linux 用户需要 GCC 的 C++ 语言支持包。
+ Windows 系统用户需要 Microsoft Visual C++ 的 IDE 环境，不支持纯编译工具

## 常见问题
+ 中文 Windows 系统用户需要安装英文支持，切换到英文界面编译
+ Windows 用户需要设置 maven 的 **localRepository** 为极短路径，例如 **C:/repo**。
+ maven 需要下载不少软件包，请配置一个好的公网环境，或者在编译时做好重试，例如：

```
mvn clean
while true; do
    mvn -Dmaven.javadoc.skip=true -Dmaven.test.skip=true -DskipTests -Pdist clean package
    [ $? -eq 0 ] && break;
    sleep 300;
done
```

# Linux
在 CentOS 7 编译 Hadoop 2 最简单，CMake 和 Protocol Buffers 用系统内置的版本即可。

其它的环境需要自己确保 Protocol Buffers 编译器的版本一定是 2.5.0。CMake 的版本不要太旧。


## 系统配置
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
+ [2.7.7/Linux-x64](https://github.com/songdongsheng/hadoop-native/tree/master/2.7.7/Linux-x64)
```
-rw-r--r-- 1 root root 1235702 Nov  3 20:53 libhadoop.a
lrwxrwxrwx 1 root root      18 Nov  3 20:53 libhadoop.so -> libhadoop.so.1.0.0
-rwxr-xr-x 1 root root  724424 Nov  3 20:53 libhadoop.so.1.0.0
-rw-r--r-- 1 root root  433452 Nov  3 20:53 libhdfs.a
lrwxrwxrwx 1 root root      16 Nov  3 20:53 libhdfs.so -> libhdfs.so.0.0.0
-rwxr-xr-x 1 root root  272064 Nov  3 20:53 libhdfs.so.0.0.0
```

## Hadoop 2.8.5
+ [2.8.5/Linux-x64](https://github.com/songdongsheng/hadoop-native/tree/master/2.8.5/Linux-x64)

## Hadoop 3.0.3
+ [3.0.3/Linux-x64](https://github.com/songdongsheng/hadoop-native/tree/master/3.0.3/Linux-x64)

## Hadoop 3.1.1
+ [3.1.1/Linux-x64](https://github.com/songdongsheng/hadoop-native/tree/master/3.1.1/Linux-x64)

# Windows
## 系统配置
### 本机程序
```
https://github.com/protocolbuffers/protobuf/releases/download/v2.5.0/protoc-2.5.0-win32.zip
https://github.com/protocolbuffers/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz

https://cmake.org/files/v3.12/cmake-3.12.4-win64-x64.zip
https://aka.ms/vs/15/release/vs_enterprise.exe

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

## Hadoop 3.0.3
+ [3.0.3/Windows-x64/](https://github.com/songdongsheng/hadoop-native/tree/master/3.0.3/Windows-x64)

## Hadoop 3.1.1
+ [3.1.1/Windows-x64/](https://github.com/songdongsheng/hadoop-native/tree/master/3.1.1/Windows-x64)
