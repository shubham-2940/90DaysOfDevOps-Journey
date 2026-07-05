# Day 32 – Docker Volumes & Networking

## Objective

Today's goal was to understand two essential Docker concepts:

- **Docker Volumes** – Persist data even after containers are removed.
- **Docker Networking** – Enable communication between containers.

These features are widely used in real-world applications where multiple containers (frontend, backend, database, etc.) need to communicate while keeping application data safe.

---

# Task 1 – The Problem (Data Loss Without Volumes)

## Objective

Understand what happens to container data when a container is removed without using a Docker volume.

---

## Step 1: Run a MySQL Container

```bash
docker run -d \
--name mysql-db \
-e MYSQL_ROOT_PASSWORD=admin123 \
-p 3306:3306 \
mysql:latest
```

---

## Step 2: Connect to MySQL

```bash
docker exec -it mysql-db mysql -u root -p
```

Enter the password:

```
admin123
```

---

## Step 3: Create a Database

```sql
CREATE DATABASE sd;
USE sd;
```

---

## Step 4: Create a Table

```sql
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);
```

---

## Step 5: Insert Data

```sql
INSERT INTO students (name)
VALUES ('Suraj'),
       ('Rahul'),
       ('Amit');
```

---

## Step 6: Verify

```sql
SELECT * FROM students;
```

Example Output

```
+----+--------+
| id | name   |
+----+--------+
| 1  | Suraj  |
| 2  | Rahul  |
| 3  | Amit   |
+----+--------+
```

---

## Step 7: Stop and Remove Container

```bash
docker stop mysql-db
docker rm mysql-db
```

---

## Step 8: Run a New Container

```bash
docker run -d \
--name mysql-db-new \
-e MYSQL_ROOT_PASSWORD=admin123 \
-p 3306:3306 \
mysql:latest
```

Connect again:

```bash
docker exec -it mysql-db-new mysql -u root -p
```

Check databases:

```sql
SHOW DATABASES;
```

The previously created database (`sd`) is no longer available.

---

## Observation

The database and table disappeared after removing the container.

### Why?

Containers are **ephemeral**. Since no Docker volume was attached, MySQL stored all data inside the container's writable layer. Removing the container deleted its filesystem, including the database.

---

# Task 2 – Named Volumes

## Objective

Store database files in a Docker volume so that data survives container removal.

---

## Step 1: Create a Named Volume

```bash
docker volume create mysql-data
```

Verify:

```bash
docker volume ls
```

---

## Step 2: Run MySQL with Volume

```bash
docker run -d \
--name mysql-db \
-e MYSQL_ROOT_PASSWORD=admin123 \
-v mysql-data:/var/lib/mysql \
-p 3306:3306 \
mysql:latest
```

---

## Step 3: Create Data

```bash
docker exec -it mysql-db mysql -u root -p
```

```sql
CREATE DATABASE sd;
USE sd;

CREATE TABLE students(
id INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(100)
);

INSERT INTO students(name)
VALUES('Suraj'),
      ('Rahul'),
      ('Amit');

SELECT * FROM students;
```

---

## Step 4: Remove Container

```bash
docker stop mysql-db
docker rm mysql-db
```

---

## Step 5: Create a New Container Using the Same Volume

```bash
docker run -d \
--name mysql-db-new \
-e MYSQL_ROOT_PASSWORD=admin123 \
-v mysql-data:/var/lib/mysql \
-p 3306:3306 \
mysql:latest
```

Connect again:

```bash
docker exec -it mysql-db-new mysql -u root -p
```

```sql
SHOW DATABASES;
USE sd;
SELECT * FROM students;
```

The data is still available.

---

## Verify Volume

```bash
docker volume ls
```

Inspect:

```bash
docker volume inspect mysql-data
```

---

## Observation

Even after deleting the container, the database and table remained intact.

### Why?

Docker volumes store data outside the container filesystem. Containers can be deleted and recreated without affecting the stored data.

---

# Task 3 – Bind Mounts

## Objective

Mount a host directory inside a container.

---

## Step 1: Create a Folder

```bash
mkdir nginx-site
cd nginx-site
```

---

## Step 2: Create index.html

```bash
nano index.html
```

```html
<!DOCTYPE html>
<html>
<head>
    <title>Docker Bind Mount</title>
</head>
<body>
    <h1>Hello from Docker Bind Mount!</h1>
</body>
</html>
```

---

## Step 3: Run Nginx

```bash
docker run -d \
--name nginx-bind \
-p 8081:80 \
-v $(pwd):/usr/share/nginx/html \
nginx
```

---

## Step 4: Access Browser

```
http://<EC2-Public-IP>:8081
```

---

## Step 5: Edit index.html

```html
<h1>Bind Mount is Working!</h1>
```

Refresh the browser.

The changes appear immediately without restarting the container.

---

## Named Volume vs Bind Mount

| Named Volume | Bind Mount |
|--------------|------------|
| Managed by Docker | Managed by Host OS |
| Stores data in Docker's storage | Uses a specific host directory |
| Best for databases | Best for development |
| Portable | Depends on host path |
| Easy backup | Direct file access |

---

# Task 4 – Docker Networking Basics

## Objective

Understand Docker's default bridge network.

---

## List Networks

```bash
docker network ls
```

---

## Inspect Bridge Network

```bash
docker network inspect bridge
```

---

## Run Two Containers

```bash
docker run -dit --name container1 ubuntu bash
```

```bash
docker run -dit --name container2 ubuntu bash
```

---

## Install Ping

```bash
docker exec -it container1 bash
```

```bash
apt update
apt install -y iputils-ping
```

Repeat for `container2`.

---

## Ping by Name

```bash
ping container2
```

Result:

```
ping: container2: Name or service not known
```

---

## Ping by IP

Find IP:

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container2
```

Example:

```
172.17.0.3
```

Ping:

```bash
ping 172.17.0.3
```

Result:

```
64 bytes from 172.17.0.3
```

---

## Observation

| Method | Result |
|----------|---------|
| Ping by Name | ❌ Failed |
| Ping by IP | ✅ Success |

---

# Task 5 – Custom Networks

## Objective

Enable DNS-based communication between containers.

---

## Create Network

```bash
docker network create my-app-net
```

---

## Run Containers

```bash
docker run -dit \
--name app1 \
--network my-app-net \
ubuntu bash
```

```bash
docker run -dit \
--name app2 \
--network my-app-net \
ubuntu bash
```

---

## Install Ping

```bash
docker exec -it app1 bash
```

```bash
apt update
apt install -y iputils-ping
```

---

## Ping by Name

```bash
ping app2
```

Result:

```
64 bytes from app2
```

---

## Observation

Containers communicate successfully using their names.

### Why?

User-defined bridge networks include Docker's built-in DNS server, allowing containers to resolve each other's names automatically.

---

# Task 6 – Put It Together

## Objective

Combine Docker Volumes and Docker Networking.

---

## Create Network

```bash
docker network create app-network
```

---

## Create Volume

```bash
docker volume create postgres-data
```

---

## Run PostgreSQL

```bash
docker run -d \
--name postgres-db \
--network app-network \
-v postgres-data:/var/lib/postgresql/data \
-e POSTGRES_PASSWORD=admin123 \
postgres
```

---

## Run Application Container

```bash
docker run -dit \
--name app-container \
--network app-network \
ubuntu bash
```

---

## Install Tools

```bash
docker exec -it app-container bash
```

```bash
apt update
apt install -y iputils-ping postgresql-client
```

---

## Verify Communication

```bash
ping postgres-db
```

Output:

```
64 bytes from postgres-db
```

---

## Connect to PostgreSQL

```bash
psql -h postgres-db -U postgres
```

Enter password:

```
admin123
```

Successful connection confirms container-to-container communication.

---

## Verify Network

```bash
docker network inspect app-network
```

Both containers appear in the network.

---

# Key Learnings

- Containers are temporary and lose data when removed.
- Docker Volumes provide persistent storage.
- Named Volumes are recommended for databases.
- Bind Mounts are useful during development because host file changes appear instantly inside containers.
- The default bridge network allows communication by IP but not by container name.
- User-defined bridge networks provide automatic DNS resolution.
- Multiple containers can communicate securely using custom Docker networks.
- Combining Docker Volumes and Docker Networking enables production-ready multi-container applications.

---

# Conclusion

In this lab, I learned how Docker handles persistent storage using **Volumes** and how containers communicate using **Docker Networks**. I observed that container data is lost without volumes, while named volumes preserve data across container recreation. I also explored the difference between bind mounts and named volumes, understood the limitations of the default bridge network, and learned how custom bridge networks provide automatic container name resolution. Finally, I combined networking and volumes to build a simple multi-container environment where an application container successfully communicated with a PostgreSQL database container while maintaining persistent data storage.

---

# Screenshots

> Add screenshots for:

- Running MySQL/PostgreSQL container
- Creating database and inserting records
- Data loss after removing the container
- `docker volume ls`
- `docker volume inspect`
- Bind mount webpage before and after editing
- `docker network ls`
- `docker network inspect bridge`
- Ping by IP (default bridge)
- Ping by name (custom bridge)
- `docker network inspect app-network`
- PostgreSQL connection from app container
