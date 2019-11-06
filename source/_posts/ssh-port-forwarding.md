---
title: SSH Port Forwarding
date: 2019-11-06 11:11:20
tags: [ssh, proxy]
categories:
- shell
---

<p dir='rtl'>port-forwarding از طریق ssh برای مقاصد مختلفی استفاده می شود که در ادامه به چند کاربرد پر استفاده از آن اشاره خواهیم کرد:</p>

### <p dir='rtl'>1. دسترسی به یک سرور ریموت از طریق کامپیوتر واسط</p>
<p dir='rtl'>فرض کنید دسترسی به سرور 10.39.23.141 و پورت 982 آن صرفا برای 192.168.100.111 فراهم می باشد و می خواهیم از طریق 192.168.100.111 به آن سرویس دسترسی داشته باشیم:</p>

‍‍‍‍‍‍‍‍‍‍‍‍```sh
	ssh -L 127.0.0.1:8686:10.39.23.141:982 user@192.168.100.111
```

### <p dir='rtl'>2. ایجاد پراکسی سرور socks</p>
‍‍‍```bash
sh -D 1337 -q -C -N -f user@192.168.10.20 
```
<span dir='rtl' style='float:right'>تنظیم پراکسی در مرورگر فایرفاکس</span>
.
![ssh-proxy-server.png](./ssh-proxy-server.png)

<span dir='rtl' style='float:right'>تنظیم پراکسی در محیط ترمینال</span>
‍‍‍```sh
export http_proxy=socks5://127.0.0.1:1337
export https_proxy=socks5://127.0.0.1:1337

unset http_proxy  # if you want to unset proxy config
```
> <span dir='rtl'>برخی برنامه ها این تنظیمات را نادیده می گیرند</span>