# User Documentation — Inception Stack

## Overview

This project deploys a small web infrastructure using Docker containers.
It provides a fully working WordPress website served through an NGINX reverse proxy, backed by a MariaDB database.

### Services included

* **NGINX**: HTTPS web server (port 443)
* **WordPress**: CMS running with PHP-FPM
* **MariaDB**: Database storing WordPress data

All services run in isolated Docker containers connected through a private Docker network.

---

## Starting the project

```bash
make
```

## Stopping the project



```bash
make down
```


---

## Accessing the website

Before accessing the site, ensure the domain is mapped locally:

```text
127.0.0.1 marykman.42.fr
```

Then open:

```text
https://marykman.42.fr
```

> You may need to accept the self-signed TLS certificate.

---

## WordPress administration panel

Access the admin interface at:

```text
https://marykman.42.fr/wp-admin
```

Login using the admin credentials defined in the environment variables.

---

## Credentials management

All credentials are defined in the `.env` file used by Docker Compose.

Typical variables include:

* SQL database name
* SQL user and password
* WordPress admin username and password
* WordPress user credentials

⚠️ These credentials are injected into containers at startup using environment variables.

---

## Checking if services are running

### 1. Check running containers

```bash
docker ps
```

You should see:

* nginx
* wordpress
* mariadb

---

### 2. Check logs

```bash
docker compose logs
```

Or for a specific service:

```bash
docker logs nginx
docker logs wordpress
docker logs mariadb
```

---

### 3. Test connectivity

From the host:

```bash
curl -k https://mylogin.42.fr
```

If WordPress loads, the stack is working correctly.

---

### 4. Check Docker network

```bash
docker network ls
docker network inspect srcs_inception
```

---

## Troubleshooting

* If the site is not reachable, check `/etc/hosts`
* If WordPress fails, check MariaDB logs first
* If login fails, verify `.env` credentials
* If services restart repeatedly, check container logs

---

## Summary

This stack provides a minimal but complete web infrastructure using Docker:

* Secure HTTPS entry point (NGINX)
* Dynamic web application (WordPress)
* Persistent relational database (MariaDB)
* Fully containerized architecture
