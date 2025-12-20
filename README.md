# 🐳 Web Infrastructure (Docker Compose)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Nginx Proxy Manager](https://img.shields.io/badge/Nginx_Proxy_Manager-F15833?style=for-the-badge&logo=nginx&logoColor=white)
![Ansible](https://img.shields.io/badge/Ansible-000000?style=for-the-badge&logo=ansible&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-800080?style=for-the-badge&logo=github-actions&logoColor=white)
![AdGuard Home](https://img.shields.io/badge/AdGuard-68BC71?style=for-the-badge&logo=adguard&logoColor=white)

This project deploys a full-stack, automated infrastructure using **Docker Compose**, **Ansible**, and **GitHub Actions**. It features an architecture with a Reverse Proxy gateway, automated deployments via Self-Hosted Runners, persistent data storage with backups, and network privacy tools.

## 🏗 Architecture & Stack

The infrastructure is composed of 5 main layers:

1.  **Gateway (Reverse Proxy):** **Nginx Proxy Manager** listening on ports `80`/`443`. It handles domain resolution (`my-app.local`, `dozzle.local`, `nginx.local`, `adguard.local`), SSL termination, and routes traffic to internal containers.

2.  **Application (Web):** Nginx server (Alpine) serving static content. It is not exposed to the public directly; it sits securely behind the proxy.

3.  **Data (Backend & Security):**
    * **Database:** MariaDB 11.4.9 with persistent volumes (`mysql-data`).
    * **Automated Backups:** A sidecar container (`fradelg/mysql-cron-backup`) that performs daily database dumps.

4.  **Network & Privacy:**
    * **AdGuard Home:** A network-wide software for blocking ads and tracking. Acts as a DNS server (Port `53`) with a web dashboard running on port `3000`.

5.  **Observability & Monitoring:**
    * **Dozzle:** Real-time log viewer for all running containers, accessible via the web interface.

6.  **Automation (CI/CD):**
    * **Ansible:** Manages the configuration state and idempotency.
    * **GitHub Actions:** A Self-Hosted Runner on the target machine detects commits and triggers Ansible automatically to redeploy changes with zero downtime.

---

##### Author: rubikey
