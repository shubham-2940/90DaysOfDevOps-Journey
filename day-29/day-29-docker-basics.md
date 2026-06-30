# Day 29 – Introduction to Docker

## Objective

Today's goal was to understand the fundamentals of Docker, install it on an Ubuntu EC2 instance, and run my first containers. I explored Docker images, containers, Docker Hub, and learned the difference between containers and virtual machines.

---

# Task 1: What is Docker?

## What is Docker?

Docker is an open-source containerization platform that allows developers to package applications along with their dependencies, libraries, and configurations into lightweight units called **containers**.

A Docker container ensures that an application behaves the same way regardless of where it is deployed (development, testing, or production).

### Why do we need Docker?

Before Docker:
- Applications worked on one machine but failed on another.
- Different operating systems and software versions caused compatibility issues.
- Setting up development environments took a long time.

Docker solves these problems by:
- Providing consistent environments
- Simplifying deployment
- Isolating applications
- Reducing setup time
- Improving scalability

---

# What is a Container?

A container is a lightweight, standalone package that contains:

- Application code
- Runtime
- Libraries
- Dependencies
- Configuration files

Unlike Virtual Machines, containers share the host operating system kernel, making them much faster and smaller.

Example:

```
Application
Dependencies
Libraries
Docker Container
```

---

# Containers vs Virtual Machines

| Containers | Virtual Machines |
|------------|------------------|
| Lightweight | Heavyweight |
| Share Host OS Kernel | Have their own Guest OS |
| Start in Seconds | Start in Minutes |
| Small Size (MBs) | Large Size (GBs) |
| Better Performance | Slower |
| Portable | Less Portable |
| High Resource Efficiency | Uses More CPU & RAM |

### Summary

Containers are ideal for modern DevOps because they are lightweight, portable, and fast, whereas Virtual Machines provide stronger isolation but consume more resources.

---

# Docker Architecture

Docker follows a Client-Server architecture.

## Components

### Docker Client

The Docker Client is the command-line interface (CLI).

Example commands:

```bash
docker run
docker ps
docker images
docker pull
```

The client sends requests to the Docker Daemon.

---

### Docker Daemon (dockerd)

The Docker Daemon:

- Builds images
- Creates containers
- Runs containers
- Manages networks
- Manages volumes

It listens for Docker API requests from the Docker Client.

---

### Docker Images

A Docker Image is a read-only template used to create containers.

Examples:

- nginx
- ubuntu
- mysql
- redis

Images are downloaded from Docker Hub.

---

### Docker Containers

A container is a running instance of an image.

Example:

```
Image
   ↓
docker run
   ↓
Container
```

---

### Docker Registry

Docker Registry stores Docker Images.

The default registry is:

**Docker Hub**

Developers can:

- Pull images
- Push images
- Share images

---

# Docker Architecture Diagram

```
               +---------------------+
               |   Docker Client     |
               | docker run          |
               | docker ps           |
               +----------+----------+
                          |
                          |
                          v
               +----------------------+
               |   Docker Daemon      |
               |  (dockerd)           |
               +----------+-----------+
                          |
          +---------------+----------------+
          |                                |
          |                                |
          v                                v
+-------------------+             +------------------+
| Docker Images     |             | Docker Containers|
+-------------------+             +------------------+
          ^
          |
          |
+-------------------+
| Docker Hub        |
| Docker Registry   |
+-------------------+
```

---

# Task 2: Install Docker

## Update Package List

```bash
sudo apt update
```

---

## Install Docker

```bash
sudo apt install docker.io -y
```

---

## Verify Installation

```bash
docker --version
```

Example Output

```
Docker version 28.x.x
```

---

## Start Docker

```bash
sudo systemctl start docker
```

---

## Enable Docker at Boot

```bash
sudo systemctl enable docker
```

---

## Verify Docker Service

```bash
sudo systemctl status docker
```

Status:

```
Active (running)
```

---

## Run Hello World

```bash
docker run hello-world
```

Observation:

- Docker searched for the image locally.
- Image was not found.
- Docker downloaded it from Docker Hub.
- Docker created a container.
- The container printed a welcome message.
- The container exited successfully.

---

# Task 3: Run Real Containers

## Pull Nginx Image

```bash
docker pull nginx
```

---

## Run Nginx Container

Foreground Mode

```bash
docker run nginx
```

This starts Nginx and attaches logs to the terminal.

---

Detached Mode

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

Explanation:

- `-d` → Run in background
- `--name my-nginx` → Custom container name
- `-p 8080:80` → Map host port 8080 to container port 80

---

## Access Nginx

Open:

```
http://<EC2-Public-IP>:8080
```

The default Nginx welcome page should appear.

---

## Run Ubuntu Container

```bash
docker run -it ubuntu bash
```

Inside the container:

```bash
ls

pwd

cat /etc/os-release

exit
```

---

## List Running Containers

```bash
docker ps
```

---

## List All Containers

```bash
docker ps -a
```

---

## Stop Container

```bash
docker stop my-nginx
```

---

## Remove Container

```bash
docker rm my-nginx
```

---

# Task 4: Explore Docker

## Run in Detached Mode

```bash
docker run -d nginx
```

Difference:

- Terminal remains free.
- Container continues running in the background.

---

## Give Container a Name

```bash
docker run -d --name my-nginx nginx
```

---

## Port Mapping

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

Host Port:

```
8080
```

Container Port:

```
80
```

---

## Check Logs

```bash
docker logs my-nginx
```

Follow logs continuously:

```bash
docker logs -f my-nginx
```

---

## Execute Commands Inside Container

```bash
docker exec -it my-nginx bash
```

Example:

```bash
ls

pwd

whoami

exit
```

---

# Useful Docker Commands

| Command | Description |
|----------|-------------|
| docker images | List images |
| docker pull nginx | Download image |
| docker run nginx | Run container |
| docker ps | Running containers |
| docker ps -a | All containers |
| docker stop container_name | Stop container |
| docker start container_name | Start container |
| docker restart container_name | Restart container |
| docker rm container_name | Remove container |
| docker logs container_name | View logs |
| docker exec -it container_name bash | Enter container |
| docker inspect container_name | Container details |
| docker system df | Docker disk usage |
| docker image prune | Remove unused images |

---

# Challenges Faced

### Issue 1

```
docker -d nginx
```

Error:

```
unknown shorthand flag: 'd'
```

Reason:

The `-d` option belongs to `docker run`, not `docker`.

Correct command:

```bash
docker run -d nginx
```

---

### Issue 2

```
failed to bind host port 80
```

Reason:

Host machine already had Nginx running on port 80.

Solution:

```bash
sudo systemctl stop nginx
```

or use another port:

```bash
docker run -d -p 8080:80 nginx
```

---

### Issue 3

```
container name already exists
```

Solution:

```bash
docker rm -f my-nginx
```

---

# Key Learnings

- Learned what Docker is and why it is used.
- Understood the difference between Containers and Virtual Machines.
- Explored Docker architecture.
- Installed Docker on an Ubuntu EC2 instance.
- Ran the `hello-world` container.
- Pulled images from Docker Hub.
- Ran Nginx and Ubuntu containers.
- Learned detached mode and interactive mode.
- Learned port mapping.
- Viewed container logs.
- Executed commands inside a running container.
- Managed Docker containers using start, stop, and remove commands.

---

# Conclusion

Docker simplifies application deployment by packaging applications and their dependencies into portable containers. Containers are lightweight, fast, and consistent across different environments, making Docker one of the most important tools in modern DevOps and cloud-native application development.

Docker serves as the foundation for container orchestration platforms like Kubernetes and is widely used in CI/CD pipelines to build, test, and deploy applications efficiently.

---

## Screenshots

- Docker installation verification
- Hello World container
- Nginx container running
- Ubuntu interactive container
- `docker ps`
- `docker ps -a`
- Nginx running in browser
