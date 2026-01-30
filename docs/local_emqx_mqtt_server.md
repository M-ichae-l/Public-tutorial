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
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image01.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image01.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in; border: 2px solid #555; padding:5px; box-shadow: 3px 3px 8px rgba(0,0,0,0.4);"
  />
</a>


<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image02.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image02.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image03.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image03.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image04.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image04.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

## Add User

Step 7.1 - Click Users then Add

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image05.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image05.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image06.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image06.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

Step 7.2 - Creat user name and password of the users

```bash
Username: testuser
Password: testpass
```

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image07.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image07.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

## Enable Authorization (ACL / permissions)

Authentication = who are you
Authorization = what are you allowed to do

Step 8.1 - Open Authorization page

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image08.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image08.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

Step 8.2 - Create authorization source

Click Create

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image09.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image09.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

```bash
Backend: Built-in Database
```
<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image10.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image10.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

Step 8.3 - Add permission for Username

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image11.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image11.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

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

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image12.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image12.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

Setup ADVANCED setting:

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image13.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image13.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

Connect, then chose the topic and PUBLISH

<a href="{{ site.baseurl }}/images/local_mqtt_emqx/image14.png" target="_blank">
  <img
    src="{{ site.baseurl }}/images/local_mqtt_emqx/image14.png"
    style="max-width:50%; max-height:400px; display:block; margin:auto; cursor:zoom-in;"
  />
</a>

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
