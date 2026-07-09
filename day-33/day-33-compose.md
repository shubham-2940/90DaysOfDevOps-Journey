# Day 33 – Docker Compose: Multi-Container Basics

## Objective

The goal of Day 33 was to learn **Docker Compose**, a tool that simplifies running multi-container applications using a single YAML configuration file.

Instead of manually creating containers, networks, and volumes using multiple Docker commands, Docker Compose allows everything to be defined in one `docker-compose.yml` file and managed with a single command.

---

# Task 1: Install & Verify Docker Compose

### Check Docker Compose Version

```bash
docker compose version
```

### Check Docker Version

```bash
docker --version
```

### Outcome

- Verified Docker Compose is installed.
- Confirmed Docker Engine is working properly.

**Screenshot:**

`images/compose-version.png`

---

# Task 2: First Docker Compose Project

## Created Project Directory

```bash
mkdir compose-basics
cd compose-basics
```

## docker-compose.yml

```yaml
services:
  nginx:
    image: nginx:latest
    container_name: nginx-compose
    ports:
      - "8080:80"
```

## Start Container

```bash
docker compose up
```

or

```bash
docker compose up -d
```

## Verify Running Containers

```bash
docker compose ps
```

## Access Nginx

```
http://<EC2-Public-IP>:8080
```

## Stop Containers

```bash
docker compose down
```

### Outcome

- Successfully created first Docker Compose project.
- Nginx launched with a single command.
- Accessed the default Nginx page in the browser.

**Screenshots**

- compose-up.png
- nginx-browser.png
- compose-down.png

---

# Task 3: Multi-Container Application (WordPress + MariaDB)

## Objective

Create a complete application using two containers:

- WordPress
- MariaDB

Compose automatically creates a shared network allowing both containers to communicate using service names.

---

## docker-compose.yml

```yaml
services:
  db:
    image: mariadb:11
    container_name: shubham29-db
    restart: always
    environment:
      MARIADB_ROOT_PASSWORD: root
      MARIADB_DATABASE: wordpress
      MARIADB_USER: wordpressuser
      MARIADB_PASSWORD: root
    volumes:
      - mydb_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: shubham29-wordpress
    restart: always
    depends_on:
      - db
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpressuser
      WORDPRESS_DB_PASSWORD: root
      WORDPRESS_DB_NAME: wordpress

volumes:
  mydb_data:
```

---

## Start Containers

```bash
docker compose up -d
```

---

## Verify Running Containers

```bash
docker compose ps
```

---

## Access WordPress

```
http://<EC2-Public-IP>:8080
```

Completed the WordPress installation wizard successfully.

---

## Verify Data Persistence

Stopped containers

```bash
docker compose down
```

Started them again

```bash
docker compose up -d
```

Verified that:

- WordPress site was still available.
- Database was preserved.
- Named volume successfully stored all MySQL/MariaDB data.

---

### What I Learned

- Compose automatically creates a network.
- Services communicate using service names.
- Named volumes persist data.
- One command launches the complete application stack.

**Screenshots**

- wordpress-running.png
- compose-ps.png
- wordpress-dashboard.png
- volume-ls.png

---

# Task 4: Docker Compose Commands

## Start Services

```bash
docker compose up -d
```

Starts all services in detached mode.

---

## View Running Services

```bash
docker compose ps
```

Lists running Compose services.

---

## View All Logs

```bash
docker compose logs
```

Displays logs from every service.

---

## View Logs of Specific Service

```bash
docker compose logs db
```

or

```bash
docker compose logs wordpress
```

---

## Stop Services

```bash
docker compose stop
```

Stops running containers without removing them.

---

## Restart Services

```bash
docker compose start
```

Starts previously stopped containers.

---

## Remove Containers & Network

```bash
docker compose down
```

Removes containers and Compose network.

---

## Remove Containers, Network & Volumes

```bash
docker compose down -v
```

Deletes persistent volumes.

---

## Rebuild Services

```bash
docker compose up --build -d
```

Rebuilds images before starting containers.

---

# Task 5: Environment Variables

Instead of hardcoding credentials inside Compose files, Docker Compose supports loading variables from a `.env` file.

---

## .env File

```env
DB_ROOT_PASSWORD=root
DB_NAME=wordpress
DB_USER=wordpressuser
DB_PASSWORD=root
WP_PORT=8080
```

---

## docker-compose.yml

```yaml
environment:
  MARIADB_ROOT_PASSWORD: ${DB_ROOT_PASSWORD}
  MARIADB_DATABASE: ${DB_NAME}
  MARIADB_USER: ${DB_USER}
  MARIADB_PASSWORD: ${DB_PASSWORD}
```

WordPress

```yaml
environment:
  WORDPRESS_DB_HOST: db:3306
  WORDPRESS_DB_USER: ${DB_USER}
  WORDPRESS_DB_PASSWORD: ${DB_PASSWORD}
  WORDPRESS_DB_NAME: ${DB_NAME}
```

---

## Verify Variables

```bash
docker compose config
```

Docker Compose replaced all placeholders with values from the `.env` file.

Also verified inside the container:

```bash
docker exec shubham29-wordpress env
```

---

# Key Concepts Learned

### Docker Compose

Runs multiple containers using a single configuration file.

---

### Services

Each container is defined as a service.

Example:

- WordPress
- MariaDB

---

### Networks

Compose automatically creates a bridge network.

Containers communicate using service names.

Example:

```
WORDPRESS_DB_HOST=db:3306
```

No IP address is required.

---

### Volumes

Named volumes store persistent data.

Example:

```
mydb_data
```

Data remains even after containers are removed.

---

### Environment Variables

Used to separate configuration from application code.

Benefits:

- Better security
- Easier maintenance
- Reusable Compose files

---

# Challenges Faced

- Incorrect volume syntax in Compose file.
- YAML indentation issues.
- MySQL image compatibility problem on the latest Ubuntu environment.
- Switched to MariaDB, which is fully compatible with WordPress.
- Learned how to debug Docker Compose using logs and service status.

---

# Key Takeaways

- Docker Compose simplifies multi-container application deployment.
- One YAML file replaces multiple Docker commands.
- Services communicate automatically through Compose networking.
- Named volumes ensure persistent storage.
- Environment variables improve security and flexibility.
- Docker Compose is an essential tool for local development and DevOps workflows.

---

# Files Created

```
2026/day-33/
│
├── day-33-compose.md
├── compose-basics/
│   └── docker-compose.yml
├── wordpress-compose/
│   ├── docker-compose.yml
│   └── .env
└── images/
```

---

# Conclusion

Day 33 introduced Docker Compose, making it significantly easier to deploy and manage multi-container applications. I learned how to define services, networks, volumes, and environment variables in a single configuration file while deploying a complete WordPress and MariaDB application stack. This hands-on practice reinforced real-world container orchestration concepts that are widely used in development and DevOps environments.
