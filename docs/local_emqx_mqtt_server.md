---
title: How to set up docker MQTT locally? By EMQX
nav_order: 2
parent: Tutorials
---

## Install Docker

Step 1.1 — Update system

```bash
sudo apt update
```

Step 1.2 — Install Docker & Compose

```bash
sudo apt install -y docker.io docker-compose
```

Step 1.3 — Allow non-root Docker usage

```bash
sudo usermod -aG docker $USER
newgrp docker
```

Step 1.4 — Verify Docker

```bash
docker --version
docker-compose --version
```

## Create EMQX project folder

Step 2.1 — Create folder

```bash
mkdir emqx-demo
cd emqx-demo
```

Step 2.2 — Create data folders

```bash
mkdir data log
```

## Create docker-compose.yml

Step 3.1 — Open editor

```bash
nano docker-compose.yml
```

Step 3.2 — Paste this exactly

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

Step 4.1 — Pull & run

```bash
docker-compose pull
docker-compose up -d
```

Step 4.2 — Confirm it’s running

```bash
docker ps
```

## Access EMQX Dashboard

Step 5.1 — Open browser

Go to
```bash
http://localhost:18083
```

Step 5.2 — Login

```bash
Username: admin
Password: admin
```
