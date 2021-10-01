---
title: Compile rsync by NDK
excerpt: Compile rsync by NDK
date: 2020-04-06 11:43:17
tags:
  - Programming
  - C/C++
categories: [Programming, C/C++]
---

# Compile rsync by NDK

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

## Android API levels

- https://source.android.com/setup/start/build-numbers
- https://developer.android.com/guide/topics/manifest/uses-sdk-element#ApiLevels

```bash
# ls android-ndk-r21b/platforms | sort
android-16  android-17  android-18  android-19
android-21  android-22  android-23  android-24
android-26  android-27  android-28  android-29
```

## Android Clang targets

Linux target triplets   | Android Clang target parameter
------------------------|-------------------------------
arm-linux-androideabi-* | --target=armv7a-linux-androideabi24
aarch64-linux-android-* |  --target=aarch64-linux-android24
x86_64-linux-android-*  |  --target=x86_64-linux-android24

## Rsync on Android 7.0 (API Level 24) or later

### armv7a-linux-androideabi24

```bash
/bin/rm -fr /tmp/rsync && mkdir -p /tmp/rsync && cd /tmp/rsync && \
    curl -sSL https://download.samba.org/pub/rsync/rsync-3.1.3.tar.gz \
        | tar -xvz --strip-components 1 -C /tmp/rsync

export NDK_TARGET=arm-linux-androideabi
./configure --host="armv7a-linux-androideabi24" \
    AR="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-ar" \
    CC="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/clang --target=armv7a-linux-androideabi24" \
    LD="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-ld" \
    RANLIB="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-ranlib" \
    STRIP="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-strip"

make && ${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-strip rsync

install -m 755 rsync /tmp/rsync-3.1.3.armv7a-androideabi
```

### aarch64-linux-android24

```bash
/bin/rm -fr /tmp/rsync && mkdir -p /tmp/rsync && cd /tmp/rsync && \
    curl -sSL https://download.samba.org/pub/rsync/rsync-3.1.3.tar.gz \
        | tar -xvz --strip-components 1 -C /tmp/rsync

export NDK_TARGET=aarch64-linux-android
./configure --host="${NDK_TARGET}24" \
    AR="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-ar" \
    CC="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/clang --target=${NDK_TARGET}24" \
    LD="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-ld" \
    RANLIB="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-ranlib" \
    STRIP="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-strip"

make && ${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-strip rsync

install -m 755 rsync /tmp/rsync-3.1.3.aarch64-android
```

### x86_64-linux-android24

```bash
/bin/rm -fr /tmp/rsync && mkdir -p /tmp/rsync && cd /tmp/rsync && \
    curl -sSL https://download.samba.org/pub/rsync/rsync-3.1.3.tar.gz \
        | tar -xvz --strip-components 1 -C /tmp/rsync

export NDK_TARGET=x86_64-linux-android
./configure --host="${NDK_TARGET}24" \
    AR="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-ar" \
    CC="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/clang --target=${NDK_TARGET}24" \
    LD="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-ld" \
    RANLIB="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-ranlib" \
    STRIP="${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-strip"

make && ${NDK_BUNDLE_DIR}/toolchains/llvm/prebuilt/linux-x86_64/bin/${NDK_TARGET}-strip rsync

install -m 755 rsync /tmp/rsync-3.1.3.x86_64-android
```

## Android Debug Bridge

### Review build results

```bash
# touch -t 197001010000 rsync*android*

# ls -l rsync*android*
-rwxr-xr-x 1 root root 471336 Jan  1  1970 rsync-3.1.3.aarch64-android
-rwxr-xr-x 1 root root 473224 Jan  1  1970 rsync-3.1.3.armv7a-androideabi
-rwxr-xr-x 1 root root 500264 Jan  1  1970 rsync-3.1.3.x86_64-android

# file rsync*android*
rsync-3.1.3.aarch64-android:    ELF 64-bit LSB shared object, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /system/bin/linker64, stripped
rsync-3.1.3.armv7a-androideabi: ELF 32-bit LSB shared object, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /system/bin/linker, stripped
rsync-3.1.3.x86_64-android:     ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /system/bin/linker64, stripped
```

### armv7a-linux-androideabi24

```bash
adb push rsync-3.1.3.armv7a-androideabi /data/local/tmp/rsync
adb shell chmod 755 /data/local/tmp/rsync

# adb shell /data/local/tmp/rsync --version
rsync  version 3.1.3  protocol version 31
Copyright (C) 1996-2018 by Andrew Tridgell, Wayne Davison, and others.
Web site: http://rsync.samba.org/
Capabilities:
    64-bit files, 64-bit inums, 32-bit timestamps, 64-bit long ints,
    no socketpairs, hardlinks, symlinks, no IPv6, batchfiles, inplace,
    append, no ACLs, xattrs, no iconv, symtimes, prealloc

rsync comes with ABSOLUTELY NO WARRANTY.  This is free software, and you
are welcome to redistribute it under certain conditions.  See the GNU
General Public Licence for details.
```

### aarch64-linux-android24

```bash
adb push rsync-3.1.3.aarch64-android /data/local/tmp/rsync
adb shell chmod 755 /data/local/tmp/rsync

# adb shell /data/local/tmp/rsync --version
rsync  version 3.1.3  protocol version 31
Copyright (C) 1996-2018 by Andrew Tridgell, Wayne Davison, and others.
Web site: http://rsync.samba.org/
Capabilities:
    64-bit files, 64-bit inums, 64-bit timestamps, 64-bit long ints,
    no socketpairs, hardlinks, symlinks, no IPv6, batchfiles, inplace,
    append, no ACLs, xattrs, no iconv, symtimes, prealloc

rsync comes with ABSOLUTELY NO WARRANTY.  This is free software, and you
are welcome to redistribute it under certain conditions.  See the GNU
General Public Licence for details.
```
