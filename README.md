# 🐳 Web Infrastructure + Database (Docker Compose)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![Ansible](https://img.shields.io/badge/Ansible-EE0000?style=for-the-badge&logo=ansible&logoColor=white)

This project deploys a complete and automated infrastructure stack using **Docker Compose** and **Ansible**. It simulates a basic production environment featuring a frontend web server and a backend database with guaranteed data persistence, orchestrated via Infrastructure as Code (IaC).

## 🏗 Architecture

The project consists of orchestrated services and automation layers:

1.  **Frontend (Web):** Nginx server (lightweight Alpine version) configured with *Bind Mounts* for real-time code editing.
2.  **Backend (DB):** MariaDB 10.6 database with persistent volumes (`mysql-data`).
3.  **Network:** Both services communicate within an isolated internal network.
4.  **Ansible:** Automation engine acting as the deployment controller. It manages the directory structure (`/opt/my-awesome-app`), handles file provisioning, and executes the Docker Compose logic through a declarative Playbook.

#