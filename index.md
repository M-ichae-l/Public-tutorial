---
title: Home
nav_order: 1
has_toc: true
has_children: true
---

![logo](images/background.jpg)

# Welcome to Public Tutorial

Welcome to this simple tutorial designed to help you learn and practice essential software skills. Whether you are a beginner or have some experience, this guide will walk you through step by step, making complex concepts easier to understand. You will gain practical knowledge by following clear instructions, trying out examples, and exploring different features of the software. By the end of this tutorial, you will feel more confident in applying these skills in real-life projects. Our goal is to make learning engaging, straightforward, and effective, so you can build a strong foundation for future growth.

## How to set up docker MQTT locally? By EMQX

### Install Docker

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

### Create EMQX project folder

Step 2.1 — Create folder

```bash
mkdir emqx-demo
cd emqx-demo
```

Step 2.2 — Create data folders

```bash
mkdir data log
```