# *This project has been created as part of the 42 curriculum by marykman.*

# Inception

## Description

Inception is a system administration project whose goal is to introduce containerization using Docker.

The project consists of deploying a small infrastructure composed of multiple services running in isolated Docker containers. Each service has its own dedicated container and communicates with the others through a Docker network.

The infrastructure includes:

* NGINX configured with TLS
* WordPress with PHP-FPM
* MariaDB
* Persistent storage using Docker volumes

The objective is to understand how modern applications are deployed, isolated, networked, and persisted while following best practices regarding security and maintainability.

---

## Project Design

### Why Docker?

Docker allows applications and their dependencies to be packaged into portable containers that can run consistently across different environments.

Benefits include:

* Reproducible deployments
* Isolation between services
* Simplified dependency management
* Lightweight compared to virtual machines

### Project Structure

```text
srcs/
├── docker-compose.yml
├── .env
└── requirements/
    ├── mariadb/
    ├── nginx/
    └── wordpress/
```

Each service has its own Dockerfile, configuration files, and startup scripts.

---

## Technical Choices

### Virtual Machines vs Docker

| Virtual Machines                    | Docker                  |
| ----------------------------------- | ----------------------- |
| Include a complete operating system | Share the host kernel   |
| Higher resource consumption         | Lightweight             |
| Slower startup time                 | Fast startup            |
| Strong hardware-level isolation     | Process-level isolation |

Docker was chosen because it provides efficient isolation while consuming fewer resources.

### Secrets vs Environment Variables

| Secrets                            | Environment Variables             |
| ---------------------------------- | --------------------------------- |
| Designed for sensitive information | Intended for configuration values |
| More secure                        | Easier to expose accidentally     |
| Managed separately from containers | Stored in container environment   |

For simplicity and according to project requirements, environment variables are used to configure the services.

### Docker Network vs Host Network

| Docker Network                                     | Host Network                      |
| -------------------------------------------------- | --------------------------------- |
| Containers communicate through an isolated network | Containers share the host network |
| Better isolation                                   | Less secure                       |
| Internal DNS resolution between services           | Direct access to host interfaces  |

A dedicated Docker bridge network is used to allow communication between services while keeping them isolated from the host.

### Docker Volumes vs Bind Mounts

| Docker Volumes                              | Bind Mounts                    |
| ------------------------------------------- | ------------------------------ |
| Managed by Docker                           | Managed by the host filesystem |
| Portable and independent from host paths    | Depend on specific host paths  |
| Recommended for persistent application data | Useful during development      |

This project uses Docker named volumes to persist MariaDB and WordPress data.

---

## Instructions

### Prerequisites

* Docker
* Docker Compose

### Build and Start

```bash
make
```

### Stop and Remove Containers

```bash
make down
```

### Full Cleanup

```bash
make clear
```

### Access WordPress

Add the following entry to `/etc/hosts`:

```text
127.0.0.1 marykman.42.fr
```

Then access:

```text
https://marykman.42.fr
```

Administration panel:

```text
https://marykman.42.fr/wp-admin
```

---

## Resources

### Docker

* Docker Official Documentation
  https://docs.docker.com/

* Docker Compose Documentation
  https://docs.docker.com/compose/

### NGINX

* https://nginx.org/en/docs/

### MariaDB

* https://mariadb.com/kb/en/documentation/

### WordPress

* https://wordpress.org/documentation/

### PHP-FPM

* https://www.php.net/manual/en/install.fpm.php

---

## AI Usage

Artificial Intelligence tools were used as learning aids during the development of this project.

AI assistance was limited to:

* Understanding Docker concepts
* Clarifying Docker networking and volumes
* Reviewing shell scripts
* Learning best practices regarding containerization

All architecture decisions, implementation, debugging, configuration, and testing were performed manually by the project authors.
