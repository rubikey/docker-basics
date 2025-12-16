# 📘 CHEATSHEET DEVOPS: DOCKER & ANSIBLE

This document is a fast reference for SysAdmins about commands and essential modules.

---

## 🐳 PART 1: DOCKER CLI (Terminal commands)

Manual container and image management.

### 🟢 Container Lifecycle
```bash
docker run -d -p 80:80 nginx   # Pull, create and start in the background
docker ps                      # Show Running containers
docker ps -a                   # Show all containers (Including stopped)
docker stop <name>             # Stop a container
docker start <name>            # Start a stopped container
docker restart <name>          # Fast restart
docker rm <name>               # Delete container (Must be stopped)
docker rm -f <name>            # Force delete

### 📦 Image Management
docker images                  # List pulled images
docker pull <image>:<tag>      # Pull without run (ex: ubuntu:24.04)
docker rmi <image>             # Delete image from disk
docker build -t <name> .       # Build image from current Dockerfile

### 🕵️ Debugging and Maintenance
docker logs <name>           # Show internal logs
docker logs -f <name>        # Show running logs (like 'tail -f')
docker exec -it <name> sh    # Enter into container (terminal)
docker inspect <name>        # Show metadata (IP, volumes) in JSON
docker stats                 # Show live CPU/RAM usage
docker system prune -a       # System cleanup (Dangerous!)
```
---

### 🏗️ PART 2: DOCKERFILE

Instructions to create custom images.

| **FROM** | Base image (Obligatory) | `FROM python:3.9-alpine` |

| **WORKDIR** | Create and enter the directory | `WORKDIR /app` |

| **COPY** | Copy files from local PC to container | `COPY . .` |

| **RUN** | Execute commands during the build | `RUN apt-get update` |

| **ENV** | Define environment variable | `ENV PORT=8080` |

| **EXPOSE** | Documents the listening port | `EXPOSE 80` |

| **CMD** | Default running command | `CMD ["python", "app.py"]` |

---

### 🐙 PART 3: DOCKER COMPOSE (Orchestration)

Basic structure of `docker-compose.yml`.

```yaml
version: "3.8"

services:
  my-app:
    container_name: web-server
    build: .                  # Build from local Dockerfile
    ports:
      - "8080:80"             # Host:Container
    environment:
      - MODO=produccion
    volumes:
      - ./html:/usr/share/nginx/html  # Bind Mount
    restart: always

  db:
    image: mariadb:10.6
    container_name: db-server
    environment:
      MARIADB_ROOT_PASSWORD: secret
    volumes:
      - data-db:/var/lib/mysql        # Persistent Volume

volumes:
  data-db: {}
```




### 🤖 PART 4: ANSIBLE CLI
Commands to execute the automation.

```bash
# Connection test (Ping) to all servers
ansible all -m ping -i inventory.ini

# Execute a playbook using inventory
ansible-playbook -i inventory.ini deploy.yml

# Review syntax errors before execution
ansible-playbook --syntax-check deploy.yml

# Create directory structure for a new Ansible Role
ansible-galaxy init <rol_name>
```


### 📜 PART 5: ANSIBLE MODULES
#### Blocks used in Playbooks (tasks:).

##### 📂 Files

```yaml
- name: Directory Management
  file:
    path: /opt/mi-app
    state: directory
    mode: '0755'

- name: Copy static files
  copy:
    src: ./local_file.txt
    dest: /remote/path/file.txt

- name: Copy with vars (Jinja2 templates)
  template:
    src: config.j2
    dest: /etc/config.conf
```

##### ⚙️ System

```yaml
- name: Install packages
  apt:
    name: ['git', 'docker', 'curl']
    state: present
    update_cache: yes

- name: Manage Services
  service:
    name: docker
    state: started
    enabled: yes

- name: Execute CLI commands
  shell: docker compose up -d
  args:
    chdir: /opt/my-app
```




##### 🐳 Docker (Docker Community Modules)
community.docker.docker_compose:

```yaml
- name: Deploy infrastructure
  community.docker.docker_compose:
    project_src: /opt/my-app
    state: present
    build: yes

```

---

### 🛡️ PART 6: NGINX PROXY MANAGER (Reverse Proxy)

Container Management `jc21/nginx-proxy-manager`.

#### 🔑 Default Credentials
* **User:** `admin@example.com`
* **Pass:** `changeme`

#### ⚙️ Docker Compose Configuration
```yaml
  proxy:
    image: 'jc21/nginx-proxy-manager:latest'
    ports:
      - '80:80'    # Web Traffic HTTP
      - '81:81'    # Administration GUI
      - '443:443'  # Secure Traffic HTTPS
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```
