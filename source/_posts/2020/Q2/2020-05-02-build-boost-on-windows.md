---
title: Build Boost on Windows
excerpt: Build Boost on Windows
date: 2020-05-02 18:22:51
tags:
  - Programming
  - C/C++
categories: [Programming, C/C++]
---

# Build Boost on Windows

## Download Boost C++ Libraries

```Batchfile
C:\>aria2c -c -m 0 -t 5 -j 8 -s 8 -x 8 -k 1m --file-allocation=none ^
    https://dl.bintray.com/boostorg/release/1.73.0/source/boost_1_73_0.tar.gz

C:\>tar -xzf boost_1_73_0.tar.gz
```

## Install Visual Studio Build Tools

Install [Build Tools for Visual Studio 2019](https://aka.ms/buildtools), then open **x64 Native Tools Command Prompt for VS 2019**.

```Batchfile
C:\>cl
Microsoft (R) C/C++ Optimizing Compiler Version 19.26.28806 for x64
Copyright (C) Microsoft Corporation.  All rights reserved.

usage: cl [ option... ] filename... [ /link linkoption... ]
```

## Build and Install Boost.Build (b2)

1. Go to the directory tools\build\
2. Run bootstrap.bat
3. Run b2 install --prefix=PREFIX where PREFIX is the directory where you want Boost.Build to be installed
4. Add PREFIX\bin to your PATH environment variable.

```Batchfile
cd tools\build\

bootstrap.bat

b2 install --prefix=C:\opt\boost-build

SET PATH=C:\opt\boost-build\bin;%PATH%
```

## Build Boost with Boost.Build (b2)

### b2 configuration

```
    https://boostorg.github.io/build/manual/master/index.html

    --layout=versioned (static)
    --layout=system (shared)

    variant=debug,release
    link=shared,static
    threading=single,multi
    runtime-link=shared,static
    address-model=32,64
    debug-symbols=on
```

### b2 - combination

```shell
while read B2_LINK B2_VARIANT REST; do
    echo b2 -j4 --layout=system \
        --build-dir=..\\boost-build-x64-${B2_LINK}-${B2_VARIANT} \
        --prefix=C:\\opt\\boost-x64-${B2_LINK}-${B2_VARIANT} \
        link=${B2_LINK} variant=${B2_VARIANT} \
        toolset=msvc runtime-link=shared threading=multi address-model=64 \
        install
done << EOF
shared  release
shared  debug
static  release
static  debug
EOF

$ du -ms boost-x64-*
206     boost-x64-shared-debug
175     boost-x64-shared-release
691     boost-x64-static-debug
354     boost-x64-static-release
1426    boost-x64-*

$ (du -ms boost-x64; cd boost-x64; du -ms include/ lib/shared/* lib/static/*)
971     boost-x64
151     include/
56      lib/shared/debug
24      lib/shared/release
540     lib/static/debug
203     lib/static/release

1426 - 971 = 455 MB
```

### b2 - static - release

```Batchfile
b2 -j4 --layout=system ^
    --build-dir=..\boost-build-x64-static-release ^
    --prefix=C:\opt\boost-x64-static-release ^
    toolset=msvc variant=release threading=multi link=static runtime-link=shared address-model=64 install
```

### b2 - shared - release

```Batchfile
b2 -j4 --layout=system ^
    --build-dir=..\boost-build-x64-shared-release ^
    --prefix=C:\opt\boost-x64-shared-release ^
    toolset=msvc variant=release threading=multi link=shared runtime-link=shared address-model=64 install
```

## Link to Boost Library

### original structure

```Batchfile
# BOOST_LIB_BUILDID, BOOST_ALL_NO_LIB, BOOST_AUTO_LINK_NOMANGLE, BOOST_LIB_DIAGNOSTIC

# 37,376
cl /MD /EHsc /DBOOST_ALL_NO_LIB=1 /I C:\opt\boost-x64-static-release\include makepasswd_boost.cpp ^
     /link /LIBPATH:C:\opt\boost-x64-static-release\lib libboost_random.lib libboost_system.lib

# 37,376
cl /MD /EHsc /DBOOST_AUTO_LINK_SYSTEM /I C:\opt\boost-x64-static-release\include makepasswd_boost.cpp ^
     /link /LIBPATH:C:\opt\boost-x64-static-release\lib

# 50,688
cl /MDd /EHsc /DBOOST_AUTO_LINK_SYSTEM /I C:\opt\boost-x64-static-debug\include makepasswd_boost.cpp ^
     /link /LIBPATH:C:\opt\boost-x64-static-debug\lib

# 23,040
cl /MD /EHsc /DBOOST_ALL_NO_LIB=1 /I C:\opt\boost-x64-shared-release\include makepasswd_boost.cpp ^
     /link /LIBPATH:C:\opt\boost-x64-shared-release\lib boost_random.lib boost_system.lib

# 23,040
cl /MD /EHsc /DBOOST_AUTO_LINK_NOMANGLE /I C:\opt\boost-x64-shared-release\include makepasswd_boost.cpp ^
     /link /LIBPATH:C:\opt\boost-x64-shared-release\lib

# 29,184
cl /MDd /EHsc /DBOOST_AUTO_LINK_NOMANGLE /I C:\opt\boost-x64-shared-debug\include makepasswd_boost.cpp ^
     /link /LIBPATH:C:\opt\boost-x64-shared-debug\lib
```

### compact structure

```
boost-x64/include/boost/version.hpp
boost-x64/lib/static/release/boost_system.lib
boost-x64/lib/shared/debug/boost_system.lib
```

```Batchfile
# BOOST_LIB_BUILDID, BOOST_ALL_NO_LIB, BOOST_AUTO_LINK_NOMANGLE, BOOST_LIB_DIAGNOSTIC

# 23,040
cl /MD /EHsc /DBOOST_AUTO_LINK_NOMANGLE /I C:\opt\boost-x64\include makepasswd_boost.cpp ^
     /link /LIBPATH:C:\opt\boost-x64\lib\shared\release

# 29,184
cl /MDd /EHsc /DBOOST_AUTO_LINK_NOMANGLE /I C:\opt\boost-x64\include makepasswd_boost.cpp ^
     /link /LIBPATH:C:\opt\boost-x64\lib\shared\debug

# 37,376
cl /MD /EHsc /DBOOST_AUTO_LINK_SYSTEM /I C:\opt\boost-x64\include makepasswd_boost.cpp ^
     /link /LIBPATH:C:\opt\boost-x64\lib\static\release

# 50,688
cl /MDd /EHsc /DBOOST_AUTO_LINK_SYSTEM /I C:\opt\boost-x64\include makepasswd_boost.cpp ^
     /link /LIBPATH:C:\opt\boost-x64\lib\static\debug
```
