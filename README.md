# 🐳 Web Infrastructure + Database (Docker Compose)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Nginx Proxy Manager](https://img.shields.io/badge/Nginx_Proxy_Manager-F15833?style=for-the-badge&logo=nginx&logoColor=white)
![Ansible](https://img.shields.io/badge/Ansible-EE0000?style=for-the-badge&logo=ansible&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)

This project deploys a full-stack, automated infrastructure using **Docker Compose**, **Ansible**, and **GitHub Actions**. It features a self-healing architecture with a Reverse Proxy gateway, automated deployments via Self-Hosted Runners, and persistent data storage.

## 🏗 Architecture & Stack

The infrastructure is composed of 4 main layers:

1.  **Gateway (Reverse Proxy):** **Nginx Proxy Manager** listening on ports `80/443`. It handles domain resolution (`mi-app.local`), SSL termination, and routes traffic to internal containers.
2.  **Application (Web):** Nginx server (Alpine) serving static content. It is **not exposed** to the public directly; it sits behind the proxy.
3.  **Data (Backend):** MariaDB 10.6 database with persistent volumes (`mysql-data`).
4.  **Automation (CI/CD):**
    * **Ansible:** Manages the configuration state and idempotency.
    * **GitHub Actions:** A Self-Hosted Runner on the target machine detects commits and triggers Ansible automatically.


#### Author: rubikey