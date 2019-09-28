---
title: Install and runing JDK on Ubuntu
date: 2019-09-28 12:09:51
tags: [ubuntu, debian, JDK]
categories: [Java]
---

1. Extract and copy files:
```sh
sudo mkdir -p /opt/java
sudo tar zxvf jdk-11-linux-x64.tar.gz --directory /opt/java
```

- You can also use */usr/lib/jvm* path.

2. Register newly installed jdk:
```sh
sudo update-alternatives --install /usr/bin/java java /opt/java/jdk-11-linux-x64/bin/
```
Outputs:
java 100

3. Choosing between installed java programms:
```sh
sudo update-alternatives --config java
```

Select * for keeping currently selected item or the number of desired application.

4. Which default java version is used:
```sh
sudo update-alternatives --display java
```

- Repeat steps 2 to 4 for setting default **javac** application.

## Old fashion Installation
Is done by updating ~/.bashrc or ~/.zshrc files and defining following environment variables:
- JAVA_HOME
- PATH