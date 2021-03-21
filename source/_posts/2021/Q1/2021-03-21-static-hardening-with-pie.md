---
title: Statically Linked Executable Hardening with PIE
description: Statically Linked Executable Hardening with PIE
date: 2021-03-21 23:17:14
tags:
    - Programming
    - C/C++
    - Linux
categories: [Programming, C/C++]
permalink: static-hardening-with-pie
---

# Statically Linked Executable Hardening with PIE

## In a nutshell

Target triplets                 | static-pie    | pie   | Linux         | glibc
--------------------------------|---------------|-------|---------------|----------
aarch64-linux-gnu-gcc           | OK            | OK    | Linux 3.7     | GLIBC_2.17
arm-linux-gnueabihf-gcc         | rcrt1.o       | OK    | Linux 3.2     | GLIBC_2.4
mips64-linux-gnuabi64-gcc       | rcrt1.o       | OK    | Linux 3.2     | GLIBC_2.2
mipsisa64r6-linux-gnuabi64-gcc  | rcrt1.o       | OK    | Linux 4.5     | GLIBC_2.2
riscv64-linux-gnu-gcc           | rcrt1.o       | OK    | Linux 4.15    | GLIBC_2.27
s390x-linux-gnu-gcc             | rcrt1.o       | OK    | Linux 3.2     | GLIBC_2.2
x86\_64-linux-gnu-gcc            | OK            | OK    | Linux 3.2     | GLIBC_2.2.5

## Hello World

```C
$ cat << EOF > HelloWorld.c
# include <stdio.h>

int main()
{
    printf("Hello, World!\n");
    return 0;
}
EOF
```

## Static PIE Generation

```bash
$ export HARDENING_FLAGS=" -D_FORTIFY_SOURCE=2 -D_GLIBCXX_ASSERTIONS -Wall -Wextra -Wformat=2 -Wdate-time -Werror=format-security -Wstack-protector -fasynchronous-unwind-tables -fexceptions -fstack-protector-strong -fstack-clash-protection -pedantic -Wl,-z,noexecstack -Wl,-z,now -Wl,-z,relro -Wl,-z,defs -fPIE"

$ x86_64-linux-gnu-gcc -O2 -march=haswell -mavx2 -std=c11 ${HARDENING_FLAGS} -pie -s -o HelloWorld HelloWorld.c

$ x86_64-linux-gnu-gcc -O2 -march=haswell -mavx2 -std=c11 ${HARDENING_FLAGS} --static-pie -s -o HelloWorld HelloWorld.c

$ file HelloWorld
HelloWorld: ELF 64-bit LSB pie executable, UCB RISC-V, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-riscv64-lp64d.so.1, BuildID[sha1]=b9f6b891213352dc1595c34538bc6974dd59465d, for GNU/Linux 4.15.0, stripped

$ file HelloWorld
HelloWorld: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, BuildID[sha1]=4aa140be9886b18dc6e8cfe969f0c62221dd5ddb, for GNU/Linux 3.2.0, stripped

$ file HelloWorld
HelloWorld: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=ce5c70eefd3813ae25cbb405af8399d689b469ac, for GNU/Linux 3.2.0, stripped


$ readelf -dl HelloWorld | grep -E "Shared library:|program interpreter:"
      [Requesting program interpreter: /lib64/ld-linux-x86-64.so.2]
 0x0000000000000001 (NEEDED)             Shared library: [libc.so.6]

$ objdump -x HelloWorld | grep -C 1 GLIBC

  required from libc.so.6:
    0x06969187 0x00 02 GLIBC_2.27

$ objdump -j .dynstr -s HelloWorld

HelloWorld:     file format elf64-little

Contents of section .dynstr:
 0388 00707574 73005f5f 6378615f 66696e61  .puts.__cxa_fina
 0398 6c697a65 005f5f6c 6962635f 73746172  lize.__libc_star
 03a8 745f6d61 696e006c 6962632e 736f2e36  t_main.libc.so.6
 03b8 00474c49 42435f32 2e323700 5f49544d  .GLIBC_2.27._ITM
 03c8 5f646572 65676973 74657254 4d436c6f  _deregisterTMClo
 03d8 6e655461 626c6500 5f49544d 5f726567  neTable._ITM_reg
 03e8 69737465 72544d43 6c6f6e65 5461626c  isterTMCloneTabl
 03f8 6500                                 e.

$ readelf --dyn-syms HelloWorld

Symbol table '.dynsym' contains 8 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND
     1: 0000000000000560     0 SECTION LOCAL  DEFAULT   12
     2: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterT[...]
     3: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND _[...]@GLIBC_2.27 (2)
     4: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND puts@GLIBC_2.27 (2)
     5: 0000000000000000     0 FUNC    WEAK   DEFAULT  UND _[...]@GLIBC_2.27 (2)
     6: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMC[...]
     7: 0000000000000624    32 FUNC    GLOBAL DEFAULT   12 main
```

## Notes on Build Hardening 

### Stack guards

To enable this stack protection, code is compiled with the option **-fstack-protector**. This looks for functions that have typical buffers, inserting the guard/canary on the stack when the function is entered, and then verifying the value hasn’t been overwritten before exit.

### ASLR

When a buffer-overflow is exploited, a hacker will overwrite values pointing to specific locations in memory. That’s because locations, the layout of memory, are predictable. It’s a detail that programmers don’t know when they write the code, but something hackers can reverse-engineer when trying to figure how to exploit code.

Obviously a useful mitigation step would be to randomize the layout of memory, so nothing is in a predictable location. This is known as **address space layout randomization** or **ASLR**.

The word layout comes from the fact that the when a program runs, it’ll consist of several segments of memory. The basic list of segments are:

- the executable code
- static values (like strings)
- global variables
- libraries
- heap (growable)
- stack (growable)
- mmap()/VirtualAlloc() (random location)

The problem for executable code is that for ASLR to work, it must be made position independent. Historically, when code would call a function, it would jump to the fixed location in memory where that function was know to be located, thus it was dependent on the position in memory.

To fix this, code can be changed to jump to relative positions instead, where the code jumps at an offset from wherever it was jumping from.

To  enable this on the compiler, the flag **-fPIE** (position independent executable) is used. Or, if building just a library and not a full executable program, the flag **-fPIC** (position independent code) is used.

Then, when linking a program composed of compiled files and libraries, the flag **-pie** or **--static-pie** is used. In other words, use **-fPIE -pie** when compiling executables, and **-fPIC** when compiling for libraries.

When compiled this way, exploits will no longer be able to jump directly into known locations for code.

### RELRO

Modern systems have dynamic/shared libraries. Most of the code of a typical program consists of standard libraries. As mentioned above, the GNU standard library glibc is 8-megabytes in size. Linking that into every one of hundreds of programs means gigabytes of disk space may be needed to store all the executables. It’s better to have a single file on the disk, libglibc.so, that all programs can share it.

The problem is that every program will load libraries into random locations. Therefore, code cannot jump to functions in the library, either with a fixed or relative address. To solve this, what position independent code does is jump to an entry in a table, relative to its own position. That table will then have the fixed location of the real function that it’ll jump to. When a library is loaded, that table is filled in with the correct values.

The problem is that hacker exploits can also write to that table. Therefore, what you need to do is make that table read-only after it’s been filled in. That’s done with the “relro” flag, meaning “relocation read-only”. An additional flag, “now”, must be set to force this behavior at program startup, rather than waiting until later.

When passed to the linker, these flags would be “**-z relro -z now**“. However, we usually call the linker directly from the compiler, and pass the flags through. This is done in gcc by doing “**-Wl,-z,relro -Wl,-z,now**“.

### Non-executable stack

Exploiting a stack buffer overflow has three steps:

- figure out where the stack is located (mitigated by ASLR)
- overwrite the stack frame control structure (mitigated by stack guards)
- execute code in the  buffer

We can mitigate the third step by preventing code from executing from stack buffers. The stack contains data, not code, so this shouldn’t be a problem. In much the same way that we can mark memory regions as read-only, we can mark them no-execute. This should be the default, of course, but as the paper above points out, there are some cases where code is actually placed on the stack.

This open can be set with **-Wl,-z,noexecstack**, when compiling both the executable and the libraries. This is the default, so you shouldn’t need to do anything special. However, as the paper points out, there are things that get in the way of this if you aren’t careful. The setting is more what you’d call “guidelines” than actual “rules”. Despite setting this flag, building software may result in an executable stack.

### FORTIFY_SOURCE

One reason for so many buffer-overflow bugs is that the standard functions that copy buffers have no ability to verify whether they’ve gone past the end of a buffer. A common recommendation for code is to replace those inherently unsafe functions with safer alternatives that include length checks. For example the notoriously unsafe function **strcpy()** can be replaced with **strlcpy()**, which adds a length check.

Instead of editing the code, the GNU compiler can do this automatically. This is done with the build option **-O2 -D_FORTIFY_SOURCE=2**.

This is only a partial solution. The compiler can’t always figure out the size of the buffer being copied into, and thus will leave the code untouched. Only when the compiler can figure things out does it make the change.

### Format string bugs

Because this post is about build hardening, I want to mention format-string bugs. This is a common  bug in old code that can be caught by adding warnings for it in the build options, namely: `-Wformat=2 -Wformat-security -Werror=format-security`

### Warnings and static analysis

When building code, the compiler will generate warnings about confusion, possible bugs, or bad style. The compiler has a default set of warnings. Robust code is compiled with the **-Wall**, meaning “all” warnings, though it actually doesn’t enable all of them. Paranoid code uses the **-Wextra** warnings to include those not included with **-Wall**. There is also the **-pedantic** or **-Wpedantic** flag, which warns on C compatibility issues.

All of these warnings can be converted into errors, which prevents the software from building, using the **-Werror** flag. As shown above, this can also be used with individual error names to make only some warnings into errors.

### Optimization level

Compilers can optimize code, looking for common patterns, to make it go faster. You can set your desired optimization level.

Some things, namely the **FORTIFY_SOURCE** feature, don’t work without optimization enabled. That’s why in the above example, **-O2** is specified, to set optimization level **2**.

Higher levels aren’t recommended. For one thing, this doesn’t make code faster on modern, complex processors. The advanced processors you have in your desktop/mobile-phone themselves do extensive optimization. What they want is small code size that fits within cache. The **-O3 level make code bigger**, which is good for older, simpler processors, but is bad for modern, advanced processors.

In addition, the aggressive settings of **-O3 have lead to security problems over “undefined behavior”**. Even **-O2 is a little suspect**, with some guides suggesting the **proper optimization is -O1**. However, some optimizations are actually more secure than no optimizations, so for the security paranoid, **-O1 should be considered the minimum**.

### Summary

If you are building code using gcc on Linux, here are the options/flags you should use: `-Wall -Wformat -Wformat-security -Werror=format-security -fstack-protector -pie -fPIE -D_FORTIFY_SOURCE=2 -O2 -Wl,-z,relro -Wl,-z,now -Wl,-z,noexecstack`

If you are more paranoid, these options would be: `-O2 -D_FORTIFY_SOURCE=2 -Wall -Wextra -Wformat=2 -Wformat-security -Wstack-protector -Wdate-time -Werror -pedantic -fstack-protector-all -fstack-clash-protection -fPIE -pie -Wl,-z,relro -Wl,-z,now -Wl,-z,noexecstack`

## Hardening Check

```shell
HelloWorld:
 Position Independent Executable: yes
 Stack protected: yes
 Fortify Source functions: unknown, no protectable libc functions used
 Read-only relocations: yes
 Immediate binding: yes
 Stack clash protection: unknown, no -fstack-clash-protection instructions found
 Control flow integrity: no, not found!
```
