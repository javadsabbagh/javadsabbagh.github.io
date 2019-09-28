---
title: Runnin Jenkins
date: 2019-07-10 15:47:20
tags: [jenkins]
categories: [CI-CD]
---

Download the lastest [jenkins.war](https://jenkins.io/download/) file and run it as following:
```sh
java -jar jenkins.war --httpPort=9292
```

If your server is behind firewall then open the given port number such as:
```sh
sudo ufw allow 9292/tcp
```

JENKINS_HOME
http://192.168.100.116:9292/systemInfo