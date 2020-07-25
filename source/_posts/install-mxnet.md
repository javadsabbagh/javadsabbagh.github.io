---
title: Install and Setup Apache Mxnet
date: 2020-05-06 21:53:42
tags: [mxnet]
categories: [DL]
---


## <p dir='rtl'>برای نصب Apache Mxnet دستورات زیر را اجرا کنید:</p>

<p dir='rtl'>در ابتدا ممکن است بخواهید pip را بروزرسانی کنید. در صورتی که احتمالا نسخه pip نصب شده قدیمی باشد</p>
‍‍‍‍‍‍‍‍‍‍‍‍```sh
pip install --upgrade pip
pip install --upgrade setuptools
```

<p dir='rtl'>و سپس به سادگی اجرا کنید:</p>
‍‍‍‍‍‍‍‍‍‍‍‍```sh
pip install mxnet
```

<p dir='rtl'>این کار ممکن است کمی بکشد ولی در نهایت با نصب موفق مشاهده خواهید کرد که وابستگی های  Apache Mxnet نیز به همراه آن نصب شده اند:</p>
- Nump
- Graphviz

```sh
 Successfully built numpy
 Installing collected packages: numpy, graphviz, mxnet
 Successfully installed graphviz-0.8.4 mxnet-1.6.0 numpy-1.18.4
```