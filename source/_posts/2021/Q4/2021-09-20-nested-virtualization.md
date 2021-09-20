---
title: Nested virtualization
description: Nested virtualization
date: 2021-09-20 10:24:30
tags:
  - Operating system
  - Linux
categories:
  - Operating system
  - Linux
permalink: nested-virtualization
---

# Nested virtualization

Nested virtualization allows you to run a virtual machine (VM) inside another VM while still using hardware acceleration from the host.

naming conventions:

+ L0 hypervisor, or Bare-metal host – this is the hypervisor that runs on the physical bare-metal server
+ L1 hypervisor or Guest hypervisor – this is a hypervisor that runs as a virtual machine in the L0 hypervisor.
+ L2 guests, or nested guests – these are virtual machines running in L1 hypervisor.

## Hardware virtualization support

Hardware virtualization support or hardware-assisted virtualization is a set of processor extensions to the x86 architecture. These extensions address issues with the virtualization of some privileged instructions and the performance of virtualized system memory. Intel’s implementation is called VT-x and AMD’s implementation is called AMD-V. This feature was introduced in 2005 and 2006 and is available in most current CPUs. Some exceptions are lower-end Atom processor. However, this feature might be disabled in the BIOS.

### Checking if virtualization is supported

To check whether hardware virtualization support is available on the host processor, check the CPU has the `vmx` or `svm` flag with the command:

```bash
# egrep --color -i "svm|vmx" /proc/cpuinfo
```

If you see `vmx` (Intel-VT technology) or `svm` (AMD-V support) in the output, the KVM guest machine can work as a hypervisor and host VMs.

## Nested virtualization support

Nested virtualization is a KVM feature that enables hardware-assisted virtualization in the guest hypervisors. With it, the guest hypervisor can leverage virtualization extensions of the physical CPU without need to emulate them in software.

### Checking if nested virtualization is supported

For **Intel** processors, check the `/sys/module/kvm_intel/parameters/nested` file. For **AMD** processors, check the `/sys/module/kvm_amd/parameters/nested` file. If you see **1** or **Y**, nested virtualization is supported; if you see **0** or **N**, nested virtualization is not supported.

For example:

```bash
# cat /sys/module/kvm_intel/parameters/nested
Y
```

### Enabling nested virtualization in KVM

#### Enable nested virtualization for Intel processors

1. Shut down all running VMs and unload the `kvm_intel` module

```bash
sudo modprobe -r kvm_intel
```

2. Activate the nesting feature

```bash
sudo modprobe kvm_intel nested=1
```

3. Enable it permanently

Nested virtualization is enabled until the host is rebooted. To enable it permanently, add the following line to the `/etc/modprobe.d/kvm.conf` file:

```bash
options kvm_intel nested=1
```

#### enable nested virtualization for AMD processors

1. Shut down all running VMs and unload the `kvm_amd` module

```bash
sudo modprobe -r kvm_amd
```

2. Activate the nesting feature

```bash
sudo modprobe kvm_amd nested=1
```

3. Enable it permanently

Nested virtualization is enabled until the host is rebooted. To enable it permanently, add the following line to the `/etc/modprobe.d/kvm.conf` file:

```bash
options kvm_amd nested=1
```

### Enabling nested virtualization in Hyper-V

Nested virtualization is a feature that allows you to run Hyper-V inside of a Hyper-V virtual machine (VM). This is helpful for running a Visual Studio phone emulator in a virtual machine, or testing configurations that ordinarily require several hosts.

#### Prerequisites

##### Intel processor with VT-x and EPT technology

+ The Hyper-V host must be Windows Server 2016/Windows 10 or greater
+ VM configuration version 8.0 or greater

##### AMD EPYC/Ryzen processor or later

+ The Hyper-V host must be Windows Server 2022/Windows 11 or greater
+ VM configuration version 10.0 or greater

#### Configure Nested Virtualization

1. Create a virtual machine. See the prerequisites above for the required OS and VM versions.
2. While the virtual machine is in the OFF state, run the following command on the physical Hyper-V host. This enables nested virtualization for the virtual machine.

```bash
Set-VMProcessor -VMName <VMName> -ExposeVirtualizationExtensions $true
```

3. Start the virtual machine. Install Hyper-V within the virtual machine, just like you would for a physical server.

## VMCS shadowing

Virtual Machine Control Structure is a memory structure where the virtual CPU state is stored on VM exit and restored from on VM resume. This happens every time the guest operating system performs a privileged operation. This is done in hardware, but then the hypervisor is a virtual machine, This function has to be emulated and causes multiple VM exits to the host.

With VMCS shadowing for every L2 VM, a VMCS is created in the host, that is shadowed in the L1 hypervisor. This way all VM exits are implemented in hardware reducing significantly the processing overhead.  VMCS shadowing is not strictly required for nested virtualization, but it gives huge performance improvement, especially on workloads involving many context switches.

To be able to do this a hardware support in the CPU is required. This hardware feature was introduced in Haswell processors initially in 2013 and is available in most current server CPUs.

To use it you need to enable it in kvm kernel module:

In `/etc/modprobe.d/kvm-intel.conf` add:

```bash
options kvm-intel enable_shadow_vmcs=1
```
