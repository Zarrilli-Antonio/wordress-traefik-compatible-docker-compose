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

## 🧭 Description

This project provides a **ready-to-use WordPress** environment powered by **Docker Compose**, designed to integrate seamlessly with **Traefik** as a reverse proxy.  
It supports **automatic HTTPS certificates**, **phpMyAdmin database management**, and **modular scalability**.

## 🧱 Technology Stack

| Service | Description |
|-----------|-------------|
| 🐬 **MariaDB** | Relational database for WordPress |
| 🧭 **phpMyAdmin** | Web interface for database management |
| 🐘 **PHP-FPM (WordPress)** | PHP interpreter used by WordPress |
| 🚀 **Nginx (within WordPress image)** | Lightweight, high-performance web server |
| 🌍 **Traefik** | External reverse proxy handling SSL and routing |

## ⚙️ Requirements

- **Docker** ≥ 24.x  
- **Docker Compose** ≥ 2.x  
- External Traefik Docker network (default: `frontend`) already created  
- Domain names correctly pointing to your server  
- (Optional) **Cloudflare** API credentials configured for certificate automation  

## 📁 Project Structure

📦 wordpress-traefik-compatible-docker-compose/<br>
├── docker-compose.yml<br>
├── .env.example<br>
├── nginx/<br>
│ └── default.conf<br>
├── site/<br>
│ └── (WordPress core or custom files)<br>
└── README.md<br>

## 🔧 Configuration

1. Copy the example environment file and rename it:

```bash
cp .env.example .env
```

Edit .env and fill in your configuration:


| Variable | Description |
|-----------|-------------|
| MYSQL_ROOT_PASSWORD	| Root password for MariaDB
| MYSQL_DATABASE	| Name of the WordPress database
| MYSQL_USER	| WordPress database username
| MYSQL_PASSWORD	| WordPress database user password
| DOMAIN	| Your main WordPress domain (e.g. example.com)
| DOMAIN_PMA	| Domain for phpMyAdmin (e.g. pma.example.com)
| ROUTERS_WEB_TRAEFIK	| Traefik router name for WordPress
| ROUTERS_PMA_TRAEFIK	| Traefik router name for phpMyAdmin
| FRONTEND_NETWORK	| External Traefik network name (default: frontend)
| DB_VOLUME	| Docker volume name for persistent MariaDB data

## 🚀 Quick Start

Create the external Traefik network if it doesn’t exist:
```bash
docker network create frontend
```
Launch the stack:
```bash
docker compose up -d
```
Check running containers:
```bash
docker ps
```
Access your services:

🌐 WordPress: https://example.com

🧭 phpMyAdmin: https://pma.example.com

(Replace the domains with your actual .env values.)

## 🧰 Useful Commands

### Stop containers
```bash
docker compose down
```

### Rebuild without cache
```bash
docker compose build --no-cache
```

### View logs in real-time
```bash
docker compose logs -f
```

## 🪄 Customizations

🧩 PHP

The wp service uses a PHP-FPM image.<br>
You can customize PHP extensions or configuration via:

./config/wp_php.ini

⚙️ Nginx

Custom configuration files can be placed under:

nginx/default.conf

Use it to set caching, compression, rewrite rules, or headers.

🔐 Traefik

This stack is fully compatible with Traefik (external).<br>
Labels automatically configure routers, entrypoints, and SSL:

```bash
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.${ROUTERS_WEB_TRAEFIK}.rule=Host(`${DOMAIN}`)"
  - "traefik.http.routers.${ROUTERS_WEB_TRAEFIK}.entrypoints=websecure"
  - "traefik.http.routers.${ROUTERS_WEB_TRAEFIK}.tls=true"
  - "traefik.http.routers.${ROUTERS_WEB_TRAEFIK}.tls.certresolver=cloudflare"
```

## 🧩 Volumes & Networks

| Type | Name | Description |
|-----------|-------------|-------------|
| Volume	| ${DB_VOLUME}	| Persistent MariaDB data
| Network	| ${FRONTEND_NETWORK}	E| xternal Traefik network (shared reverse proxy)

## 🔎 Troubleshooting

| Problem | Possible Solution |
|-----------|-------------|
| ❌ Domain not responding	| Check DNS records and Traefik router configuration
| ⚠️ phpMyAdmin not loading	| Ensure DOMAIN_PMA and Traefik labels are correct
| 🧱 Permission denied	| Ensure ./site directory has www-data ownership
| 🔄 SSL not generated	| Check certresolver configuration and Cloudflare credentials

## 🧑‍💻 Author

Antonio Zarrilli

📦 GitHub: Zarrilli-Antonio

💡 Project: wordpress-traefik-compatible-docker-compose

## 🪪 License

This project is distributed under the MIT License.<br>
You may use it freely for personal or commercial projects.
