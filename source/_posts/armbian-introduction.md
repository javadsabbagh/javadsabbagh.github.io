---
title: Armbian Introduction
date: 2020-05-29 15:47:40
tags: [Armbian, ARM]
categories:
- [embedded]
---

## Armbian documentation

- [Armbian Home Page](https://docs.armbian.com/)

## What is Armbian?

Armbian is a lightweight Linux distro based on Debian or Ubuntu with XFCE desktop for ARM processors.

## Some default configs
user: root
pass: 1234

Please, look at "Performance tweaks" section, and if needed disable swap to boost the performance.

Hardware info:

```bash
/etc/init.d/armhwinfo
```

## Change Boot Settings

You should modify following file:

```bash
/boot/armbianEnv.text
```

## Look at CPU temperature

Proprt cooling is need for boards with A64 processors. Keep eye on CPU usage by command: 

```bash
sudo armbianmonitor -r
```
