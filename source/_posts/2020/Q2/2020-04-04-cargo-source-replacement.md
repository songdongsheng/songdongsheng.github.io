---
title: Cargo Source Replacement
description: Cargo Source Replacement
date: 2020-04-04 16:30:07
tags:
  - Programming
  - Rust
categories: [Programming, Rust]
permalink: cargo-source-replacement
---

# Cargo Source Replacement

## Environment variables and configurations

- https://doc.rust-lang.org/cargo/reference/config.html
- https://doc.rust-lang.org/cargo/reference/environment-variables.html
- CARGO\_HOME: ~/.rustup or %USERPROFILE%/.rustup
- CARGO\_HTTP\_MULTIPLEXING: true
- CARGO\_HTTP\_SSL\_VERSION: tlsv1.3
- CARGO\_HTTP\_PROXY: socks5h://proxy.example.com | http://proxy.example.com:80
- CARGO\_HTTP\_TIMEOUT: 10
- CARGO\_NET\_OFFLINE: false
- RUSTUP\_DIST\_SERVER: https://static.rust-lang.org
- RUSTUP\_UPDATE\_ROOT: https://static.rust-lang.org/rustup
- RUSTUP\_TOOLCHAIN: stable | beta | nightly
- RUSTUP\_IO\_THREADS: 4

## Rustup profiles

Rustup profiles are groups of components you can choose to download while installing a new Rust toolchain. The profiles available at this time are **minimal**, **default**, and **complete**:

- The **minimal** profile includes as few components as possible to get a working compiler (**rustc**, **rust-std**, and **cargo**). It's recommended to use this component on Windows systems if you don't use local documentation, and in CI.
- The **default** profile includes all the components previously installed by default (rustc, rust-std, cargo, and rust-docs) plus **rustfmt** and **clippy**. This profile will be used by rustup by default, and it's the one recommended for general use.
- The **complete** profile includes all the components available through rustup, including **miri** and IDE integration tools (**rls** and **rust-analysis**).

To change the rustup profile you can use the rustup set profile command. For example, to select the minimal profile you can use:

```bash
rustup set profile minimal
```

## Rustup components history

- https://rust-lang.github.io/rustup-components-history/
- https://rust-lang.github.io/rustup-components-history/x86_64-unknown-linux-gnu.html
- https://rust-lang.github.io/rustup-components-history/x86_64-pc-windows-msvc.html
- https://rust-lang.github.io/rustup-components-history/x86_64-unknown-freebsd.html
- https://rust-lang.github.io/rustup-components-history/x86_64-unknown-netbsd.html
- https://rust-lang.github.io/rustup-components-history/x86_64-apple-darwin.html
- https://rust-lang.github.io/rustup-components-history/s390x-unknown-linux-gnu.html
- https://rust-lang.github.io/rustup-components-history/riscv64gc-unknown-linux-gnu.html

## Rustup installation

```bash
# curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs \
    | sh -s -- --profile minimal --default-toolchain nightly

# export CARGO_HTTP_PROXY="socks5h://proxy.example.com"
# export CARGO_HOME=/opt/rust
# export CARGO_HTTP_MULTIPLEXING=true

$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

```PowerShell
$url = "https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe"
$output = "rustup-init-x86_64-pc-windows-msvc.exe"
$start_time = Get-Date

Invoke-WebRequest -Uri $url -OutFile $output
Write-Output "Time taken: $((Get-Date).Subtract($start_time).Seconds) second(s)"
```

```PowerShell
$url = "https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe"
$output = "rustup-init-x86_64-pc-windows-msvc.exe"
$start_time = Get-Date

(New-Object System.Net.WebClient).DownloadFile($url, $output)

Write-Output "Time taken: $((Get-Date).Subtract($start_time).Seconds) second(s)"

```

```PowerShell
$url = "https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe"
$output = "rustup-init-x86_64-pc-windows-msvc.exe"
$start_time = Get-Date

Import-Module BitsTransfer
Start-BitsTransfer -Source $url -Destination $output
# Start-BitsTransfer -Source $url -Destination $output -Asynchronous

Write-Output "Time taken: $((Get-Date).Subtract($start_time).Seconds) second(s)"
```

## Build offline

```bash
cargo fetch --target x86_64-unknown-linux-gnu --verbose

cargo build --release --offline --frozen --target x86_64-unknown-linux-gnu --verbose
```

```bash
cargo vendor

cargo build --release --offline --frozen --target x86_64-unknown-linux-gnu --verbose
```

## Overriding dependencies

The desire to [override a dependency](https://doc.rust-lang.org/cargo/reference/overriding-dependencies.html) can arise through a number of scenarios. Most of them, however, boil down to the ability to work with a crate before it's been published to [crates.io](https://crates.io/). For example:

- A crate you're working on is also used in a much larger application you're working on, and you'd like to test a bug fix to the library inside of the larger application.
- An upstream crate you don't work on has a new feature or a bug fix on the master branch of its git repository which you'd like to test out.
- You're about to publish a new major version of your crate, but you'd like to do integration testing across an entire package to ensure the new major version works.
- You've submitted a fix to an upstream crate for a bug you found, but you'd like to immediately have your application start depending on the fixed version of the crate to avoid blocking on the bug fix getting merged.

These scenarios can be solved with the [[patch] manifest section](https://doc.rust-lang.org/cargo/reference/overriding-dependencies.html#the-patch-section).

Let's say you're working with the [uuid crate](https://crates.io/crates/uuid) but while you're working on it you discover a bug. You are, however, quite enterprising so you decide to also try to fix the bug!

```ini
[package]
name = "my-library"
version = "0.1.0"
authors = ["..."]

[dependencies]
uuid = "1.0"

[patch.crates-io]
uuid = { path = "../path/to/uuid" }
```

If you are working with an unpublished minor version, just edit **Cargo.toml**:

```ini
[package]
name = "my-library"
version = "0.1.0"
authors = ["..."]

[dependencies]
uuid = "1.0"

[patch.crates-io]
uuid = { git = "https://github.com/uuid-rs/uuid", branch = "2.0.0" }
```

In case the dependency you want to override isn't loaded from [crates.io](https://crates.io/), you'll have to change a bit how you use **[patch]**. For example, if the dependency is a git dependency, you can override it to a local path with:

```ini
[patch."https://github.com/your/repository"]
my-library = { path = "../my-library/path" }
```

## Using an alternate registry

To use a [registry](https://doc.rust-lang.org/cargo/reference/registries.html) other than [crates.io](https://crates.io/), the name and index URL of the registry must be added to a **.cargo/config** file. The registries table has a key for each registry, for example:

```ini
[registries]
my-registry = { index = "https://my-intranet:8080/git/index" }
```

The **index** key should be a URL to a git repository with the registry's index. A crate can then depend on a crate from another registry by specifying the **registry** key and a value of the registry's name in that dependency's entry in **Cargo.toml**:

```ini
[package]
name = "my-project"
version = "0.1.0"

[dependencies]
other-crate = { version = "1.0", registry = "my-registry" }
```

## Source replacement

This document is about [replacing the crate index](https://doc.rust-lang.org/cargo/reference/source-replacement.html). You can read about overriding dependencies in the [overriding dependencies](https://doc.rust-lang.org/cargo/reference/overriding-dependencies.html) section of this documentation.

A source is a provider that contains crates that may be included as dependencies for a package. Cargo supports the ability to **replace one source** with another to express strategies such as:

- Vendoring - custom sources can be defined which represent crates on the local filesystem. These sources are subsets of the source that they're replacing and can be checked into packages if necessary.

- Mirroring - sources can be replaced with an equivalent version which acts as a cache for crates.io itself.

Cargo has a core assumption about source replacement that the source code is exactly the same from both sources. Note that this also means that a replacement source is not allowed to have crates which are not present in the original source.

As a consequence, source replacement is not appropriate for situations such as patching a dependency or a private registry. Cargo supports patching dependencies through the usage of [the [patch] key](https://doc.rust-lang.org/cargo/reference/overriding-dependencies.html), and private registry support is described in [the Registries chapter](https://doc.rust-lang.org/cargo/reference/registries.html).

Configuration of replacement sources is done through **.cargo/config** and the full set of available keys are:

```ini
[source]

[source.my-vendor-source]
directory = "vendor"

[source.crates-io]
replace-with = "my-vendor-source"

# Each source has its own table where the key is the name of the source
[source.the-source-name]

# Indicate that `the-source-name` will be replaced with `another-source`,
# defined elsewhere
replace-with = "another-source"

# Several kinds of sources can be specified (described in more detail below):
registry = "https://example.com/path/to/index"
local-registry = "path/to/registry"
directory = "path/to/vendor"

# Git sources can optionally specify a branch/tag/rev as well
git = "https://example.com/path/to/repo"
# branch = "master"
# tag = "v1.0.1"
# rev = "313f44e8"
```

## Vendor all dependencies for a project locally

This cargo subcommand will vendor all crates.io and git dependencies for a project into the specified directory at **<path>**. After this command completes the vendor directory specified by **<path>** will contain all remote sources from dependencies specified. Additional manifests beyond the default one can be specified with the **-s** option.

The **cargo vendor** command will also print out the configuration necessary to use the vendored sources, which you will need to add to **.cargo/config**.

```bash
cargo vendor [OPTIONS] [PATH]
```

## Prefetch popular dependencies

```bash
cargo install cargo-prefetch
cargo prefetch
cargo prefetch --list

cargo prefetch serde
cargo prefetch serde@=1.0.90

cargo prefetch --top-downloads=400
```

## Find out what takes most of the space in your executable.

```bash
cargo install cargo-bloat
cargo install cargo-bloat --features regex-filter

# Get a list of the biggest functions in the release build
% cargo bloat --release -n 10

# Get a list of the biggest dependencies in the release build:
% cargo bloat --release --crates

# Get a list of the biggest functions in the release build filtered by the regexp:
# Note: you have to build cargo-bloat with a regex-filter feature enabled.
% cargo bloat --release --filter '^__' -n 10
```

## Building dependency graphs

Dependencies are colored depending on their kind:

- Black: regular dependency
- Purple: build dependency
- Blue: dev dependency
- Red: optional dependency

```bash
cargo install cargo-deps
cargo deps | dot -Tpng > graph.png

# The default behavior is to exclude optional, dev, and build dependencies. To see all dependencies, pass --all-deps:
cargo deps --all-deps | dot -Tpng > graph.png
```
