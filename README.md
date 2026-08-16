# WordPress Docker Environment

A simple and reusable Docker environment for running **WordPress + MariaDB + phpMyAdmin** using Docker Compose.

This project can be used as a starting point for creating a fresh WordPress website locally or on a server.

## 🚀 Stack

This project uses:

* WordPress
* MariaDB
* phpMyAdmin
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

Contains the environment variables used by Docker Compose.

Current configuration:

```env
# Database Configuration
DB_ROOT_PASSWORD=password
DB_NAME=wordpress
DB_USER=root
DB_PASSWORD=password

# Container Names
DB_CONTAINER_NAME=wordpress_db
WP_CONTAINER_NAME=wordpress_site
PMA_CONTAINER_NAME=wordpress_phpmyadmin

# WordPress Configuration
WP_PORT=8080

# phpMyAdmin Configuration
PMA_PORT=8081
```

#### `docker-compose.yml`

Defines the Docker services:

* MariaDB
* WordPress
* phpMyAdmin

It also creates the Docker network and persistent volumes.

#### `uploads.ini`

Contains custom PHP configuration for WordPress, such as upload size, memory limit, and execution time.

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

Make sure the following are installed:

* Docker
* Docker Compose

Check Docker:

```bash
docker --version
```

Check Docker Compose:

```bash
docker compose version
```

---

# 🛠️ Installation

## 1. Clone the Repository

Clone this repository:

```bash
git clone YOUR_REPOSITORY_URL
```

Enter the project directory:

```bash
cd docker-wp
```

---

## 2. Configure `.env`

The default `.env` configuration is:

```env
# Database Configuration
DB_ROOT_PASSWORD=password
DB_NAME=wordpress
DB_USER=root
DB_PASSWORD=password

# Container Names
DB_CONTAINER_NAME=wordpress_db
WP_CONTAINER_NAME=wordpress_site
PMA_CONTAINER_NAME=wordpress_phpmyadmin

# WordPress Configuration
WP_PORT=8080

# phpMyAdmin Configuration
PMA_PORT=8081
```

### Database Configuration

| Variable           | Value       | Description                 |
| ------------------ | ----------- | --------------------------- |
| `DB_ROOT_PASSWORD` | `password`  | MariaDB root password       |
| `DB_NAME`          | `wordpress` | WordPress database name     |
| `DB_USER`          | `root`      | WordPress database username |
| `DB_PASSWORD`      | `password`  | WordPress database password |

### Container Names

| Variable             | Value                  |
| -------------------- | ---------------------- |
| `DB_CONTAINER_NAME`  | `wordpress_db`         |
| `WP_CONTAINER_NAME`  | `wordpress_site`       |
| `PMA_CONTAINER_NAME` | `wordpress_phpmyadmin` |

### Ports

| Variable   |   Port |
| ---------- | -----: |
| `WP_PORT`  | `8080` |
| `PMA_PORT` | `8081` |

---

# ▶️ Start WordPress

From the project directory, run:

```bash
docker compose up -d
```

Check the containers:

```bash
docker compose ps
```

You should see:

```text
wordpress_db
wordpress_site
wordpress_phpmyadmin
```

All containers should have a status similar to:

```text
Up
```

---

# 🌐 Access WordPress

Open your browser and visit:

```text
http://localhost:8080
```

You should see the WordPress installation screen.

Complete the WordPress setup normally.

---

# 🗄️ Access phpMyAdmin

Open:

```text
http://localhost:8081
```

The database server is:

```text
db
```

Using the default `.env` configuration, you can log in with:

```text
Username: root
Password: password
```

The WordPress database will be:

```text
wordpress
```

---

# 🔌 WordPress Database Connection

WordPress connects to MariaDB through the Docker network.

The database host should be:

```yaml
WORDPRESS_DB_HOST: db:3306
```

Do **not** use:

```text
localhost
```

The Docker architecture is:

```text
WordPress Container
        │
        │ db:3306
        ▼
MariaDB Container
```

The service name:

```text
db
```

is automatically resolved by Docker's internal network.

---

# 💾 Persistent Data

The Docker Compose configuration uses persistent volumes.

Example:

```yaml
volumes:
  - db_data:/var/lib/mysql
  - wp_data:/var/www/html
```

This means WordPress files and the MariaDB database persist even if the containers are stopped or recreated.

---

# 🛑 Stop WordPress

To stop the environment:

```bash
docker compose down
```

This removes the containers but keeps the persistent volumes.

Start the environment again:

```bash
docker compose up -d
```

---

# 🔄 Restart WordPress

```bash
docker compose restart
```

---

# 🔍 Useful Commands

### Check containers

```bash
docker compose ps
```

### View all running containers

```bash
docker ps
```

### WordPress logs

```bash
docker logs wordpress_site
```

### MariaDB logs

```bash
docker logs wordpress_db
```

### phpMyAdmin logs

```bash
docker logs wordpress_phpmyadmin
```

### Follow WordPress logs

```bash
docker logs -f wordpress_site
```

### Follow MariaDB logs

```bash
docker logs -f wordpress_db
```

---

# 🧹 Reset the WordPress Installation

If this is a test installation and you want to completely start over:

```bash
docker compose down -v
```

Then:

```bash
docker compose up -d
```

This will create a completely new MariaDB database and WordPress installation.

> ⚠️ **Warning:** `docker compose down -v` deletes the Docker volumes. This means the WordPress database and stored WordPress files will be deleted.

Do not use this command on a production website unless you have a backup.

---

# 🔐 Security

The default configuration uses simple development passwords:

```text
password
```

This configuration is intended for **local development and testing**.

For production environments:

* Use strong passwords.
* Do not expose MariaDB directly to the internet.
* Restrict phpMyAdmin access.
* Use HTTPS.
* Keep Docker images updated.
* Create regular backups.
* Use separate database users for WordPress.
* Never publish production credentials to GitHub.

For a public GitHub repository, it is recommended to use:

```text
.env.example
```

instead of committing real credentials in:

```text
.env
```

---

# 📌 Default URLs

| Service    | URL                   |
| ---------- | --------------------- |
| WordPress  | http://localhost:8080 |
| phpMyAdmin | http://localhost:8081 |

---

# 🧱 Creating Another WordPress Installation

This project can be duplicated to create another independent WordPress installation.

Copy the project into a new directory and change the following values in `.env`:

```env
DB_NAME=wordpress2
DB_CONTAINER_NAME=wordpress2_db
WP_CONTAINER_NAME=wordpress2_site
PMA_CONTAINER_NAME=wordpress2_phpmyadmin

WP_PORT=8090
PMA_PORT=8091
```

Make sure the container names and ports are different from the existing installation.

Then run:

```bash
docker compose up -d
```

The new WordPress installation will be available at:

```text
http://localhost:8090
```

---

# 🚀 Future Improvements

This project can later be extended to support:

* Multiple WordPress websites
* Multiple databases
* WooCommerce
* Nginx Proxy Manager
* Custom domains
* HTTPS / SSL
* Redis Object Cache
* Automated backups
* Development and production configurations

---

## 📄 License

This project is provided for development and testing purposes.
