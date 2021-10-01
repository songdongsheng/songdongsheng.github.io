---
title: NDK console program
excerpt: NDK console program
date: 2020-04-05 23:02:24
tags:
  - Programming
  - C/C++
categories: [Programming, C/C++]
---

# NDK console program

## Install NDK

- https://developer.android.com/ndk/reference
- https://developer.android.com/ndk/guides/abis
- https://developer.android.com/ndk/downloads

```bash
export NDK_BUNDLE_DIR=/opt/android-ndk-r21b
export NDK_DL_URL=https://dl.google.com/android/repository/android-ndk-r21b-linux-x86_64.zip
export PATH=/opt/maven-3/bin:/opt/jdk-8/bin:/usr/sbin:/usr/bin:/sbin:/bin
export PATH=${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin:${NDK_BUNDLE_DIR}:$PATH

/bin/rm -fr ${NDK_BUNDLE_DIR} \
    && mkdir -p ${NDK_BUNDLE_DIR} \
    && curl -sSL -o /tmp/ndk.zip ${NDK_DL_URL} \
    && (cd ${NDK_BUNDLE_DIR}/.. && unzip /tmp/ndk.zip)
```

## Hello, World

### Hello.c

```c
/*
 * gcc -g -std=c11 -Wall -Wextra -Wformat -pedantic -o Hello Hello.c
 * valgrind -v ./Hello
 */
#include <stdio.h>
int main()
{
    printf("Hello, World!\n");
    return 0;
}
```

### Hello.cpp

```cpp
/*
 * g++ -g -std=c++17 -Wall -Wextra -Wformat -pedantic -o Hello Hello.cpp
 * valgrind -v ./Hello
 */
#include <iostream>
int main()
{
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

### Android.mk

```make
LOCAL_PATH := $(call my-dir)
include $(CLEAR_VARS)

LOCAL_MODULE := Hello

LOCAL_SRC_FILES := Hello.c
#LOCAL_SRC_FILES := Hello.cpp

include $(BUILD_EXECUTABLE)
```

### Application.mk

```make
APP_PLATFORM := android-24
APP_BUILD_SCRIPT := Android.mk
APP_ABI := armeabi-v7a arm64-v8a x86_64

APP_STL := none
#APP_STL := c++_static
```

### ndk-build

```shell
# ndk-build NDK_PROJECT_PATH=. NDK_APPLICATION_MK=Application.mk
[armeabi-v7a] Executable     : Hello
[armeabi-v7a] Install        : Hello => libs/armeabi-v7a/Hello
[arm64-v8a] Compile        : Hello <= Hello.c
[arm64-v8a] Executable     : Hello
[arm64-v8a] Install        : Hello => libs/arm64-v8a/Hello
[x86_64] Compile        : Hello <= Hello.c
[x86_64] Executable     : Hello
[x86_64] Install        : Hello => libs/x86_64/Hello

# file libs/armeabi-v7a/Hello libs/arm64-v8a/Hello libs/x86_64/Hello
libs/armeabi-v7a/Hello: ELF 32-bit LSB shared object, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /system/bin/linker, BuildID[sha1]=899b8ea06a0b7372e7eef43db09bc145d5140ca5, stripped
libs/arm64-v8a/Hello:   ELF 64-bit LSB shared object, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /system/bin/linker64, BuildID[sha1]=1d490b72bbf70379b127a49f60d0383a62312937, stripped
libs/x86_64/Hello:      ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /system/bin/linker64, BuildID[sha1]=2af85ada459a58e93ab96e7aceb5b546f8bce80d, stripped

arm-linux-androideabi-readelf -h libs/armeabi-v7a/Hello
arm-linux-androideabi-readelf -A libs/armeabi-v7a/Hello
arm-linux-androideabi-readelf -p .interp libs/armeabi-v7a/Hello
arm-linux-androideabi-readelf -p .rodata libs/armeabi-v7a/Hello

aarch64-linux-android-readelf -h libs/arm64-v8a/Hello
aarch64-linux-android-readelf -p .interp libs/arm64-v8a/Hello
aarch64-linux-android-readelf -p .rodata libs/arm64-v8a/Hello
```

## Android Debug Bridge

```shell
adb push libs/armeabi-v7a/Hello /data/local/tmp/Hello
adb shell /data/local/tmp/Hello

adb push libs/arm64-v8a/Hello /data/local/tmp/Hello
adb shell /data/local/tmp/Hello

adb push libs/x86_64/Hello /data/local/tmp/Hello
adb shell /data/local/tmp/Hello
```
