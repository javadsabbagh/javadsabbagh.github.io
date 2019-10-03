---
title: Installing Rocket.Chat Server
---
Running Rochat.Chat server as docker container is straightforward. 

# Requirements
In order to run Rocket.Chat server, the following softwares are need to installed:s 
* {% post_link install-docker %}
* {% post_link install-docker-compose %}

# Steps to setup Rocket.chat server
### 1. Create a user to run Rocket.Chat service
For security reasons, it is highly suggested to run services as a non-root user.

- Add a user:
```bash
useradd payamresan
```

- Add docker group to 'payamresan' user:
```bash
usermod -aG docker payamresan
```

- Check out the created user is a member of 'docker' group:
```bash
id payamresan
# uid=1001(payamresan) gid=1001(payamresan) groups=994(docker),1001(payamresan)
```

### 2. Pull docker images

- Rocket.chat uses Mongodb as a database. Then, following images are needed to be in local docker registery
```bash
docker pull mongo
docker pull rocketchat/rocket.chat
```

- Create the following directory:
```bash
mkdir -p /var/www/rocket.chat
```

- Create **docker-compose.yml** file in above directory:
```yaml
db:
  image: mongo
  volumes:
    - ./data/runtime/db:/data/db
    - ./data/dump:/dump
  ports:
    - 27017:27017
  command: mongod --smallfiles

rocketchat:
  image: rocketchat/rocket.chat:latest
  environment:
    - MONGO_URL=mongodb://db:27017/rocketchat
    - ROOT_URL=http://192.168.100.118
    - Accounts_UseDNSDomainCheck=True
  links:
    - db:db
  ports:
    - 3000:3000

hubot:
  image: rocketchat/hubot-rocketchat:latest
  environment:
    - ROCKETCHAT_URL=192.168.100.118:3000
    - ROCKETCHAT_ROOM=GENERAL
    - ROCKETCHAT_USER=Botname
    - ROCKETCHAT_PASSWORD=BotPassw0rd
    - BOT_NAME=Botname
    - EXTERNAL_SCRIPTS=hubot-help,hubot-seen,hubot-links,hubot-greetings
  links:
    - rocketchat:rocketchat
# this is used to expose the hubot port for notifications on the host on port 3001, e.g. for hubot-jenkins-notifier
  ports:
    - 3001:8080
```

- Give access to payamresan user:
```bash
chown payamresan:docker /var/www/rocket.chat/
chown payamresan:docker /var/www/rocket.chat/docker-compose.yml
```

### 3. Create systemd unit services
Create the following file in /etc/systemd/system/ directory:

- **payamresan.service**
```
[Unit]
Description=Payamresan container starter based on docker-compose
After=docker.service network-online.target
Requires=docker.service network-online.target

[Service]
WorkingDirectory=/var/www/rocket.chat
User=payamresan
Group=docker
Type=oneshot
RemainAfterExit=yes

### Net connection possibly is not available, then I comment the following command
#ExecStartPre=-/usr/local/bin/docker-compose pull --quiet
ExecStart=/usr/local/bin/docker-compose up -d db
ExecStart=/usr/local/bin/docker-compose up -d rocketchat

ExecStop=/usr/local/bin/docker-compose down

### Net connection possibly is not available, then I comment the following command
#ExecReload=/usr/local/bin/docker-compose pull --quiet
ExecStart=/usr/local/bin/docker-compose up -d db
ExecReload=/usr/local/bin/docker-compose up -d rocketchat

[Install]
WantedBy=multi-user.target
```

- Reload systemd daemons to identify our recently created service:
```bash
systemctl daemon-reload
```

- Start payamresan service:
```bash
systemctl start payamresan
```

- Check service status:
```bash
systemctl status payamresan
```
 - You should see the following output:
```
● payamresan.service - Payamresan container starter based on docker-compose
   Loaded: loaded (/etc/systemd/system/payamresan.service; enabled; vendor preset: disabled)
   Active: active (exited) since Sat 2019-02-23 17:05:26 IRST; 2 weeks 1 days ago
  Process: 14296 ExecStop=/usr/local/bin/docker-compose down (code=exited, status=0/SUCCESS)
  Process: 14566 ExecStart=/usr/local/bin/docker-compose up -d db (code=exited, status=0/SUCCESS)
  Process: 14472 ExecStart=/usr/local/bin/docker-compose up -d rocketchat (code=exited, status=0/SUCCESS)
  Process: 14387 ExecStart=/usr/local/bin/docker-compose up -d db (code=exited, status=0/SUCCESS)
 Main PID: 14566 (code=exited, status=0/SUCCESS)
   Memory: 0B
   CGroup: /system.slice/payamresan.service
```

 - In the case of any failure, check error message with following command:
```bash
journalctl -xe
```

- Enable *payamresan* service, so it is started automatically after system boot.
```bash
systemctl enable  payamresan
```
