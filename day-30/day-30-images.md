# Day 30 – Docker Images & Container Lifecycle

## Objective

Today's goal was to understand how Docker images and containers work, explore image layers, and practice the complete lifecycle of a Docker container.

---

# Task 1: Docker Images

## Pull Docker Images

Pulled the following images from Docker Hub:

```bash
docker pull nginx
docker pull ubuntu
docker pull alpine
```

### Verify Downloaded Images

```bash
docker images
```

### Sample Output

```text
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    xxxxxxxxxxxx   2 weeks ago   192MB
ubuntu       latest    xxxxxxxxxxxx   3 weeks ago    80MB
alpine       latest    xxxxxxxxxxxx   5 days ago      8MB
```

> 📸 Screenshot: `docker images`

---

## Ubuntu vs Alpine

| Ubuntu | Alpine |
|---------|---------|
| Full Linux distribution | Minimal Linux distribution |
| Includes many packages and utilities | Contains only essential packages |
| Uses GNU libc | Uses musl libc and BusyBox |
| Larger image (~80 MB) | Very small image (~8 MB) |
| Good for development | Best for lightweight containers |

### Why is Alpine Smaller?

- Alpine contains only the essential components required to run applications.
- Uses **musl libc** instead of GNU libc.
- Uses **BusyBox**, which combines many Linux utilities into a single lightweight executable.
- Smaller images download faster, start faster, consume less storage, and reduce network bandwidth.

---

## Inspect an Image

Command:

```bash
docker image inspect ubuntu
```

Information available from inspection:

- Image ID
- Repository Tags
- Creation Date
- Architecture
- Operating System
- Environment Variables
- Docker Version
- Working Directory
- Entrypoint
- Default Command
- Root Filesystem Layers

---

## Remove an Image

```bash
docker image rm alpine
```

Verify:

```bash
docker images
```

---

# Task 2: Image Layers

View image history:

```bash
docker image history nginx
```

### Sample Output

```text
IMAGE          CREATED        CREATED BY                SIZE
xxxxxxx        2 weeks ago    CMD ["nginx"...]          0B
xxxxxxx        2 weeks ago    EXPOSE 80                0B
xxxxxxx        2 weeks ago    COPY docker-entrypoint   4KB
xxxxxxx        2 weeks ago    RUN apt-get install      90MB
xxxxxxx        2 weeks ago    Base Layer               70MB
```

> 📸 Screenshot: `docker image history nginx`

---

## What are Docker Layers?

Docker images are built from multiple **read-only layers**.

Each instruction inside a Dockerfile (such as `RUN`, `COPY`, or `ADD`) creates a new layer.

When Docker builds an image again, unchanged layers are reused instead of rebuilding everything.

### Advantages of Layers

- Faster image builds
- Efficient storage
- Layer caching
- Faster downloads
- Images can share common layers

---

# Task 3: Container Lifecycle

## Create a Container (without starting)

```bash
docker create --name my-nginx nginx
```

Status:

```bash
docker ps -a
```

Container State:

```
Created
```

---

## Start Container

```bash
docker start my-nginx
```

State:

```
Up
```

---

## Pause Container

```bash
docker pause my-nginx
```

Verify:

```bash
docker ps
```

State:

```
Up (Paused)
```

---

## Unpause Container

```bash
docker unpause my-nginx
```

State:

```
Up
```

---

## Stop Container

```bash
docker stop my-nginx
```

State:

```
Exited
```

---

## Restart Container

```bash
docker restart my-nginx
```

State:

```
Up
```

---

## Kill Container

```bash
docker kill my-nginx
```

State:

```
Exited
```

---

## Remove Container

```bash
docker rm my-nginx
```

Verify:

```bash
docker ps -a
```

Container removed successfully.

---

## Container Lifecycle Summary

```
Create
   │
   ▼
Created
   │
Start
   ▼
Running
   │
Pause
   ▼
Paused
   │
Unpause
   ▼
Running
   │
Stop
   ▼
Exited
   │
Restart
   ▼
Running
   │
Kill
   ▼
Exited
   │
Remove
   ▼
Deleted
```

---

# Task 4: Working with Running Containers

## Run Nginx Container in Detached Mode

```bash
docker run -d --name sd-nginx nginx
```

---

## View Logs

```bash
docker logs sd-nginx
```

Displays startup logs of the Nginx server.

---

## View Real-Time Logs

```bash
docker logs -f sd-nginx
```

Continuously displays new log entries until interrupted.

---

## Enter Running Container

```bash
docker exec -it sd-nginx bash
```

Useful commands inside the container:

```bash
pwd

ls

cd /

ls -la

cat /etc/os-release
```

Exit:

```bash
exit
```

---

## Execute a Single Command

Without entering the container:

```bash
docker exec sd-nginx ls /
```

Other examples:

```bash
docker exec sd-nginx whoami

docker exec sd-nginx pwd

docker exec sd-nginx hostname
```

---

## Inspect Running Container

```bash
docker inspect sd-nginx
```

Important information available:

- Container ID
- Image
- Current State
- IP Address
- Port Mappings
- Mounted Volumes
- Environment Variables
- Network Configuration

### Get Container IP

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' sd-nginx
```

Example:

```
172.17.0.2
```

### View Port Mapping

```bash
docker port sd-nginx
```

If started without `-p`, no host port is mapped.

### View Mounts

```bash
docker inspect -f '{{json .Mounts}}' sd-nginx
```

If no volumes are attached:

```
[]
```

---

# Task 5: Cleanup

## Stop All Running Containers

```bash
docker stop $(docker ps -q)
```

---

## Remove All Stopped Containers

```bash
docker container prune -f
```

---

## Remove Unused Images

```bash
docker image prune -a -f
```

---

## Check Docker Disk Usage

```bash
docker system df
```

Detailed view:

```bash
docker system df -v
```

---

# Docker Image vs Container

| Docker Image | Docker Container |
|---------------|------------------|
| Blueprint | Running instance |
| Read-only | Read-write |
| Cannot execute | Executes applications |
| Created once | Created from an image |
| Stored on disk | Runs in memory |

---

# Key Docker Commands Learned

## Image Commands

```bash
docker pull

docker images

docker image inspect

docker image history

docker rmi
```

---

## Container Commands

```bash
docker create

docker start

docker pause

docker unpause

docker stop

docker restart

docker kill

docker rm

docker ps

docker ps -a
```

---

## Running Container Commands

```bash
docker logs

docker logs -f

docker exec

docker inspect

docker port
```

---

## Cleanup Commands

```bash
docker container prune

docker image prune

docker system prune

docker system df
```

---

# Key Takeaways

- Docker images are read-only templates used to create containers.
- Containers are isolated runtime instances of images.
- Docker images consist of multiple reusable layers.
- Layer caching significantly speeds up image builds.
- Containers move through different lifecycle states such as Created, Running, Paused, Exited, and Removed.
- `docker exec` allows commands to run inside a running container.
- `docker inspect` provides detailed metadata about images and containers.
- Docker cleanup commands help reclaim unused disk space and keep the environment organized.

---

# Conclusion

Today I explored Docker images, understood how image layers improve efficiency, practiced the complete lifecycle of containers, worked with running containers using logs and exec commands, inspected container metadata, and learned how to clean up Docker resources effectively.
