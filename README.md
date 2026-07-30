# 🚀 Real-Time WebSocket Chat Application
### DevOps Engineering Assignment

## 📌 Project Overview

This project demonstrates the deployment of a containerized Real-Time WebSocket Chat Application using Docker, Docker Compose, Nginx Reverse Proxy, GitHub Actions CI/CD, and AWS EC2.

The objective of this assignment was to debug an intentionally misconfigured deployment environment without modifying the application source code. The focus was on identifying infrastructure issues, fixing container networking, configuring Nginx for WebSocket proxying, and automating deployments.

---

# 🏗️ Architecture

```
                    User Browser
                          │
                          ▼
                AWS EC2 Public IP
                          │
                          ▼
               Nginx Reverse Proxy
                  (Docker Container)
                          │
                WebSocket Proxy (/ws)
                          │
                          ▼
          FastAPI WebSocket Application
               (Docker Container)
```

---

# 🛠️ Technology Stack

- Docker
- Docker Compose
- FastAPI
- Nginx
- WebSockets
- GitHub Actions
- AWS EC2 (Ubuntu)
- Linux

---

# 📁 Project Structure

```
websocket-app/
│
├── app/
│   ├── main.py
│   └── requirements.txt
│
├── frontend/
│   └── index.html
│
├── .github/
│   └── workflows/
│       └── deploy.yml
│
├── Dockerfile
├── docker-compose.yml
├── nginx.conf
├── README.md
```

---

# 🐳 Docker Container Setup

The application consists of two Docker containers.

## Backend Container

- FastAPI application
- Runs on Port **8000**
- Handles WebSocket communication

## Nginx Container

- Serves static frontend files
- Reverse proxies WebSocket requests
- Runs on Port **80**

Both containers communicate over the Docker Compose network.

---

# 🌐 Docker Networking

Docker Compose automatically creates a bridge network where containers communicate using service names.

Example:

```
backend
```

instead of

```
localhost
```

The Nginx container forwards WebSocket requests directly to the backend service over the Docker network.

---

# 🔄 Nginx Reverse Proxy

Nginx performs two functions:

- Serves frontend static files
- Proxies WebSocket traffic to the FastAPI backend

WebSocket configuration:

```nginx
location /ws {

    proxy_pass http://backend:8000/ws;

    proxy_http_version 1.1;

    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";

    proxy_set_header Host $host;

    proxy_set_header X-Real-IP $remote_addr;

    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

    proxy_set_header X-Forwarded-Proto $scheme;

}
```

---

# 🔌 WebSocket Communication

The browser establishes a persistent WebSocket connection through Nginx.

```
Browser
    │
    ▼
Nginx
    │
    ▼
FastAPI (/ws)
```

This enables:

- Real-time messaging
- Multiple users
- Live user count
- Persistent bidirectional communication

---

# 🚀 Deployment

The application is deployed on an AWS EC2 Ubuntu instance.

Deployment command:

```bash
docker compose up -d --build
```

Application URL:

```
http://3.111.236.54/
```

---

# ⚙️ CI/CD Pipeline

GitHub Actions automates deployment.

Workflow:

1. Push code to GitHub
2. GitHub Actions starts
3. Connects to AWS EC2 via SSH
4. Pulls latest code
5. Rebuilds Docker images
6. Restarts containers

Deployment becomes fully automated.

---

# 🐞 Issues Identified & Fixes

## Issue 1 – Backend Container Unreachable

### Problem

FastAPI was listening on:

```text
127.0.0.1
```

This prevented the Nginx container from accessing the backend.

### Fix

Changed:

```text
127.0.0.1
```

to

```text
0.0.0.0
```

Result:

Containers communicate successfully.

---

## Issue 2 – Frontend Not Loading

### Problem

Nginx displayed the default welcome page.

### Cause

Frontend directory was not mounted into the container.

### Fix

Mounted:

```yaml
./frontend:/usr/share/nginx/html
```

Result:

Chat UI loads successfully.

---

## Issue 3 – WebSocket Connection Failed

### Problem

Nginx attempted to proxy requests to:

```text
localhost:8000
```

Inside Docker, localhost refers to the Nginx container itself.

### Fix

Changed to:

```text
backend:8000
```

Also enabled:

- Upgrade header
- Connection header

Result:

WebSocket connection established successfully.

---

# ✅ Verification

Successfully verified:

- Docker Compose deployment
- Container networking
- Nginx reverse proxy
- WebSocket connection
- Real-time messaging
- Multiple browser tabs
- AWS deployment
- CI/CD automation

---

# ▶️ Running Locally

Clone repository

```bash
git clone https://github.com/samruddhi10/websocket-app.git
```

Go to project

```bash
cd websocket-app
```

Build containers

```bash
docker compose up -d --build
```

Open browser

```
http://localhost
```

---

# 🌍 Live Deployment

Application URL

```
http://3.111.236.54/
```

---

# 📸 Screenshots

Include screenshots of:

- Running containers (`docker ps`)
- Chat application
- Multiple browser tabs chatting
- GitHub Actions workflow
- AWS EC2 instance

---

# 📚 Learning Outcomes

This assignment demonstrates practical experience with:

- Docker
- Docker Compose
- Container Networking
- FastAPI
- Nginx Reverse Proxy
- WebSockets
- AWS EC2
- GitHub Actions
- CI/CD Automation
- Production Deployment

---

# 👩‍💻 Author

**Samruddhi Parate**

Cloud Engineer | AWS | Docker | Linux | CI/CD | DevOps
