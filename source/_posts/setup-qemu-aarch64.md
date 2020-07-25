---
title: Setup QEMU for ARM 64
date: 2020-05-29 13:09:48
tags: [QEMU, ARM]
categories:
- [embedded]
---

> **Note**
> ARM 64 architecture is also known as, aarch64.

## 1. Why we need QEMU?

We cannot virtualize ARM on X86 machine, then wee need to emulate it. So, we need an emulator  such as QEMU.

## 2. QEMU and Hypervisors

QEMU can be used by virtualization hypervisors such as XEN and KVM. Hypervisors, underneath, can manage QEMU access to system resources.

For more information please look: [KVM-QEMU-Libvirt](https://events19.linuxfoundation.org/wp-content/uploads/2017/12/Kashyap-Chamarthy_Effective-Virtual-CPU-Configuration-OSS-EU2018.pdf).

### 2.1 LibVirt vs KVM

Libvirt is a library which can be used to manage QEMU instances, like Vagrant which is used to create and manage VirtualBox machines. Its API can be called by OpenStack and etc.

KVM is a hypervisor in Linux Kernel space and QEMU can be connected to it to get services regarding running machine resources.

## 3. Building QEMU for ARM 64 (aarch64)

### 3.1 Buidling from the source

The reason for building from source is that we are going to have the latest release!

```bash
wget https://download.qemu.org/qemu-5.0.0.tar.xz
tar xvJf qemu-5.0.0.tar.xz
cd qemu-5.0.0
mkdir  build && cd build
../configure --target-list=aarch64-softmmu,aarch64-linux-user
make -j2
```

> Target for regular (32 bit) ARM is : arm-softmmu

### 3.2 Build Config

> **Note**
> If you don't specify target-list it will compile and build QEMU for all available target  architectures. Which is unrelevant, because it takes a very long time and uses lots of disk space.

To see available target architectures, run:

```bash
./configure --help
```

Then have a look at available values for ***-target-list*** parameter.

After successful build the output would be in following directory:

```sh
aarch64-softmmu/qemu-system-aarch64
```

### 3.3 Running QEMU

Getting list of supported machines config:

```bash
aarch64-softmmu/qemu-system-aarch64 -M help | grep orange
```

It only shows ***orangepi-pc*** which is the version of Orange PI using Allwinner H3 (Cortex-A7) 32-bit ARM processor. It means that probably you cannot emulate for example boards with H6 chip with ***-M orangepi-pc*** option.

However, if you run:

```bash
 aarch64-softmmu/qemu-system-aarch64 -cpu help
```

You see that cortex-a53, architecture implemented by Allwinner H6, is in the list. For that purpose, we will try another options in {% post_link armbian-orangepione-qemu Running Armbian on QEMU for OrangePi One Plus %} post.

### 3.4 Good to consider

You noticed, that we have build ***aarch64-linux-user*** in the target list. I don't know exactly what it does, maybe emulates ARM-64 programs as Linux User-Space applications.

Following command:

```bash
aarch64-linux-user/qemu-aarch64 -cpu help
```

Shows that it support much less architectures:

```sh
Available CPUs:
  cortex-a53
  cortex-a57
  cortex-a72
  max
```
