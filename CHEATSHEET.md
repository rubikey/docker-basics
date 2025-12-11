# 📘 CHEATSHEET DEVOPS: DOCKER & ANSIBLE

Este documento es una referencia rápida para SysAdmins sobre comandos y módulos esenciales.

---

## 🐳 PARTE 1: DOCKER CLI (Comandos de Terminal)

Comandos para gestionar contenedores e imágenes manualmente.

### 🟢 Ciclo de Vida del Contenedor
```bash
docker run -d -p 80:80 nginx   # Descarga, crea y arranca en 2º plano
docker ps                      # Muestra contenedores CORRIENDO
docker ps -a                   # Muestra TODOS (incluidos apagados)
docker stop <nombre>           # Detiene un contenedor
docker start <nombre>          # Enciende un contenedor apagado
docker restart <nombre>        # Reinicio rápido
docker rm <nombre>             # Borra un contenedor (debe estar detenido)
docker rm -f <nombre>          # Fuerza el borrado inmediato

### 📦 Gestión de Imágenes
docker images                  # Lista imágenes descargadas
docker pull <imagen>:<tag>     # Descarga sin correr (ej: ubuntu:24.04)
docker rmi <imagen>            # Borra una imagen del disco
docker build -t <nombre> .     # Construye imagen desde Dockerfile actual

### 🕵️ Debugging y Mantenimiento
docker logs <nombre>           # Muestra logs internos
docker logs -f <nombre>        # Logs en vivo (como tail -f)
docker exec -it <nombre> sh    # Entrar dentro del contenedor (terminal)
docker inspect <nombre>        # Ver metadata (IP, volúmenes) en JSON
docker stats                   # Ver consumo CPU/RAM en vivo
docker system prune -a         # ¡PELIGRO! Limpieza total (libera espacio)
```
---

### 🏗️ PARTE 2: DOCKERFILE

Instrucciones para crear imágenes personalizadas.

| **FROM** | Imagen base (Obligatorio). | `FROM python:3.9-alpine` |

| **WORKDIR** | Crea y entra a carpeta interna. | `WORKDIR /app` |

| **COPY** | Copia archivos de tu PC al contenedor. | `COPY . .` |

| **RUN** | Ejecuta comandos durante la construcción. | `RUN apt-get update` |

| **ENV** | Define variables de entorno. | `ENV PORT=8080` |

| **EXPOSE** | Documenta el puerto de escucha. | `EXPOSE 80` |

| **CMD** | Comando de arranque por defecto. | `CMD ["python", "app.py"]` |

---

### 🐙 PARTE 3: DOCKER COMPOSE (La Orquestación)

Estructura básica de `docker-compose.yml`.

```yaml
version: "3.8"

services:
  mi-app:
    container_name: web-server
    build: .                  # Construir desde Dockerfile local
    ports:
      - "8080:80"             # Host:Contenedor
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
      - data-db:/var/lib/mysql        # Volumen Persistente

volumes:
  data-db: {}
```




### 🤖 PARTE 4: ANSIBLE CLI
Comandos para ejecutar automatizaciones.

```bash
# Prueba de conexión (Ping) a todos los servidores
ansible all -m ping -i inventory.ini

# Ejecutar un Playbook completo
ansible-playbook -i inventory.ini deploy.yml

# Revisar errores de sintaxis antes de ejecutar
ansible-playbook --syntax-check deploy.yml

# Crear estructura de carpetas para un nuevo Role
ansible-galaxy init <nombre_del_rol>
```


### 📜 PARTE 5: MÓDULOS DE ANSIBLE
#### Bloques usados dentro de los Playbooks (tasks:).

##### 📂 Archivos

```yaml
- name: Gestión de directorios
  file:
    path: /opt/mi-app
    state: directory
    mode: '0755'

- name: Copiar archivos estáticos
  copy:
    src: ./archivo-local.txt
    dest: /ruta/remota/archivo.txt

- name: Copiar con variables (Plantillas)
  template:
    src: config.j2
    dest: /etc/config.conf
```

##### ⚙️ Sistema

```yaml
- name: Instalar paquetes
  apt:
    name: ['git', 'docker', 'curl']
    state: present
    update_cache: yes

- name: Gestionar servicios
  service:
    name: docker
    state: started
    enabled: yes

- name: Ejecutar comandos de terminal
  shell: docker compose up -d
  args:
    chdir: /opt/mi-app
```




##### 🐳 Docker (Módulos Community)
community.docker.docker_compose:

```yaml
- name: Levantar infraestructura
  community.docker.docker_compose:
    project_src: /opt/mi-app
    state: present
    build: yes

```