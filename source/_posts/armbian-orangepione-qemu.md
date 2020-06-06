---
title: Running Armbian on QEMU for OrangePi One Plus
date: 2020-05-29 19:38:58
tags: [QEMU, ARM]
categories:
- [embedded]
---

## Board Hardware

Our board (Orange Pi One Plus) leverages Allwiner H6 which is a 64-bit (Cortex A53) ARM processor.

> **Note**
> There are different Cortext-A53 implementations, and Allwinner (Sunnix) H6 is one of them.

## Write Armbian on SD-Card

Prepare SD card (four methodes):

```bash
zcat Armbian_20.02.1_Orangepioneplus_buster_current_5.4.20.img.gz | pv | dd of=/dev/mmcblkX bs=1M
dd if=Armbian_20.02.1_Orangepioneplus_buster_current_5.4.20.img of=/dev/mmcblkX bs=1M 
balena-etcher Armbian_20.02.1_Orangepioneplus_buster_current_5.4.20.img.gz -d /dev/mmcblkX
balena-etcher Armbian_20.02.1_Orangepioneplus_buster_current_5.4.20.img -d /dev/mmcblkX
```

## Remove CPU frequency limit for H6

Default setting for CPU frequency is as follows:

```sh
CPU configuration: 480000 - 1810000 with ondemand
```

The H6 CPU frequency has ben soft-capped at 1,48 GHz to avoid thermal throttling too fast. This limit can be lifted by editing /etc/default/cpufrequtils and set MAX_SPEED to 1810000.
