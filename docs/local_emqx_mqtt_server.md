---
title: How to set up docker MQTT locally? By EMQX
nav_order: 2
parent: Tutorials
---

## Install Docker

Step 1.1 - Update system

```bash
sudo apt update
```

Step 1.2 - Install Docker & Compose

```bash
sudo apt install -y docker.io docker-compose
```

Step 1.3 - Allow non-root Docker usage

```bash
sudo usermod -aG docker $USER
newgrp docker
```

Step 1.4 - Verify Docker

```bash
docker --version
docker-compose --version
```

## Create EMQX project folder

Step 2.1 - Create folder

```bash
mkdir emqx-demo
cd emqx-demo
```

Step 2.2 - Create data folders

```bash
mkdir data log
```

## Create docker-compose.yml

Step 3.1 - Open editor

```bash
nano docker-compose.yml
```

Step 3.2 - Paste this exactly

```yaml
version: '3.8'

services:
  emqx:
    image: emqx/emqx:latest
    container_name: emqx
    ports:
      - "1883:1883"     # MQTT
      - "18083:18083"   # Dashboard
    environment:
      EMQX_ALLOW_ANONYMOUS: "false"
      EMQX_DASHBOARD__DEFAULT_USERNAME: admin
      EMQX_DASHBOARD__DEFAULT_PASSWORD: admin
    volumes:
      - ./data:/opt/emqx/data
      - ./log:/opt/emqx/log
    restart: unless-stopped
```

Save and exit (CTRL+O, Enter, CTRL+X).

## Start EMQX

Step 4.1 - Pull & run

```bash
docker-compose pull
docker-compose up -d
```

Step 4.2 - Confirm it’s running

```bash
docker ps
```

## Access EMQX Dashboard

Step 5.1 - Open browser

Go to
```bash
http://localhost:18083
```

Step 5.2 - Login

```bash
Username: admin
Password: admin
```

## Create Built-in Database auth

Step 6.1 - Click Create and chose:

```bash
Mechanism: Password-Based
Backend: Built-in Database
```

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image01.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image01.png"
    alt="image01"
    style="max-width: 50%; width: auto; display: block; margin: 0 auto; cursor: zoom-in;"
  />
</a>

![image02]({{ site.baseurl }}/images/local_mqtt_emqx/image02.png)({{ site.baseurl }}/images/local_mqtt_emqx/image02.png)

![image03]({{ site.baseurl }}/images/local_mqtt_emqx/image03.png)({{ site.baseurl }}/images/local_mqtt_emqx/image03.png)

![image04]({{ site.baseurl }}/images/local_mqtt_emqx/image04.png)({{ site.baseurl }}/images/local_mqtt_emqx/image04.png)

## Add User

Step 7.1 - Click Users then Add

![image05]({{ site.baseurl }}/images/local_mqtt_emqx/image05.png)

![image06]({{ site.baseurl }}/images/local_mqtt_emqx/image06.png)

Step 7.2 - Creat user name and password of the users

```bash
Username: testuser
Password: testpass
```

![image07]({{ site.baseurl }}/images/local_mqtt_emqx/image07.png)

## Enable Authorization (ACL / permissions)

Authentication = who are you
Authorization = what are you allowed to do

Step 8.1 - Open Authorization page

![image08]({{ site.baseurl }}/images/local_mqtt_emqx/image08.png)

Step 8.2 - Create authorization source

Click Create

![image09]({{ site.baseurl }}/images/local_mqtt_emqx/image09.png)

```bash
Backend: Built-in Database
```
![image10]({{ site.baseurl }}/images/local_mqtt_emqx/image10.png)

Step 8.3 - Add permission for Username

![image11]({{ site.baseurl }}/images/local_mqtt_emqx/image11.png)

## Install Mosquitto clients

Step 9.1 - Install Mosquitto

```bash
sudo apt update
sudo apt install -y mosquitto-clients
sudo snap install mosquitto
```

## Testing

Step 10.1 - Terminal 1 Subscribe

```bash
mosquitto_sub -h <broker_ip> -u testuser -P testpass -I amebaClient -t outTopic
```

or

```bash
mosquitto_sub -h localhost -u testuser -P testpass -I amebaClient -t outTopic
```

Step 10.1 - Terminal 2 Publish allowed

```bash
mosquitto_pub -h localhost -u testuser -P testpass -I amebaClient -t outTopic -m "hello world"
```

## Testing with MQTT explore

Install program from [MQTT Explore](https://mqtt-explorer.com/){: .btn .btn-primary }

Setup BASIC setting:

![image12]({{ site.baseurl }}/images/local_mqtt_emqx/image12.png)

Setup ADVANCED setting:

![image13]({{ site.baseurl }}/images/local_mqtt_emqx/image13.png)

Connect, then chose the topic and PUBLISH

![image14]({{ site.baseurl }}/images/local_mqtt_emqx/image14.png)

## Clean restart

Stop & remove old container

```bash
docker-compose down
```

```bash
sudo snap stop mosquitto
```

If you ran EMQX before with volumes and want a true reset:

```bash
rm -rf data log
mkdir data log
```

## Find your PC’s local IP

```bash
ip addr show
```
