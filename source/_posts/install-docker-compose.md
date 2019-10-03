---
title: Install Docker Compose
date: 2019-03-10 17:30:53
tags:
---

Docker compose is needed when we need to run several containers together.

Installing docker-compose can be done in one of the following ways:

### Method 1- Download binary file
Maybe, the easiest and more straightforward method of installing docker-compose is simply downloading and copying it
to the proper directory:


> **Note**
> You need to replace docker-compose version (1.23.2) with the last stable released number.


1. Download docker-compose:
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2. Make the file executable:
```bash
sudo chmod +x /usr/local/bin/docker-compose
```
3. Optionally, dowloand bash-complete file:
```bash
sudo curl -L "https://raw.githubusercontent.com/docker/compose/1.23.2/contrib/completion/bash/docker-compose" -o /etc/bash_completion.d/docker-compose
```

Now, you can check docker-compose is installed correctly:
```bash
docker-compose --version
# docker-compose version 1.23.2, build 1110ad01
```

### Method 2- Install using PyPi
In this regard, you need python and pip to be already installed.
```bash
pip install docker-compose
```