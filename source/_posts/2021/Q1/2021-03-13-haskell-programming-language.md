---
title: Haskell Programming Language
description: Haskell Programming Language
date: 2021-03-13 23:46:54
tags:
    - Programming
    - Haskell
categories: [Programming, Haskell]
permalink: haskell-programming-language
---

# Haskell Programming Language

Haskell is a general-purpose, statically typed, purely functional programming language with type inference and lazy evaluation. Developed to be suitable for teaching, research and industrial application, Haskell has pioneered a number of advanced programming language features such as type classes, which enable type-safe operator overloading. Haskell's main implementation is the Glasgow Haskell Compiler (GHC). It is named after logician Haskell Curry.

## Features

- Statically typed
- Purely functional
- Type inference
- Concurrent
- Lazy
- Packages

## Overview

- [GHC](https://www.haskell.org/ghc/): A compiler and interpreter for Haskell programs.
- [Cabal](https://www.haskell.org/cabal/): Common Architecture for Building Applications and Libraries.
- [Stack](https://docs.haskellstack.org/): A project builder for multi-package Haskell projects.
- [Haddock](https://www.haskell.org/haddock/): A documentation generator for Haskell packages.
- [Hackage](https://hackage.haskell.org/): The Haskell central packages repository.
- [Stackage](https://www.stackage.org/): A curated repository of thousands of packages installed on demand.

## Usage

Haskell has significant usage in:

- Web development
- Concurrent and parallel programming
- Cluster computing
- Financial modeling
- Scientific and biotech modeling
- Parsers, compilers, type-checkers
- Blockchain

## Documentation

- [Haskell 2010 Language Report - git](https://github.com/haskell/haskell-report)
- [Haskell 2010 Language Report - pdf](https://www.haskell.org/definition/haskell2010.pdf)
- [Haskell 2010 Language Report - online](https://www.haskell.org/onlinereport/haskell2010/)
- [GHC Developer Blog](https://www.haskell.org/ghc/blog.html)
- [Haskell Package Versioning Policy](https://pvp.haskell.org/)
- [The GHC Milestones](https://gitlab.haskell.org/ghc/ghc/-/milestones)
- [The GHC User's Guide and Libraries - gitlab](https://ghc.gitlab.haskell.org/ghc/doc/)
- [The GHC User's Guide and Libraries - latest](https://downloads.haskell.org/~ghc/latest/docs/html/)
- [The GHC User's Guide and Libraries - 9.0](https://downloads.haskell.org/~ghc/9.0-latest/docs/html/)
- [The GHC User's Guide and Libraries - 8.10](https://downloads.haskell.org/~ghc/8.10-latest/docs/html/)
- [The GHC User's Guide and Libraries - 8.8](https://downloads.haskell.org/~ghc/8.8-latest/docs/html/)
- [The Cabal User Guide](https://cabal.readthedocs.io/)
- [The Haskell Tool Stack User Guide](https://docs.haskellstack.org/)
- [The Haddock User Guide](https://haskell-haddock.readthedocs.io/)
- [How to write a Haskell program](https://wiki.haskell.org/How_to_write_a_Haskell_program)
- [FP Complete Haskell](https://www.fpcomplete.com/haskell/learn/)
- [Why Applied Haskell](https://www.snoyman.com/reveal/why-applied-haskell/)
- [Promote Haskell](https://www.fpcomplete.com/haskell/promote/)
- [Asynchronous and Concurrent Programming](https://www.fpcomplete.com/haskell/library/async/)
- [Decoding and encoding binary data](https://www.fpcomplete.com/haskell/library/binary/)
- [How to Script with Stack](https://www.fpcomplete.com/haskell/tutorial/stack-script/)
- [Profiling and Performance](https://www.fpcomplete.com/haskell/tutorial/profiling/)
- [Mutable variables](https://www.fpcomplete.com/haskell/tutorial/mutable-variables/)
- [Why you should use Software Transactional Memory](https://github.com/snoyberg/why-you-should-use-stm)
- [Haskell Tutorial for C Programmers](https://wiki.haskell.org/Haskell_Tutorial_for_C_Programmers)
- [What I Wish I Knew When Learning Haskell](http://dev.stephendiehl.com/hask/)
- [Haskell in Depth](https://livebook.manning.com/book/haskell-in-depth/)
- [Learn You a Haskell for Great Good!](http://learnyouahaskell.com/chapters)
- [Real World Haskell](http://book.realworldhaskell.org/read/)
- [Parallel and Concurrent Programming in Haskell](https://www.oreilly.com/library/view/parallel-and-concurrent/9781449335939/)
- [Stable Haskell package sets](https://www.stackage.org/)
- [Hackage: The Haskell Package Repository](http://hackage.haskell.org/)
- [Haskell API search engine](https://hoogle.haskell.org/)
- [Eta - Modern Haskell on the JVM](https://github.com/typelead/eta)
- [Eta Cheatsheet](https://eta-lang.org/docs/cheatsheets/eta-cheatsheet)
- [2020 State of Haskell Survey](https://taylor.fausak.me/)
- [Simple Haskell is Best Haskell](https://medium.com/@fommil/simple-haskell-is-best-haskell-6a1ea59c73b)
- [Delivering with Haskell](https://levelup.gitconnected.com/delivering-with-haskell-a347d8359597)
- [A Brief Example of servant-persistent](https://www.parsonsmatt.org/2015/06/07/servant-persistent.html)
- [Basic Type Level Programming in Haskell](https://www.parsonsmatt.org/2017/04/26/basic_type_level_programming_in_haskell.html)
- [Servant Route Smooshing](https://www.parsonsmatt.org/2018/03/14/servant_route_smooshing.html)
- [Haskell Language Extension Taxonomy](https://gist.github.com/mightybyte/6c469c125eb50e0c2ebf4ae26b5adfff)
- [Language pragma history](https://gitlab.haskell.org/ghc/ghc/-/wikis/language-pragma-history)
- [GHC 2021](https://github.com/ghc-proposals/ghc-proposals/blob/master/proposals/0380-ghc2021.rst)

## Download and Install

The latest (2021-03-13) **[LTS Haskell](https://www.stackage.org/lts)** 17 use **ghc 8.10.4**, so **don't install ghc 9.0 or later** if you are beginners.

### Visual Studio Code

See [Haskell Language Server (HLS)](https://github.com/haskell/haskell-language-server), you can install from [the VSCode marketplace](https://marketplace.visualstudio.com/items?itemName=haskell.haskell), or manually from the repository [Haskell for Visual Studio Code](https://github.com/haskell/vscode-haskell).

### OS-specific packages

The [OS-specific packages](https://www.haskell.org/downloads/linux/) (eg. RPMs on Linux) are generally a better bet than the vanilla binary bundles, because they will check for dependencies and allow the package to be uninstalled at a later date.

e.g., install GHC 8.10.4 and cabal-install 3.4 on Debian Linux 10 (buster) amd64:

#### GHC on Debian 10

```shell
# sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys BA3CBA3FFE22B574
curl -sSL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0xBA3CBA3FFE22B574" \
  | sudo tee /etc/apt/trusted.gpg.d/downloads.haskell.org.asc

echo deb [arch=amd64] http://downloads.haskell.org/debian buster main \
  | sudo tee /etc/apt/sources.list.d/haskell-buster.list
sudo apt update
sudo apt install --no-install-recommends -y aria2 autoconf automake \
    build-essential ca-certificates clang cmake curl dialog fakeroot file git \
    gnupg less lld llvm man-db manpages-dev ninja-build python3 sudo \
    systemd-container tmux vim \
    libbsd-dev libcurl4-openssl-dev libdwarf-dev libexpat1-dev libffi-dev \
    libgmp-dev libjson-c-dev libjsoncpp-dev liblzma-dev libmariadb-dev \
    libncurses-dev libnuma-dev libpcre2-dev libpcre3-dev libpq-dev \
    librdkafka-dev libssl-dev libunwind-dev libxml2-dev libyaml-dev \
    libzstd-dev zlib1g-dev

sudo apt install -y ghc-8.10.4 cabal-install-3.4
curl -sSL https://get.haskellstack.org/stable/linux-x86_64.tar.gz \
  | sudo tar -xvz --strip-components=1 -C /usr/bin --wildcards */stack

export PATH=/opt/cabal/3.4/bin:/opt/ghc/8.10.4/bin:/usr/bin:/bin:/usr/sbin
```

### Minimal installation

The Minimal installation include GHC (the compiler), build tools (primarily Cabal and Stack), and documentation generator (primarily Haddock).

#### GHC

GHC 8.10 only works with cabal-install version 3.2 or later.
GHC 9.0 only works with cabal-install version 3.4 or later.
GHC 9.2.1 will be released in June 2021 sporting Darwin/ARM support.

- [Downloads for Linux](https://www.haskell.org/downloads/linux/)
- [GHC & Cabal APT Repository](https://downloads.haskell.org/~debian/)
- [GHC APT Repository](https://downloads.haskell.org/debian/pool/main/g/)
- [Cabal APT Repository](https://downloads.haskell.org/debian/pool/main/c/)
- [GHC Downloads Repository](https://downloads.haskell.org/~ghc/)
- [GHC - 8.8 Latest](https://downloads.haskell.org/~ghc/8.8-latest/)
- [GHC - 8.10.4 Source](https://downloads.haskell.org/~ghc/8.10.4/ghc-8.10.4-src.tar.xz)
- [GHC - 8.10.4 for Alpine Linux](https://downloads.haskell.org/~ghc/8.10.4/ghc-8.10.4-x86_64-alpine3.10-linux-integer-simple.tar.xz)
- [GHC - 8.10.4 for Debian Linux](https://downloads.haskell.org/~ghc/8.10.4/ghc-8.10.4-x86_64-deb10-linux.tar.xz)
- [GHC - 8.10.4 for Windows](https://downloads.haskell.org/~ghc/8.10.4/ghc-8.10.4-x86_64-unknown-mingw32.tar.xz)
- [GHC - 9.0 Latest](https://downloads.haskell.org/~ghc/9.0-latest/)
- [GHC - 9.0.1 Source](https://downloads.haskell.org/~ghc/9.0.1/ghc-9.0.1-src.tar.xz)
- [GHC - 9.0.1 for Alpine Linux](https://downloads.haskell.org/~ghc/9.0.1/ghc-9.0.1-x86_64-alpine3.10-linux-integer-simple.tar.xz)
- [GHC - 9.0.1 for Debian Linux](https://downloads.haskell.org/~ghc/9.0.1/ghc-9.0.1-x86_64-deb10-linux.tar.xz)
- [GHC - 9.0.1 for Windows](https://downloads.haskell.org/~ghc/9.0.1/ghc-9.0.1-x86_64-unknown-mingw32.tar.xz)

#### Cabal

- [Cabal Repository](https://downloads.haskell.org/~cabal/)
- [Cabal Repository - Latest](https://downloads.haskell.org/~cabal/cabal-install-latest/)
- [Cabal Repository - Alpine Linux](https://downloads.haskell.org/~cabal/cabal-install-3.4.0.0/cabal-install-3.4.0.0-x86_64-alpine-3.11.6-static-noofd.tar.xz)
- [Cabal Repository - Debian/Ubuntu Linux](https://downloads.haskell.org/~cabal/cabal-install-3.4.0.0/cabal-install-3.4.0.0-x86_64-ubuntu-16.04.tar.xz)
- [Cabal Repository - Windows](https://downloads.haskell.org/~cabal/cabal-install-3.4.0.0/cabal-install-3.4.0.0-x86_64-windows.zip)

Cabal install is done by installing them in the store and symlinking/copying the executables in the directory specified by the `--installdir` flag (`~/.cabal/bin/` by default). If you want the installed executables to be available globally, make sure that the PATH environment variable contains that directory.

```shell
-- ~/.cabal/config
-- %APPDATA%\cabal\config
-- https://cabal.readthedocs.io/en/3.4/installing-packages.html
-- repository my-local-repository
--   url: file+noindex:///absolute/path/to/directory

-- WSL 1 fix: fdLock: invalid argument (Invalid argument)
-- https://github.com/haskell/cabal/issues/6551
-- constraint: lukko -ofd-locking

-- static: True
-- executable-static: True
optimization: 2
executable-stripping: True

program-default-options
  -- -Werror -Wall -threaded -fPIC -static -optl-s -optl-static -optl-pthread
  -- -optc-O3 -optc-ffast-math -funfolding-use-threshold=16
  ghc-options: -O2 -threaded -Wall -fPIC
```

#### Stack

- [Stack on Linux](https://get.haskellstack.org/stable/linux-x86_64.tar.gz)
- [Stack on Windows](https://get.haskellstack.org/stable/windows-x86_64.zip)

```shell
# ~/.stack/config.yaml
# %APPDATA%/stack/config.yaml
# http://docs.haskellstack.org/en/stable/yaml_configuration/
templates:
  params:
    author-email: songdongsheng@live.cn
    author-name: 宋冬生

allow-different-user: true
local-bin-path: C:\Users\EDNGSNG\AppData\Roaming\cabal\bin

color: auto
recommend-stack-upgrade: true

system-ghc: true
install-ghc: false

compiler-check: newer-minor
allow-newer: true

ghc-options:
  # -Werror -Wall -threaded -fPIC -static -optl-s -optl-static -optl-pthread
  #     -optc-O3 -optc-ffast-math -funfolding-use-threshold=16
  # "$locals": -O2 -threaded -Wall -fPIC
  # "$targets": -O2 -threaded -Wall -fPIC
  "$everything": -O2 -threaded -Wall -fPIC

connection-count: 16

package-indices:
  - download-prefix: https://hackage.haskell.org/
    hackage-security:
      keyids:
        - 0a5c7ea47cd1b15f01f5f51a33adda7e655bc0f0b0615baa8e271f4c3351e21d
        - 1ea9ba32c526d1cc91ab5e5bd364ec5e9e8cb67179a471872f6e26f0ae773d42
        - 280b10153a522681163658cb49f632cde3f38d768b736ddbc901d99a1a772833
        - 2a96b1889dc221c17296fcc2bb34b908ca9734376f0f361660200935916ef201
        - 2c6c3627bd6c982990239487f1abd02e08a02e6cf16edb105a8012d444d870c3
        - 51f0161b906011b52c6613376b1ae937670da69322113a246a09f807c62f6921
        - 772e9f4c7db33d251d5c6e357199c819e569d130857dc225549b40845ff0890d
        - aa315286e6ad281ad61182235533c41e806e5a787e0b6d1e7eef3f09d137d2e9
        - fe331502606802feac15e514d9b9ea83fee8b6ffef71335479a2e68d84adc6b0
      key-threshold: 3 # number of keys required
```

#### Haddock

[Haddock](https://www.haskell.org/haddock/) is a documentation generator for Haskell packages.

```shell
PS C:\> cabal update
PS C:\> cabal install haddock
Resolving dependencies...
Build profile: -w ghc-9.0.1 -O1
In order, the following will be built (use -v for more details):
 - ghc-paths-0.1.0.12 (lib:ghc-paths) (requires download & build)
 - haddock-library-1.10.0 (lib) (requires download & build)
 - haddock-api-2.25.0 (lib) (requires download & build)
 - haddock-2.25.0 (exe:haddock) (requires download & build)
Downloading  haddock-library-1.10.0
Downloaded   haddock-library-1.10.0
Downloading  ghc-paths-0.1.0.12
Starting     haddock-library-1.10.0 (lib)
Downloaded   ghc-paths-0.1.0.12
Downloading  haddock-api-2.25.0
Starting     ghc-paths-0.1.0.12 (all, legacy fallback)
Downloaded   haddock-api-2.25.0
Downloading  haddock-2.25.0
Downloaded   haddock-2.25.0
Building     haddock-library-1.10.0 (lib)
Installing   haddock-library-1.10.0 (lib)
Completed    haddock-library-1.10.0 (lib)
Building     ghc-paths-0.1.0.12 (all, legacy fallback)
Installing   ghc-paths-0.1.0.12 (all, legacy fallback)
Completed    ghc-paths-0.1.0.12 (all, legacy fallback)
Starting     haddock-api-2.25.0 (lib)
Building     haddock-api-2.25.0 (lib)
Installing   haddock-api-2.25.0 (lib)
Completed    haddock-api-2.25.0 (lib)
Starting     haddock-2.25.0 (exe:haddock)
Building     haddock-2.25.0 (exe:haddock)
Installing   haddock-2.25.0 (exe:haddock)
Completed    haddock-2.25.0 (exe:haddock)
Copying 'haddock.exe' to
'%APPDATA%\cabal\bin\haddock.exe'
```

## Quick Start

### Docker

- [Minimal Haskell tool-chain Docker Images](https://hub.docker.com/_/haskell?tab=tags)
- [Haskell Docker Images for static executables](https://hub.docker.com/r/utdemir/ghc-musl/tags)

```shell
docker run -it --rm --pull always haskell:8.10-stretch
docker run -it --rm --pull always haskell:8.10-buster

docker run -it --rm --pull always haskell:8.8-stretch
docker run -it --rm --pull always haskell:8.8-buster

docker run -it --rm --pull always fpco/stack-build:lts-16
docker run -it --rm --pull always fpco/stack-build-small:lts-16
```

### Hello GHC

- [Using ghc --make](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/using.html#make-mode)
- [Forcing options to particular phases](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/flags.html#forcing-options-to-particular-phases)

```shell
export PATH=/opt/ghc-8.10/bin:/bin:/usr/bin:/sbin:/usr/sbin

$ ghc --version
$ ghc --show-options
$ ghc --info

$ cat << EOF > HelloWorld.hs
module Main where
main = putStrLn "Hello, World!"
EOF

# ld.lld: error: duplicate symbol: __lll_lock_wait_private
$ rm -f HelloWorld HelloWorld.hi HelloWorld.o && \
    ghc HelloWorld.hs -O2 -optl-s -optc-static -optl-static

$ rm -f HelloWorld HelloWorld.hi HelloWorld.o && \
    ghc HelloWorld.hs -O2 -optl-s

$ ./HelloWorld
Hello, World !

$ du -ks HelloWorld
732     HelloWorld  [8.10.4]
744     HelloWorld  [9.0.1]

$ file HelloWorld
HelloWorld: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked,
interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0,
BuildID[xxHash]=656ac9e0cf71b484, stripped

$ ldd HelloWorld
        linux-vdso.so.1 (0x00007ffc78aa0000)
        libgmp.so.10 => /lib/x86_64-linux-gnu/libgmp.so.10 (0x00007f02d43f8000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f02d4233000)
        libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f02d40ef000)
        librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x00007f02d40e4000)
        libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f02d40de000)
        libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f02d40bc000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f02d4487000)
```

### Hello Cabal

- [Module and Preprocessor](https://cabal.readthedocs.io/en/3.4/cabal-package.html#modules-and-preprocessors)
- [Conditional Block](https://cabal.readthedocs.io/en/3.4/cabal-package.html#conditional-blocks)

```shell
$ export https_proxy=http://example.com:8080

$ cabal --version
$ cabal configure
$ cabal update
$ cabal list | grep '^\*'| sort -u | wc -l
15687
$ cabal list --installed | grep '^\*'| sort -u | wc -l
259
$ ghc-pkg list | awk '{print $1}' | grep -E '^[a-zA-Z]+' | sort -u | wc -l
259

$ cabal install --lib Cabal
Resolving dependencies...
Up to date
$ cabal install --lib socket
$ cabal install hoogle
$ cabal init --minimal --exe --verbose 3
$ cabal build --enable-optimization=2 --enable-executable-stripping \
    --enable-executable-static

%LOCALAPPDATA%\Programs\stack\x86_64-windows\msys2-20200903\mingw64.exe

pacman -Syuv --noconfirm
pacman -Suv --noconfirm

pacman -Ss "mingw-w64-x86_64-.*curl"

pacman -Sv --noconfirm --needed \
    autoconf base-devel bsdtar diffstat git patch tar vim \
    mingw-w64-x86_64-aria2 \
    mingw-w64-x86_64-binutils \
    mingw-w64-x86_64-ca-certificates \
    mingw-w64-x86_64-ccache \
    mingw-w64-x86_64-clang \
    mingw-w64-x86_64-cmake \
    mingw-w64-x86_64-curl \
    mingw-w64-x86_64-diffutils \
    mingw-w64-x86_64-gmp \
    mingw-w64-x86_64-mpc \
    mingw-w64-x86_64-mpfr \
    mingw-w64-x86_64-libmariadbclient \
    mingw-w64-x86_64-librdkafka \
    mingw-w64-x86_64-libtool \
    mingw-w64-x86_64-lld \
    mingw-w64-x86_64-llvm \
    mingw-w64-x86_64-make \
    mingw-w64-x86_64-ncurses \
    mingw-w64-x86_64-ninja \
    mingw-w64-x86_64-openssl \
    mingw-w64-x86_64-pcre \
    mingw-w64-x86_64-pcre2 \
    mingw-w64-x86_64-postgresql \
    mingw-w64-x86_64-python \
    mingw-w64-x86_64-tools-git \
    mingw-w64-x86_64-xz \
    mingw-w64-x86_64-zlib \
    mingw-w64-x86_64-zstd

pacman -Q
pacman -Ql mingw-w64-x86_64-llvm
pacman -Qo clang-format.exe

LC_ALL=C pacman -Qi \
    | awk '/^Name/{name=$3} /^Installed Size/{print $4$5, name}' \
    | sort -h

$ pacman -Scc --noconfirm
$ paccache -r -k 0
```

### Hello Stack

```shell
export https_proxy=http://example.com:8080

stack --version

stack config set system-ghc --global true
stack config set install-ghc --global false

stack update

stack --resolver lts --install-ghc exec -- ghc --version      [8.10.4]
stack --resolver lts-17 --install-ghc exec -- ghc --version   [8.10.4]
stack --resolver lts-16 --install-ghc exec -- ghc --version   [8.8.4]

stack setup 8.10.4

stack setup --system-ghc --resolver lts-17.7

stack build --copy-bins hindent

stack new hello-world simple
stack ghci
stack build
stack exec my-project

stack install --resolver lts hlint hindent

stack exec ghc-pkg -- list

ghc-pkg list
ghc-pkg dot
```

## The Stable Haskell package sets

- [Stable Haskell package sets](https://www.stackage.org/)
- [LTS Latest](https://www.stackage.org/lts)
- [LTS 17@2021-03-20, GHC 8.10.4, base 4.14.1, 2659 packages](https://www.stackage.org/lts-17)
- [LTS 16@2021-01-17, GHC 8.8.4, base 4.13.0, 2512 packages](https://www.stackage.org/lts-16)
- [LTS 14@2020-02-15, GHC 8.6.5, base 4.12.0, 2474 packages](https://www.stackage.org/lts-14)

## The Popular Haskell Packages

### Sites

- [Awesome Haskell - Konstantin](https://github.com/krispo/awesome-haskell)
- [Awesome Haskell - LibHunt](https://haskell.libhunt.com/)
- [Top 20 Haskell Projects by mentions](https://www.libhunt.com/l/haskell)
- [Packages by category](http://hackage.haskell.org/packages/)
- [Top Downloaded packages](https://hackage.haskell.org/packages/top)

### Standard Packages

```shell
/opt/ghc-8.10/lib/ghc-8.10.4/lib/package.conf.d
    Cabal-3.2.1.0
    array-0.5.4.0
    base-4.14.1.0
    binary-0.8.8.0
    bytestring-0.10.12.0
    containers-0.6.2.1
    deepseq-1.4.4.0
    directory-1.3.6.0
    exceptions-0.10.4
    filepath-1.4.2.1
    ghc-8.10.4
    ghc-boot-8.10.4
    ghc-boot-th-8.10.4
    ghc-compact-0.1.0.0
    ghc-heap-8.10.4
    ghc-prim-0.6.1
    ghci-8.10.4
    haskeline-0.8.0.1
    hpc-0.6.1.0
    integer-gmp-1.0.3.0
    libiserv-8.10.4
    mtl-2.2.2
    parsec-3.1.14.0
    pretty-1.1.3.6
    process-1.6.9.0
    rts-1.0
    stm-2.5.0.0
    template-haskell-2.16.0.0
    terminfo-0.4.1.4
    text-1.2.4.1
    time-1.9.3
    transformers-0.5.6.2
    unix-2.7.2.2
    xhtml-3000.2.2.1
```

### Packages

- [aeson](https://github.com/haskell/aeson): Fast JSON parsing and encoding
- [Alex](https://github.com/simonmar/alex): A lexical analyser generator for Haskell
- [async](https://github.com/simonmar/async): Run IO operations asynchronously and wait for their results
- [attoparsec](https://github.com/haskell/attoparsec): Fast combinator parsing for bytestrings and text
- [auto-update](https://github.com/yesodweb/wai): Efficiently run periodic, on-demand actions
- [base16-bytestring](https://github.com/haskell/base16-bytestring): Fast base16 (hexadecimal) encoding and decoding for Haskell bytestrings
- [base64-bytestring](https://github.com/haskell/base64-bytestring): Fast base64 encoding and decoding for Haskell
- [binary](https://github.com/kolmodin/binary): Binary serialisation for Haskell values using lazy ByteStrings
- [bytestring](https://github.com/haskell/bytestring): Fast, compact, strict and lazy byte strings with a list interface
- [conduit](https://github.com/snoyberg/conduit): Streaming data processing library
- [Conferer](https://github.com/ludat/conferer): Configuration managment for haskell
- [containers](https://github.com/haskell/containers): Assorted concrete container types
- [cql-io](https://gitlab.com/twittner/cql-io/): Cassandra CQL client
- [cql](https://gitlab.com/twittner/cql/): cql: Cassandra CQL binary protocol
- [criterion](https://github.com/haskell/criterion): Robust, reliable performance measurement and analysis
- [datadog](https://github.com/iand675/datadog): Haskell DataDog client library
- [deepseq](https://github.com/haskell/deepseq): Deep evaluation of data structures
- [directory](https://github.com/haskell/directory): Platform-agnostic library for filesystem operations
- [fgl](https://github.com/haskell/fgl): A Functional Graph Library for Haskell
- [filepath](https://github.com/haskell/filepath): Library for manipulating FilePaths in a cross platform way
- [gauge](https://github.com/vincenthz/hs-gauge): Lean Haskell Benchmarking
- [Happy](https://github.com/simonmar/happy): The Parser Generator for Haskell
- [Haxl](https://github.com/facebook/Haxl): Simplifies access to remote data.
- [hoopl](https://github.com/haskell/hoopl): Higher-order optimization library
- [hpack](https://github.com/sol/hpack): A modern format for Haskell packages
- [hsc2hs](https://github.com/haskell/hsc2hs): Haskell Pre-processor for C FFI bindings
- [hspec](https://github.com/hspec/hspec): A Testing Framework for Haskell
- [http-client-openssl](https://github.com/snoyberg/http-client): http-client backend using the OpenSSL library
- [http-client-tls](https://github.com/snoyberg/http-client): http-client backend using the connection package and tls library
- [http-client](https://github.com/snoyberg/http-client): An HTTP client engine
- [http-streams](https://github.com/aesiniath/http-streams): An HTTP client using io-streams
- [http2](https://github.com/kazu-yamamoto/http2): HTTP/2 library
- [hw-json](https://github.com/haskell-works/hw-json/): Memory efficient JSON parser
- [hw-simd](https://github.com/haskell-works/hw-simd): Library of simd functions
- [hw-xml](https://github.com/haskell-works/hw-xml): XML parser based on succinct data structures
- [inline-c](https://github.com/fpco/inline-c): Haskell and C can be freely intermixed in the same source file
- [iproute](http://github.com/kazu-yamamoto/iproute): A Simple Routing Table Structure for CIDR
- [lens](https://github.com/ekmett/lens/): Lenses, Folds and Traversals
- [math-functions](https://github.com/haskell/math-functions): Special mathematical functions
- [msgpack-binary](https://github.com/TokTok/hs-msgpack-binary): A Haskell implementation of MessagePack
- [mime-types](https://github.com/yesodweb/wai): Basic mime-type handling types and functions
- [mtl](https://github.com/haskell/mtl): The Monad Transformer Library
- [mwc-random](https://github.com/haskell/mwc-random): Fast, high quality pseudo random number generation
- [network-uri](https://github.com/haskell/network-uri): URI manipulation facilities
- [network](https://github.com/haskell/network): Low-level networking interface
- [odbc](https://github.com/fpco/odbc): Haskell binding to the ODBC API, aimed at SQL Server driver
- [optparse-applicative](https://github.com/pcapriotti/optparse-applicative): Command line options parsing
- [ormolu](ormolu): A formatter for Haskell source code
- [parallel](https://github.com/haskell/parallel): A library for parallel programming
- [pretty](https://github.com/haskell/pretty): Haskell Pretty-printer library
- [primitive](https://github.com/ekmett/semigroups/): Primitive memory-related operations
- [quickcheck](https://github.com/haskell/test-framework): Framework for running and organising tests, with HUnit and QuickCheck support
- [random](https://github.com/haskell/random): Pseudo-random number generation
- [req](https://github.com/mrkkrp/req): Easy-to-use, type-safe, expandable, high-level HTTP client library
- [rio](https://github.com/commercialhaskell/rio): A standard library for Haskell
- [safe-exceptions](https://github.com/fpco/safe-exceptions): safe-exceptions: Safe, consistent, and easy exception handling
- [serialise](https://github.com/well-typed/cborg): Binary serialisation in the CBOR format
- [servant-server](https://github.com/haskell-servant/servant): A family of combinators for defining webservices APIs and serving them
- [servant](https://github.com/haskell-servant/servant): A family of combinators for defining webservices APIs
- [snap-server](https://github.com/snapframework/snap-server): Snap Framework HTTP Server Library
- [socket](https://github.com/lpeterse/haskell-socket): POSIX sockets API
- [stm](https://github.com/haskell/stm): Software Transactional Memory
- [tar](https://github.com/haskell/tar): Reading, writing and manipulating ".tar" archive files
- [time-manager](https://github.com/yesodweb/wai): Scalable timer
- [time](https://github.com/haskell/time): A Time, clocks and calendars library
- [tls-session-manager](https://github.com/vincenthz/hs-tls): In-memory TLS session manager
- [tls](https://github.com/vincenthz/hs-tls): Native Haskell TLS and SSL protocol implementation for server and client
- [transformers](https://hackage.haskell.org/package/transformers): Monad transformers
- [typed-process](https://github.com/fpco/typed-process): Run external processes, with strong typing of streams
- [unliftio](https://github.com/fpco/unliftio): Lifting and unlifting IO actions
- [uuid](https://github.com/haskell-hvr/uuid): For creating, comparing, parsing and printing Universally Unique Identifiers
- [vector](https://github.com/haskell/vector): Efficient Arrays
- [wai-extra](http://github.com/yesodweb/wai): Provides some basic WAI handlers and middleware.
- [wai](https://github.com/yesodweb/wai): Web Application Interface
- [warp-quic](https://github.com/yesodweb/wai): WAI handler for HTTP/3 based on QUIC
- [weigh](https://github.com/fpco/weigh): Memory benchmarking
- [wrap](https://github.com/yesodweb/wai): A fast, light-weight web server for WAI applications
- [wreq](https://github.com/haskell/wreq): An easy-to-use HTTP client library
- [xhtml](https://github.com/haskell/xhtml): XHTML combinator library
- [yaml](https://github.com/snoyberg/yaml): Parsing and rendering YAML
- [zlib](https://github.com/haskell/zlib): Compression and decompression in the gzip and zlib formats
- [zstd](https://github.com/luispedro/hs-zstd): Haskell bindings to the Zstandard compression algorithm]

## Tricks to Fight Pain

### Static Linking

**Alpine Linux** use musl libc, it can generate statically-linked programs correctly, see [ghc-musl](https://github.com/utdemir/ghc-musl) for more details. **GNU C Library (glibc)** based distributions have this blocker issue [Wrong order of `-lc` and `-lpthread` when building a static binary](https://gitlab.haskell.org/ghc/ghc/-/issues/19029).

### ghc-pkg and cabal does not list user installed packages

After you install packages by cabal, you can not found them by `cabal list --installed`, [this issue](https://gitlab.haskell.org/ghc/ghc/-/issues/17341) had reported to upstream. Here is the workaround.

```shell
# ~/.ghc/x86_64-linux-8.10.4/environments/default
# ~/.cabal/store/ghc-8.10.4/package.db/package.cache

# use the specified package database
$ ghc-pkg list -f ~/.cabal/store/ghc-8.10.4/package.db

# fix the default package database
rm -fr ~/.ghc/x86_64-linux-8.10.4/environments/default
ln -s ~/.cabal/store/ghc-8.10.4/package.db ~/.ghc/x86_64-linux-8.10.4/package.conf.d

# Now it works !

$ cabal list --installed | grep '^\*'| sort -u | wc -l
259

$ ghc-pkg list | awk '{print $1}' | grep -E '^[a-zA-Z]+' | sort -u | wc -l
259

ghc-pkg field cql-io depends
ghc-pkg dot | tred | dot -Tpdf > pkgs.pdf
stack dot | dot -Tpng -o wreq.png
```

## GHC on Alpine Linux

### Prepare Alpine Linux

Both Docker or chroot are good for us.

#### Prepare by Docker

```shell
docker run --rm -it --pull always -w /root alpine:3
```

#### Prepare by Chroot

- [Alpine Linux 3.13 Minimal root filesystem](https://dl-cdn.alpinelinux.org/alpine/v3.13/releases/x86_64/alpine-minirootfs-3.13.2-x86_64.tar.gz)
- [Alpine Linux 3.12 Minimal root filesystem](https://dl-cdn.alpinelinux.org/alpine/v3.12/releases/x86_64/alpine-minirootfs-3.12.4-x86_64.tar.gz)
- [Alpine Linux 3.11 Minimal root filesystem](https://dl-cdn.alpinelinux.org/alpine/v3.11/releases/x86_64/alpine-minirootfs-3.11.8-x86_64.tar.gz)

```shell

export LFS=~/x86_64-alpine-linux
rm -fr ${LFS} && mkdir -p ${LFS} && \
cd ${LFS} && \
  curl -sSL https://dl-cdn.alpinelinux.org/alpine/v3.13/releases/x86_64/alpine-minirootfs-3.13.2-x86_64.tar.gz \
    | tar -xz

cp /etc/resolv.conf  $LFS/etc

mknod -m 600 $LFS/dev/console c 5 1
mknod -m 666 $LFS/dev/null c 1 3

mount -v --bind /dev $LFS/dev
mount -v --bind /dev/pts $LFS/dev/pts
mount -vt proc proc $LFS/proc
mount -vt sysfs sysfs $LFS/sys
mount -vt tmpfs tmpfs $LFS/run

cd $LFS/root && chroot "$LFS" /usr/bin/env -i   \
    HOME=/root                  \
    TERM="$TERM"                \
    PS1='\u:\w\$ ' \
    PATH=/opt/ghc-8.10/bin:/bin:/usr/bin:/sbin:/usr/sbin \
    /bin/sh --login
```

### Install Haskell

```shell
export PS1="\[\e[31m\][\[\e[m\]\[\e[38;5;172m\]\u\[\e[m\]@\[\e[38;5;153m\]\h\[\e[m\] \[\e[38;5;214m\]\W\[\e[m\]\[\e[31m\]]\[\e[m\]\\$ "
export PATH=/opt/ghc-8.10/bin:/bin:/usr/bin:/sbin:/usr/sbin

cd /root/
apk update && apk upgrade && \
apk add abuild autoconf automake binutils-gold build-base clang cmake coreutils \
    curl doxygen file g++ git gnupg grep lld llvm make python3 samurai sudo tar \
    vim wget xz \
    curl-dev curl-static expat-dev expat-static gmp-dev json-c-dev jsoncpp-dev \
    jsoncpp-static libdwarf-dev libdwarf-static libffi-dev librdkafka-dev \
    librdkafka-static libunwind-dev libunwind-static libxml2-dev mariadb-dev \
    mariadb-static musl-dev ncurses-dev ncurses-static numactl-dev openssl-dev \
    openssl-libs-static pcre-dev pcre2-dev postgresql-dev xz-dev yaml-dev \
    yaml-static zlib-dev zlib-static zstd-dev zstd-static

apk info -L musl-dev | sort

wget -q -O - https://downloads.haskell.org/~cabal/cabal-install-latest/cabal-install-3.4.0.0-x86_64-alpine-3.11.6-static-noofd.tar.xz \
  | tar -xvJ
mv cabal /usr/bin

wget -q -O - https://github.com/commercialhaskell/stack/releases/download/v2.5.1/stack-2.5.1-linux-x86_64.tar.gz \
  | tar -xz
mv stack-2.5.1-linux-x86_64/stack /usr/bin
rm -fr stack-2.5.1-linux-x86_64

wget -q -O - https://downloads.haskell.org/~ghc/8.10.4/ghc-8.10.4-x86_64-alpine3.10-linux-integer-simple.tar.xz \
  | tar -xJ

cd ghc-8.10.4-x86_64-unknown-linux/
./configure --prefix=/opt/ghc-8.10
make install && cd .. && rm -fr ghc-8.10.4-x86_64-unknown-linux/

# export PATH=/opt/ghc-9.0/bin:/bin:/usr/bin:/sbin:/usr/sbin
#
# wget -q -O - https://downloads.haskell.org/~ghc/9.0.1/ghc-9.0.1-x86_64-alpine3.10-linux-integer-simple.tar.xz \
#   | tar -xJ

# cd ghc-9.0.1-x86_64-unknown-linux/
# ./configure --prefix=/opt/ghc-9.0
# make install && cd .. && rm -fr ghc-9.0.1-x86_64-unknown-linux/
```

### Using Haskell

```shell
$ ghc --version
$ ghc --show-options
$ ghc --info

$ cat << EOF > HelloWorld.hs
module Main where
main = putStrLn "Hello, World!"
EOF

$ rm -f HelloWorld HelloWorld.hi HelloWorld.o && \
    ghc HelloWorld.hs -O2 -optl-s

$ ./HelloWorld
Hello, World !

$ du -ks HelloWorld
5060    HelloWorld

$ file HelloWorld
HelloWorld: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked,
interpreter /lib/ld-musl-x86_64.so.1, stripped

$ ldd HelloWorld
        /lib/ld-musl-x86_64.so.1 (0x7f9320ec0000)
        libgmp.so.10 => /usr/lib/libgmp.so.10 (0x7f9320e5a000)
        libc.musl-x86_64.so.1 => /lib/ld-musl-x86_64.so.1 (0x7f9320ec0000)

$ rm -f HelloWorld HelloWorld.hi HelloWorld.o && \
    ghc HelloWorld.hs -O2 -optl-s -optc-static -optl-static

$ ./HelloWorld
Hello, World !

$ du -ks HelloWorld
5472    HelloWorld

$ file HelloWorld
HelloWorld: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, stripped

$ du -ms / /opt/ghc-8.10 /opt/ghc-8.10/*
1488    /
1249    /opt/ghc-8.10
1       /opt/ghc-8.10/bin
1       /opt/ghc-8.10/include
1248    /opt/ghc-8.10/lib
1       /opt/ghc-8.10/mingw
1       /opt/ghc-8.10/share

$ du -ms / /opt/ghc-9.0 /opt/ghc-9.0/*
1972    .
1733    /opt/ghc-9.0
1       /opt/ghc-9.0/bin
1       /opt/ghc-9.0/include
1224    /opt/ghc-9.0/lib
509     /opt/ghc-9.0/share
```

### Cleanup Alpine Linux

This just need for the chroot setup.

```shell
umount --recursive $LFS/run
umount --recursive $LFS/sys
umount --recursive $LFS/proc
umount --recursive $LFS/dev/pts
umount --recursive $LFS/dev
```

## GHC on SUSE Linux 15

### Install Haskell

```shell
export PS1="\[\e[31m\][\[\e[m\]\[\e[38;5;172m\]\u\[\e[m\]@\[\e[38;5;153m\]\h\[\e[m\] \[\e[38;5;214m\]\W\[\e[m\]\[\e[31m\]]\[\e[m\]\\$ "
export PATH=/opt/ghc-8.10/bin:/bin:/usr/bin:/sbin:/usr/sbin

cd /root/

curl -sSLO http://ftp.debian.org/debian/pool/main/n/ncurses/libtinfo5_6.1+20181013-2+deb10u2_amd64.deb
ar libtinfo5_6.1+20181013-2+deb10u2_amd64.deb
cp -af lib/x86_64-linux-gnu/libtinfo.so.5* /lib64/

zypper install -y glibc-devel gmp-devel libopenssl-devel libcurl-devel \
  zlib-devel libzstd-devel xz-devel ncurses-devel pcre-devel pcre2-devel

curl -sSL https://downloads.haskell.org/~cabal/cabal-install-3.4.0.0/cabal-install-3.4.0.0-x86_64-alpine-3.11.6-static-noofd.tar.xz \
  | tar -xJ
mv cabal /usr/bin

curl -sSL https://github.com/commercialhaskell/stack/releases/download/v2.5.1/stack-2.5.1-linux-x86_64.tar.gz \
  | tar -xz
mv stack-2.5.1-linux-x86_64/stack /usr/bin
rm -fr stack-2.5.1-linux-x86_64

curl -sSL https://downloads.haskell.org/~ghc/8.10.4/ghc-8.10.4-x86_64-deb9-linux.tar.xz \
  | tar -xJ

cd ghc-8.10.4/
CC=gcc-10 ./configure --prefix=/opt/ghc-8.10
make install && cd .. && rm -fr ghc-8.10.4/

# export PATH=/opt/ghc-9.0/bin:/bin:/usr/bin:/sbin:/usr/sbin
#
# curl -sSL https://downloads.haskell.org/~ghc/9.0.1/ghc-9.0.1-x86_64-deb9-linux.tar.xz \
#   | tar -xJ

# cd ghc-9.0.1/
# CC=gcc-10 ./configure --prefix=/opt/ghc-9.0
# make install && cd .. && rm -fr ghc-9.0.1/
```

### Using Haskell

```shell
$ ghc --version
$ ghc --show-options
$ ghc --info

$ cat << EOF > HelloWorld.hs
module Main where
main = putStrLn "Hello, World!"
EOF

$ rm -f HelloWorld HelloWorld.hi HelloWorld.o && \
  ghc HelloWorld.hs -O2 -optl-s

$ ./HelloWorld
Hello, World !

$ du -ks HelloWorld
732     HelloWorld  [ghc 8.10.4]
744     HelloWorld  [ghc 9.0.1]

$ file HelloWorld
HelloWorld: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked,
interpreter /lib64/ld-linux-x86-64.so.2,
BuildID[sha1]=9f1540564ef645a459ee1e01dfd6e9b207c979c5, for GNU/Linux 3.2.0, stripped

$ ldd HelloWorld
        linux-vdso.so.1 (0x00007ffff93f9000)
        libgmp.so.10 => /usr/lib64/libgmp.so.10 (0x00007f69d66c0000)
        libc.so.6 => /lib64/libc.so.6 (0x00007f69d6305000)
        libm.so.6 => /lib64/libm.so.6 (0x00007f69d5fcd000)
        librt.so.1 => /lib64/librt.so.1 (0x00007f69d5dc5000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007f69d5bc1000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f69d59a2000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f69d6956000)

# # /usr/bin/ld.bfd: error: multiple definition of `__lll_lock_wait_private'
$ rm -f HelloWorld HelloWorld.hi HelloWorld.o && \
    ghc HelloWorld.hs -O2 -optl-s -optc-static -optl-static

$ du -ms /opt/ghc-8.10 /opt/ghc-8.10/*
2081    /opt/ghc-8.10
1       /opt/ghc-8.10/bin
1795    /opt/ghc-8.10/lib
287     /opt/ghc-8.10/share

$ du -ms /opt/ghc-9.0 /opt/ghc-9.0/*
2077    /opt/ghc-9.0
1       /opt/ghc-9.0/bin
1776    /opt/ghc-9.0/lib
302     /opt/ghc-9.0/share
```

## GHC on RHEL 8

### Install Haskell

```shell
export PS1="\[\e[31m\][\[\e[m\]\[\e[38;5;172m\]\u\[\e[m\]@\[\e[38;5;153m\]\h\[\e[m\] \[\e[38;5;214m\]\W\[\e[m\]\[\e[31m\]]\[\e[m\]\\$ "
export PATH=/opt/ghc-8.10/bin:/bin:/usr/bin:/sbin:/usr/sbin

cd /root/

sudo dnf install -y glibc-devel gmp-devel openssl-devel curl-devel \
  zlib-devel libzstd-devel xz-devel ncurses-devel pcre-devel pcre2-devel

curl -sSL https://downloads.haskell.org/~cabal/cabal-install-3.4.0.0/cabal-install-3.4.0.0-x86_64-alpine-3.11.6-static-noofd.tar.xz \
  | tar -xJ
mv cabal /usr/bin

curl -sSL https://github.com/commercialhaskell/stack/releases/download/v2.5.1/stack-2.5.1-linux-x86_64.tar.gz \
  | tar -xz
mv stack-2.5.1-linux-x86_64/stack /usr/bin
rm -fr stack-2.5.1-linux-x86_64

curl -sSL https://downloads.haskell.org/~ghc/8.10.4/ghc-8.10.4-x86_64-deb10-linux.tar.xz \
  | tar -xJ

cd ghc-8.10.4/
./configure --prefix=/opt/ghc-8.10
make install && cd .. && rm -fr ghc-8.10.4/

# export PATH=/opt/ghc-9.0/bin:/bin:/usr/bin:/sbin:/usr/sbin
#
# curl -sSL https://downloads.haskell.org/~ghc/9.0.1/ghc-9.0.1-x86_64-deb10-linux.tar.xz \
#   | tar -xJ

# cd ghc-9.0.1/
# ./configure --prefix=/opt/ghc-9.0
# make install && cd .. && rm -fr ghc-9.0.1/
```

### Using Haskell

```shell
$ ghc --version
$ ghc --show-options
$ ghc --info

$ cat << EOF > HelloWorld.hs
module Main where
main = putStrLn "Hello, World!"
EOF

$ rm -f HelloWorld HelloWorld.hi HelloWorld.o && \
  ghc HelloWorld.hs -O2 -optl-s

$ ./HelloWorld
Hello, World !

$ du -ks HelloWorld
752     HelloWorld  [ghc 8.10.4]
764     HelloWorld  [ghc 9.0.1]

$ file HelloWorld
HelloWorld: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked,
interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0,
BuildID[sha1]=4f23f2d113a7e64e9933897e0d1f21277b84496b, stripped

$ ldd HelloWorld
        linux-vdso.so.1 (0x00007ffc949f2000)
        libgmp.so.10 => /lib64/libgmp.so.10 (0x00007f2a4d368000)
        libc.so.6 => /lib64/libc.so.6 (0x00007f2a4cfa5000)
        libm.so.6 => /lib64/libm.so.6 (0x00007f2a4cc23000)
        librt.so.1 => /lib64/librt.so.1 (0x00007f2a4ca1b000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007f2a4c817000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f2a4c5f7000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f2a4d600000)

# # /usr/bin/ld.bfd: error: multiple definition of `__lll_lock_wait_private'
$ rm -f HelloWorld HelloWorld.hi HelloWorld.o && \
    ghc HelloWorld.hs -O2 -optl-s -optc-static -optl-static

$ du -ms /opt/ghc-8.10 /opt/ghc-8.10/*
2091    /opt/ghc-8.10
1       /opt/ghc-8.10/bin
1805    /opt/ghc-8.10/lib
287     /opt/ghc-8.10/share

$ du -ms /opt/ghc-9.0 /opt/ghc-9.0/*
2103    /opt/ghc-9.0
1       /opt/ghc-9.0/bin
1801    /opt/ghc-9.0/lib
303     /opt/ghc-9.0/share
```

## Program Structure

Every complete Haskell program must define main in module Main in package main.

1. At the topmost level a Haskell program is a set of modules. Modules provide a way to control namespaces and to re-use software in large programs.

2. The top level of a module consists of a collection of declarations, of which there are several kinds. Declarations define things such as ordinary values, datatypes, type classes, and fixity information.

3. At the next lower level are expressions. An expression denotes a value and has a static type; expressions are at the heart of Haskell programming “in the small.”

4. At the bottom level is Haskell’s lexical structure. The lexical structure captures the concrete representation of Haskell programs in text files.

## Language extensions

50% of all production Haskell code is language extension and import lines

```haskell
{-# LANGUAGE OverloadedStrings #-}
{-# LANGUAGE DeriveFunctor #-}
{-# LANGUAGE GADTs #-}
```

Added at the top of your file. Basic structure:

```haskell
#!/usr/bin/env stack
-- stack --resolver lts-16.31 script

{-# LANGUAGE OverloadedStrings #-}
{-# LANGUAGE DeriveFunctor #-}
{-# LANGUAGE GADTs #-}
{-# LANGUAGE LambdaCase #-}

module Main (main) where

import Control.Monad (when)
-- import ...

main :: IO ()
main = putStrLn "Finally, some actual code!"
```

## Modules

A module defines a collection of values, datatypes, type synonyms, classes, etc., in an environment created by a set of imports (resources brought into scope from other modules). It exports some of these resources, making them available to other modules. We use the term entity to refer to a value, type, or class defined in, imported into, or perhaps exported from a module.

A Haskell program is a collection of modules, one of which, by convention, must be called Main and must export the value main. The value of the program is the value of the identifier main in module Main, which must be a computation of type IO τ for some type τ. When the program is executed, the computation main is performed, and its result (of type τ) is discarded.

### Module Structure

A module defines a mutually recursive scope containing declarations for value bindings, data types, type synonyms, classes, etc.

```
module  →  module modid [exports] where body
 |  body
body  →  { impdecls ; topdecls }
 |  { impdecls }
 |  { topdecls }

impdecls  →  impdecl1 ; … ; impdecln      (n ≥ 1)
topdecls  →  topdecl1 ; … ; topdecln      (n ≥ 1)
```

A module begins with a header: the keyword module, the module name, and a list of entities (enclosed in round parentheses) to be exported. The header is followed by a possibly-empty list of import declarations that specify modules to be imported, optionally restricting the imported bindings. This is followed by a possibly-empty list of top-level declarations.

An abbreviated form of module, consisting only of the module body, is permitted. If this is used, the header is assumed to be ‘**module Main(main) where**’. If the first lexeme in the abbreviated module is not a {, then the layout rule applies for the top level of the module.

### Export Lists

```
exports  →  ( export1 , … , exportn [ , ] )      (n ≥ 0)

export  →  qvar
 |  qtycon [(..) | ( cname1 , … , cnamen )]      (n ≥ 0)
 |  qtycls [(..) | ( var1 , … , varn )]      (n ≥ 0)
 |  module modid

cname  →  var | con
```

An export list identifies the entities to be exported by a module declaration. A module implementation may only export an entity that it declares, or that it imports from some other module. If the export list is omitted, all values, types and classes defined in the module are exported, but not those that are imported.

### Import Declarations

```
impdecl  →  import [qualified] modid [as modid] [impspec]
 |       (empty declaration)
impspec  →  ( import1 , … , importn [ , ] )      (n ≥ 0)
 |  hiding ( import1 , … , importn [ , ] )      (n ≥ 0)

import  →  var
 |  tycon [ (..) | ( cname1 , … , cnamen )]      (n ≥ 0)
 |  tycls [(..) | ( var1 , … , varn )]      (n ≥ 0)
cname  →  var | con
```

The entities exported by a module may be brought into scope in another module with an import declaration at the beginning of the module. The import declaration names the module to be imported and optionally specifies the entities to be imported. A single module may be imported by more than one import declaration. Imported names serve as top level declarations: they scope over the entire body of the module but may be shadowed by local non-top-level bindings.

The effect of multiple import declarations is strictly cumulative: an entity is in scope if it is imported by any of the import declarations in a module. The ordering of import declarations is irrelevant.

Lexically, the terminal symbols “as”, “qualified” and “hiding” are each a varid rather than a reservedid. They have special significance only in the context of an import declaration; they may also be used as variables.

### What is imported

Exactly which entities are to be imported can be specified in one of the following three ways:

  1. The imported entities can be specified explicitly by listing them in parentheses. Items in the list have the same form as those in export lists, except qualifiers are not permitted and the ‘module modid’ entity is not permitted. When the (..) form of import is used for a type or class, the (..) refers to all of the constructors, methods, or field names exported from the module.

  The list must name only entities exported by the imported module. The list may be empty, in which case nothing except the instances is imported.

  2. Entities can be excluded by using the form hiding(import1 , … , importn ), which specifies that all entities exported by the named module should be imported except for those named in the list. Data constructors may be named directly in hiding lists without being prefixed by the associated type. Thus, in `import M hiding (C)` any constructor, class, or type named C is excluded. In contrast, using C in an import list names only a class or type.

  It is an error to hide an entity that is not, in fact, exported by the imported module.

  3. Finally, if impspec is omitted then all the entities exported by the specified module are imported.

## Lexical Structure

- Notational Conventions
- Lexical Program Structure
- Comments
- Identifiers and Operators
- Numeric Literals
- Character and String Literals
- Layout

## Expressions

- Errors
- Variables, Constructors, Operators, and Literals
- Curried Applications and Lambda Abstractions
- Operator Applications
- Sections
- Conditionals
- Lists
- Tuples
- Unit Expressions and Parenthesized Expressions
- Arithmetic Sequences
- List Comprehensions
- Let Expressions
- Case Expressions
- Do Expressions
- Datatypes with Field Labels
- Expression Type-Signatures
- Pattern Matching

## Predefined Types and Classes

- Standard Haskell Types
- Strict Evaluation
- Standard Haskell Classes
- Numbers

### Standard Haskell Types

#### Booleans

```
data  Bool  =  False | True deriving
                             (Read, Show, Eq, Ord, Enum, Bounded)
```

The boolean type **Bool** is an enumeration. The basic boolean functions are && (and), || (or), and not. The name **otherwise** is defined as **True** to make guarded expressions more readable.

#### Characters and Strings

The character type **Char** is an enumeration whose values represent Unicode characters. Type Char is an instance of the classes Read, Show, Eq, Ord, Enum, and Bounded. The toEnum and fromEnum functions, standard functions from class Enum, map characters to and from the Int type.

A **string** is a list of characters:

```
type  String  =  [Char]
```

#### Lists

```
data  [a]  =  [] | a : [a]  deriving (Eq, Ord)
```

Lists are an algebraic datatype of two constructors, although with special syntax. Lists are an instance of classes Read, Show, Eq, Ord, Monad, Functor, and MonadPlus.

#### Tuples

Tuples are algebraic datatypes with special syntax. Each tuple type has a single constructor. All tuples are instances of Eq, Ord, Bounded, Read, and Show (provided, of course, that all their component types are).

Haskell implementation must support tuples up to size 15, together with the instances for Eq, Ord, Bounded, Read, and Show. The Prelude and libraries define tuple functions such as zip for tuples up to a size of 7.

#### The Unit Datatype

```
data  () = () deriving (Eq, Ord, Bounded, Enum, Read, Show)
```

The unit datatype () has one non-⊥ member, the nullary constructor ().

#### Function Types

Functions are an abstract type: no constructors directly create functional values. The following simple functions are found in the Prelude: id, const, (.), flip, ($), and until.

#### The IO and IOError Types

The IO type serves as a tag for operations (actions) that interact with the outside world. The IO type is abstract: no constructors are visible to the user. IO is an instance of the Monad and Functor classes.

IOError is an abstract type representing errors raised by I/O operations. It is an instance of Show and Eq.

#### Other Types

```
data  Maybe a     =  Nothing | Just a  deriving (Eq, Ord, Read, Show)
data  Either a b  =  Left a | Right b  deriving (Eq, Ord, Read, Show)
data  Ordering    =  LT | EQ | GT deriving
                                  (Eq, Ord, Bounded, Enum, Read, Show)
```

The Maybe type is an instance of classes Functor, Monad, and MonadPlus. The Ordering type is used by compare in the class Ord. The functions maybe and either are found in the Prelude.

### Standard Haskell Classes

- Eq
- Ord
- Read and Show
- Enum
- Functor
- Monad
- Bounded
- Numbers
  - Integer (Integral)
  - Int [−2^29, 2^29−1] (Integral)
  - Float (RealFloat)
  - Double (RealFloat)
  - The class **Integral** contains integers of both limited and unlimited range; the class **Fractional** contains all non-integral types; and the class **Floating** contains all floating-point types, both real and complex.

## Basic Input/Output

### Standard I/O Functions

#### Output Functions

These functions write to the standard output device (this is normally the user’s terminal).

```

  putChar  :: Char -> IO ()
  putStr   :: String -> IO ()
  putStrLn :: String -> IO ()  -- adds a newline
  print    :: Show a => a -> IO ()
```

#### Input Functions

These functions read input from the standard input device (normally the user’s terminal).

```
  getChar     :: IO Char
  getLine     :: IO String
  getContents :: IO String
  interact    :: (String -> String) -> IO ()
  readIO      :: Read a => String -> IO a
  readLn      :: Read a => IO a
```

#### Files

These functions operate on files of characters. Files are named by strings using some implementation-specific method to resolve strings as file names.

The writeFile and appendFile functions write or append the string, their second argument, to the file, their first argument. The readFile function reads a file and returns the contents of the file as a string. The file is read lazily, on demand, as with getContents.

```
  type FilePath =  String

  writeFile  :: FilePath -> String -> IO ()
  appendFile :: FilePath -> String -> IO ()
  readFile   :: FilePath           -> IO String
```

### Sequencing I/O Operations

The type constructor IO is an instance of the Monad class. The two monadic binding functions, methods in the Monad class, are used to compose a series of I/O operations. The >> function is used where the result of the first operation is uninteresting, for example when it is (). The >>= operation passes the result of the first operation as an argument to the second operation.

```
  (>>=) :: IO a -> (a -> IO b) -> IO b
  (>>)  :: IO a -> IO b        -> IO b
```

### Exception Handling in the I/O Monad

The I/O monad includes a simple exception handling system. Any I/O operation may raise an exception instead of returning a result.

Exceptions in the I/O monad are represented by values of type IOError. This is an abstract type: its constructors are hidden from the user. The IO library defines functions that construct and examine IOError values. The only Prelude function that creates an IOError value is userError. User error values include a string describing the error.

```
  userError :: String -> IOError
```

Exceptions are raised and caught using the following functions:

```
  ioError :: IOError -> IO a
  catch   :: IO a    -> (IOError -> IO a) -> IO a
```

## Foreign Function Interface

- Foreign Languages
- Contexts
- Lexical Structure
- Foreign Declarations
  - Calling Conventions
  - Foreign Types
    - Char, Int, Double, Float, and Bool as exported by the Haskell Prelude as well as
    - Int8, Int16, Int32, Int64, Word8, Word16, Word32, Word64, Ptr a, FunPtr a, and StablePtr a, for any type a, as exported by Foreign
  - Import Declarations
  - Export Declarations
- Specification of External Entities
  - Standard C Calls
  - Win32 API Calls
- Marshalling
- The External C Interface

## Compiler Pragmas

- Inlining
- Specialization
- Language extensions
