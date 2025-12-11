# 🐳 Web Infrastructure + Database (Docker Compose)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)

This project deploys a complete and automated infrastructure stack using **Docker Compose**. It simulates a basic production environment featuring a frontend web server and a backend database with guaranteed data persistence.

## 🏗 Architecture

The project consists of two orchestrated services:

1.  **Frontend (Web):** Nginx server (lightweight Alpine version) configured with *Bind Mounts* for real-time code editing.
2.  **Backend (DB):** MariaDB 10.6 database with persistent volumes.
3.  **Network:** Both services communicate within an isolated internal network.

## 🚀 Quick Start

Ensure you have **Docker** and **Docker Compose** installed.

```bash
# 1. Clone the repository
git clone [https://github.com/rubikey/docker-basics.git](https://github.com/rubikey/docker-basics.git)
cd my-first-docker

# 2. Start the infrastructure (detached mode)
docker compose up -d