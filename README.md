# WordPress Docker Environment

A simple and reusable Docker environment for running **WordPress + MariaDB + phpMyAdmin** locally.

This project is designed to make it easy to create a fresh WordPress installation using Docker without installing PHP, MariaDB/MySQL, or phpMyAdmin directly on the host machine.

## 🚀 Stack

This project uses:

* [WordPress](https://hub.docker.com/_/wordpress)
* [MariaDB](https://hub.docker.com/_/mariadb)
* [phpMyAdmin](https://hub.docker.com/_/phpmyadmin)
* Docker
* Docker Compose

## 📁 Project Structure

```text
docker-wp/
├── .env
├── docker-compose.yml
├── uploads.ini
└── README.md
```

### Files

#### `.env`

Contains the environment variables used by Docker Compose, including:

* Database name
* Database username
* Database passwords
* Container names
* WordPress port
* phpMyAdmin port

> Do not commit real production passwords or sensitive credentials to GitHub.

#### `docker-compose.yml`

Defines the Docker services:

* MariaDB
* WordPress
* phpMyAdmin

It also creates the Docker network and persistent volumes.

#### `uploads.ini`

Custom PHP configuration for WordPress, mainly used for increasing upload and PHP limits.

Example:

```ini
upload_max_filesize = 128M
post_max_size = 128M
memory_limit = 256M
max_execution_time = 300
max_input_time = 300
```

---

# ⚙️ Requirements

Before starting, make sure you have:

* Docker
* Docker Compose

Check your installation:

```bash
docker --version
```

```bash
docker compose version
```

---

# 🛠️ Installation

## 1. Clone the repository

Clone the project from GitHub:

```bash
git clone YOUR_REPOSITORY_URL
```

Enter the project directory:

```bash
cd docker-wp
```

---

## 2. Configure `.env`

Create or edit the `.env` file.

Example:

```env
# Database Configuration
DB_ROOT_PASSWORD=password
DB_NAME=woo_db
DB_USER=woo_user
DB_PASSWORD=root

# Container Names
DB_CONTAINER_NAME=woo_db
WP_CONTAINER_NAME=woo_site
PMA_CONTAINER_NAME=woo_phpmyadmin

# WordPress Configuration
WP_PORT=8080

# phpMyAdmin Configuration
PMA_PORT=8081
```

### Database variables

| Variable           | Description                      |
| ------------------ | -------------------------------- |
| `DB_ROOT_PASSWORD` | MariaDB root password            |
| `DB_NAME`          | WordPress database name          |
| `DB_USER`          | WordPress database user          |
| `DB_PASSWORD`      | WordPress database user password |

### Container variables

| Variable             | Description               |
| -------------------- | ------------------------- |
| `DB_CONTAINER_NAME`  | MariaDB container name    |
| `WP_CONTAINER_NAME`  | WordPress container name  |
| `PMA_CONTAINER_NAME` | phpMyAdmin container name |

### Ports

| Variable   | Description          |
| ---------- | -------------------- |
| `WP_PORT`  | WordPress HTTP port  |
| `PMA_PORT` | phpMyAdmin HTTP port |

---

# ▶️ Start the Environment

Run:

```bash
docker compose up -d
```

Check running containers:

```bash
docker compose ps
```

You should see:

```text
woo_db
woo_site
woo_phpmyadmin
```

---

# 🌐 Access WordPress

Open:

```text
http://localhost:8080
```

You should see the WordPress installation screen.

Complete the WordPress installation normally.

---

# 🗄️ Access phpMyAdmin

Open:

```text
http://localhost:8081
```

For the database server, use:

```text
db
```

You can log in using the database credentials defined in `.env`.

For example:

```text
Username: woo_user
Password: root
```

Or use the MariaDB root account:

```text
Username: root
Password: password
```

---

# 🔌 Database Connection

WordPress connects to MariaDB using the Docker service name:

```yaml
WORDPRESS_DB_HOST: db:3306
```

Do **not** use:

```text
localhost
```

The reason is that WordPress and MariaDB run inside separate Docker containers.

Docker provides an internal network where the service:

```text
db
```

resolves to the MariaDB container.

The connection looks like:

```text
WordPress
    │
    │ db:3306
    ▼
MariaDB
```

---

# 💾 Persistent Data

The Docker Compose configuration uses persistent volumes for WordPress and MariaDB.

Example:

```yaml
volumes:
  - db_data:/var/lib/mysql
  - wp_data:/var/www/html
```

This means that restarting or recreating containers does not automatically remove the WordPress files or database.

---

# 🛑 Stop the Environment

To stop the containers:

```bash
docker compose down
```

The containers will be removed, but persistent volumes remain.

Start them again with:

```bash
docker compose up -d
```

---

# ⚠️ Reset the Environment

For a completely fresh installation:

```bash
docker compose down -v
```

Then:

```bash
docker compose up -d
```

> **Warning:** `docker compose down -v` removes the Docker volumes, including the WordPress database and WordPress files stored in those volumes.

Use this only when you want to completely reset the environment.

---

# 🔍 Useful Commands

### View containers

```bash
docker compose ps
```

### View all containers

```bash
docker ps
```

### View WordPress logs

```bash
docker logs woo_site
```

### View MariaDB logs

```bash
docker logs woo_db
```

### View phpMyAdmin logs

```bash
docker logs woo_phpmyadmin
```

### Follow WordPress logs

```bash
docker logs -f woo_site
```

### Restart the environment

```bash
docker compose restart
```

---

# 🧹 Remove Containers

```bash
docker compose down
```

This removes the containers but keeps the persistent volumes.

---

# 🔐 Security

This project is primarily intended for **local development and testing**.

Do not use the example passwords in a production environment.

For production:

* Use strong passwords.
* Do not commit `.env` files containing real credentials.
* Use HTTPS.
* Restrict phpMyAdmin access.
* Use a firewall.
* Keep Docker images updated.
* Use regular backups.
* Do not expose MariaDB directly to the public internet.

---

# 🚫 Git Ignore

If this repository is public, consider adding `.env` to `.gitignore`:

```gitignore
.env
```

Then create an example environment file:

```text
.env.example
```

Example:

```env
DB_ROOT_PASSWORD=change_me
DB_NAME=wordpress_db
DB_USER=wordpress_user
DB_PASSWORD=change_me

DB_CONTAINER_NAME=wordpress_db
WP_CONTAINER_NAME=wordpress_site
PMA_CONTAINER_NAME=wordpress_phpmyadmin

WP_PORT=8080
PMA_PORT=8081
```

This allows other developers to create their own `.env` file without exposing your credentials.

---

# 🔄 Updating Docker Images

To download the latest images:

```bash
docker compose pull
```

Then recreate the containers:

```bash
docker compose up -d
```

---

# 📌 Default URLs

| Service    | URL                   |
| ---------- | --------------------- |
| WordPress  | http://localhost:8080 |
| phpMyAdmin | http://localhost:8081 |

Ports can be changed through the `.env` file.

---

# 🧩 Future Improvements

This project can be extended to support:

* Multiple WordPress installations
* Multiple databases
* Nginx Proxy Manager
* Custom domains
* HTTPS / SSL
* WordPress Multisite
* WooCommerce
* Redis / Object Cache
* Automated backups
* Development and production configurations

---

## 📄 License

This project is provided for development and testing purposes.
