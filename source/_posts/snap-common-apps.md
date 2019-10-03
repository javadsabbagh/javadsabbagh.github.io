---
title: Common Apps on the snap service
date: 2019-10-02 15:47:20
tags: [linux]
---


### Install Apps
- VS Code
```bash
sudo snap install code
```

- 
```bash
sudo snap install gtk-common-themes
```

- Gitkraken
```bash
sudo snap install gitkraken
```

- Skype
```bash
sudo snap install skype --classic
```

- MicroK8s
```bash
sudo snap install microk8s --classic
```

- Sublime
```bash
sudo snap install  sublime-text --classic
```

#### Update app
```bash
snap refresh gitkraken
```

### List installed apps
```bash
snap list
```

### List snap services
```bash
snap services 
```

### Search for an app
```bash
snap search microk8s
```

### Manage snap service
```bash
 sudo systemctl start snapd
 sudo systemctl enable snapd
 sudo systemctl status snap
 sudo systemctl stop snap
 ```
