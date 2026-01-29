---
title: How to set up docker MQTT locally? By EMQX
nav_order: 2
parent: docs
---

# Install Docker

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

# Create EMQX project folder

Step 2.1 — Create folder

```bash
mkdir emqx-demo
cd emqx-demo
```

Step 2.2 — Create data folders

```bash
mkdir data log
```