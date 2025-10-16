<h1 align="center">🐳 WordPress + Nginx + MariaDB + Traefik (Docker Compose)</h1>

<p align="center">
  <strong>Docker environment optimized for WordPress with Traefik support, automatic HTTPS, and complete management via phpMyAdmin.</strong>
</p>

<p align="center">
  <a href="https://www.docker.com/"><img src="https://img.shields.io/badge/Docker-24%2B-blue?logo=docker&logoColor=white" alt="Docker"></a>
  <a href="https://traefik.io/"><img src="https://img.shields.io/badge/Traefik-compatible-24A1C1?logo=traefikproxy&logoColor=white" alt="Traefik Compatible"></a>
  <a href="https://nginx.org/"><img src="https://img.shields.io/badge/Nginx-stable-green?logo=nginx&logoColor=white" alt="Nginx"></a>
  <a href="https://mariadb.org/"><img src="https://img.shields.io/badge/MariaDB-10.6-blue?logo=mariadb&logoColor=white" alt="MariaDB"></a>
  <a href="https://www.phpmyadmin.net/"><img src="https://img.shields.io/badge/phpMyAdmin-5.2-orange?logo=phpmyadmin&logoColor=white" alt="phpMyAdmin"></a>
</p>

---

## 🧭 Description

This project provides a **ready-to-use WordPress** environment in **Docker Compose**, compatible with **Traefik** as a reverse proxy.  
It is designed for **self-hosted** environments that require **automatic HTTPS**, **DB management via phpMyAdmin**, and **modular scalability**.

---

## 🧱 Technology Stack

| Service | Description |
|-----------|-------------|
| 🐬 **MariaDB** | Relational database for WordPress |
| 🧭 **phpMyAdmin** | Web interface for database management |
| 🐘 **PHP-FPM** | PHP interpreter for WordPress |
| 🚀 **Nginx** | Lightweight, high-performance web server |
| 🌍 **Traefik** | Reverse proxy with automatic SSL certificate management (external to compose) |

---

## ⚙️ Requirements

- **Docker** ≥ 24.x
- **Docker Compose** ≥ 2.x
- **External Traefik** Docker network (`frontend`) already configured
- Domain and DNS correctly pointed to the server
- (Optional) **Cloudflare** account configured for TLS automation  

---

## 📁 Project Structure

📦 wordpress-traefik-compatible-docker-compose/<br>
├── docker-compose.yml<br>
├── .env.example<br>
├── nginx/<br>
│ └── default.conf<br>
├── site/<br>
│ └── (WordPress or PHP file)<br>
└── README.md<br>


---

## 🔧 Configuration

1. Copy the sample file `.env.example` and rename it:

```bash
     cp .env.example .env
   ```

Modify the variables in the .env file according to your needs:


| Variable | Description |
|-----------|-------------|
| MYSQL_ROOT_PASSWORD | Root password for MariaDB |
| MYSQL_DATABASE | WordPress database name |
| MYSQL_USER | Database user |
| MYSQL_PASSWORD | Database user password |
| DOMAIN | WordPress domain |
| DOMAIN_PMA | Traefik router for WordPress |
| ROUTERS_WEB_TRAEFIK | Traefik router for WordPress |
| ROUTERS_PMA_TRAEFIK | Traefik router for phpMyAdmin |
| FRONTEND_NETWORK | External Traefik network name |
| DB_VOLUME | Docker volume for the database |

## 🚀 Quick Start

1. Create the Traefik network (if it doesn't exist):

```bash
  docker network create frontend
```


2. Start the containers:

```bash
  docker compose up -d
```

3. Verify that everything is working:

```bash
  docker ps
```

4. Access the services:

🌐 WordPress / Nginx: https://test.domain.com

🧭 phpMyAdmin: https://testpma.domain.com

(Replace the domains with those set in your .env)

## 🧰 Useful Commands

# Stop containers

    docker compose down

# Rebuild images without cache

    docker compose build --no-cache

# Show logs in real time

    docker compose logs -f

## 🪄 Customizations

🧩 PHP

  The php-fpm service is built from a local Dockerfile.
  You can add PHP extensions, libraries, or modify PHP.ini settings.

⚙️ Nginx

  All configuration is in nginx/default.conf.
  Here you can define rewrite rules, caching, gzip, security headers, etc.

🔐 Traefik

  Not included in the project, but fully compatible.
  Labels automatically configure routers, entry points, and SSL certificates:

    labels:
      - “traefik.enable=true”
      - “traefik.http.routers.${ROUTERS_WEB_TRAEFIK}.rule=Host(`${DOMAIN}`)”
      - “traefik.http.routers.${ROUTERS_WEB_TRAEFIK}.entrypoints=websecure”
      - “traefik.http.routers.${ROUTERS_WEB_TRAEFIK}.tls=true”
      - “traefik.http.routers.${ROUTERS_WEB_TRAEFIK}.tls.certresolver=cloudflare”

## 🧩 Volumes & Network

| Type | Name | Description |
|-----------|-------------|-------------|
| Volume | ${DB_VOLUME} | Persistent MariaDB data |
| Network | ${FRONTEND_NETWORK} | Shared network with Traefik |


## 🔎 Troubleshooting

| Problem | Possible solution |
|-----------|-------------|
| ❌ Domain not responding | Check DNS and Traefik configuration |
| ⚠️ phpMyAdmin does not open | Check labels and DOMAIN_PMA domain |
| 🧱 Permission error | Ensure that ./site has www-data permissions |
| 🔄 SSL certificate not generated | Check tls.certresolver configuration and Cloudflare credentials |

## 🧑‍💻 Author

Antonio Zarrilli

📦 GitHub: Zarrilli-Antonio<br>

💡 Project: wordpress-traefik-compatible-docker-compose

## 🪪 License

This project is distributed under the MIT license.<br>
You can use it freely for personal or commercial projects.
