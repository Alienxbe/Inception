# Developer Documentation — Inception Stack

## Overview

This document explains how to set up, build, and manage the Inception Docker-based infrastructure from a developer perspective.

The project is composed of three main services:

* NGINX (TLS reverse proxy)
* WordPress (PHP-FPM application)
* MariaDB (database server)

All services are orchestrated using Docker Compose.

---

## Prerequisites

Before starting, ensure the following tools are installed:

* Docker
* Docker Compose plugin
* Make (if using the provided Makefile)
* A Linux environment (recommended Debian-based system)

---

## Environment Setup

### 1. Clone the repository

```bash id="k0v6pz"
git clone https://github.com/Alienxbe/Inception.git
cd Inception
```

---

### 2. Configure environment variables

Create or edit the `.env` file located at the root of the project:

```text id="w3x2qz"
SQL_DATABASE=wordpress
SQL_USER=your_user
SQL_PASSWORD=your_password
SQL_ROOT_PASSWORD=your_root_password

WP_ADMIN_USER=admin
WP_ADMIN_PASSWORD=admin_password
WP_ADMIN_EMAIL=admin@example.com

WP_USER=user
WP_USER_PASSWORD=user_password
WP_USER_EMAIL=user@example.com

DOMAIN_NAME=mylogin.42.fr
```

⚠️ These values are injected into containers at runtime.

---

### 3. Host configuration

Add your local domain mapping:

```text id="u8n4qp"
127.0.0.1 mylogin.42.fr
```

---

## Build and Launch

### Using Docker Compose

Build and start all services:

```bash id="x1m9pl"
docker compose up --build
```

Run in background:

```bash id="b7q3zd"
docker compose up -d
```

---

### Using Makefile

```bash id="k8v0ab"
make
```

---

## Container Management

### View running containers

```bash id="m2n5ty"
docker ps
```

---

### Stop services

```bash id="z9v1ks"
docker compose stop
```

---

### Stop and remove containers

```bash id="p6q0ld"
docker compose down
```

---

### Full reset (containers + network + volumes)

```bash id="h3w8zc"
docker compose down -v
```

---

### Rebuild everything from scratch

```bash id="r8t2qa"
docker compose down -v
docker compose up --build
```

---

## Data Persistence

This project uses **Docker named volumes** to persist data across container restarts.

### Stored data

| Service   | Volume         | Purpose                 |
| --------- | -------------- | ----------------------- |
| MariaDB   | mariadb_data   | Database files          |
| WordPress | wordpress_data | Website files & uploads |

---

### Where data is stored

```text id="v5x9kd"
/home/marykman/data/
```

These volumes are managed by Docker and persist even if containers are recreated.

---

### Inspect volumes

```bash id="t4q8bn"
docker volume ls
docker volume inspect mariadb_data
```

---

## Project Architecture

### Key design choices

* Each service runs in a dedicated container
* Communication is handled through a private Docker bridge network
* Data persistence is handled via Docker named volumes
* TLS is configured at the NGINX layer only

---

## Useful Commands

### Logs

```bash id="l9p2wx"
docker compose logs
docker logs nginx
docker logs wordpress
docker logs mariadb
```

---

### Access container shell

```bash id="s7d1mn"
docker exec -it wordpress bash
```

---

### Inspect network

```bash id="q1v6ta"
docker network inspect srcs_inception
```

---

## Data Reset Strategy

To fully reset the environment:

```bash id="f8k3lp"
docker compose down -v
docker system prune -a
```

---

## Notes

* WordPress initialization runs only if `wp-config.php` is missing
* MariaDB initializes only on first startup with empty volume
* Rebuilding images does not reset persistent data
