# Day 31 – Dockerfile: Build Your Own Docker Images

## Task

Today's goal is to learn how to create custom Docker images using a **Dockerfile**.

You will:
- Create your first Dockerfile
- Learn common Dockerfile instructions
- Understand the difference between `CMD` and `ENTRYPOINT`
- Build a simple static website using Nginx
- Use `.dockerignore` to optimize builds
- Learn how Docker layer caching improves build performance

---

# What is a Dockerfile?

A **Dockerfile** is a text file that contains a set of instructions used by Docker to automatically build a Docker image.

Instead of manually creating containers and installing software every time, you can define everything in a Dockerfile and build identical images whenever needed.

---

# Why Dockerfile?

- Automates image creation
- Ensures consistent environments
- Easy to share and version control
- Makes deployments reproducible
- Saves time by using Docker cache

---

# Common Dockerfile Instructions

| Instruction | Description |
|------------|-------------|
| FROM | Specifies the base image |
| RUN | Executes commands during image build |
| COPY | Copies files from host to image |
| WORKDIR | Sets the working directory |
| EXPOSE | Documents the port used by the container |
| CMD | Specifies the default command |
| ENTRYPOINT | Specifies the main executable of the container |

---

# Task 1 – My First Dockerfile

### Dockerfile

```dockerfile
FROM ubuntu:latest

RUN apt update && apt install -y curl

CMD ["echo", "Hello from my custom image!"]
```

### Build Image

```bash
docker build -t my-ubuntu:v1 .
```

### Run Container

```bash
docker run my-ubuntu:v1
```

### Output

```
Hello from my custom image!
```

### Learning

- Ubuntu is used as the base image.
- curl is installed during image build.
- CMD runs when the container starts.

---

# Task 2 – Dockerfile Instructions

## Dockerfile

```dockerfile
FROM nginx:latest

RUN echo "Docker image is being built"

WORKDIR /usr/share/nginx/html

COPY index.html .

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Build

```bash
docker build -t dockerfile-task2:v1 .
```

### Run

```bash
docker run -d -p 8080:80 --name docker-task2 dockerfile-task2:v1
```

### Verify

```bash
curl http://localhost:8080
```

### Learning

- RUN executes commands during build.
- WORKDIR changes the current working directory.
- COPY copies files into the image.
- EXPOSE documents the container port.
- CMD starts Nginx in the foreground.

---

# Task 3 – CMD vs ENTRYPOINT

## CMD Example

Dockerfile

```dockerfile
FROM ubuntu:latest

CMD ["echo", "hello"]
```

Build

```bash
docker build -t cmd-demo:v1 .
```

Run

```bash
docker run cmd-demo:v1
```

Output

```
hello
```

Run with custom command

```bash
docker run cmd-demo:v1 date
```

Output

```
Current Date and Time
```

### Observation

The custom command replaces CMD.

---

## ENTRYPOINT Example

Dockerfile

```dockerfile
FROM ubuntu:latest

ENTRYPOINT ["echo"]
```

Build

```bash
docker build -t entrypoint-demo:v1 .
```

Run

```bash
docker run entrypoint-demo:v1 Hello Docker!
```

Output

```
Hello Docker!
```

### Observation

Additional arguments are appended to ENTRYPOINT.

---

## CMD vs ENTRYPOINT

| CMD | ENTRYPOINT |
|------|------------|
| Default command | Fixed executable |
| Easily overridden | Runs every time |
| Best for optional commands | Best for dedicated applications |

---

# Task 4 – Build a Static Website

## index.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Website</title>
</head>
<body>
    <h1>Welcome to My Docker Website!</h1>
    <p>This website is running inside an Nginx container.</p>
</body>
</html>
```

## Dockerfile

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Build

```bash
docker build -t my-website:v1 .
```

### Run

```bash
docker run -d -p 8080:80 --name my-website-container my-website:v1
```

### Verify

```bash
curl http://localhost:8080
```

---

# Task 5 – .dockerignore

## .dockerignore

```text
node_modules
.git
*.md
.env
```

### Build

```bash
docker build -t my-website:v2 .
```

### Why use .dockerignore?

- Reduces build context size
- Improves build speed
- Prevents sensitive files from entering the image
- Keeps images clean

---

# Task 6 – Docker Build Optimization

## Dockerfile

```dockerfile
FROM nginx:alpine

RUN echo "Installing application..."

COPY index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Build

```bash
docker build -t cache-demo:v1 .
```

Modify `index.html` and rebuild.

```bash
docker build -t cache-demo:v2 .
```

### Observation

Docker reused cached layers and rebuilt only the changed layer and the layers after it.

---

# Why Docker Layer Order Matters

Docker builds images layer by layer.

Every Dockerfile instruction creates a new layer.

If one layer changes:
- Docker rebuilds that layer.
- All layers after it are rebuilt.
- Layers before it are reused from cache.

### Best Practice

Keep rarely changing instructions at the top.

Example:

```dockerfile
FROM nginx:alpine

RUN apt update

COPY index.html /usr/share/nginx/html/

CMD ["nginx", "-g", "daemon off;"]
```

Since `COPY` changes frequently, placing it near the bottom allows Docker to reuse previous layers.

---

# Important Commands

## Build Image

```bash
docker build -t image-name .
```

## Run Container

```bash
docker run image-name
```

## Run in Detached Mode

```bash
docker run -d image-name
```

## Port Mapping

```bash
docker run -d -p 8080:80 image-name
```

## View Running Containers

```bash
docker ps
```

## View All Containers

```bash
docker ps -a
```

## Stop Container

```bash
docker stop container-name
```

## Remove Container

```bash
docker rm container-name
```

## View Logs

```bash
docker logs container-name
```

---

# Key Takeaways

- Learned how to create custom Docker images.
- Understood the purpose of Dockerfile instructions.
- Learned the difference between CMD and ENTRYPOINT.
- Built and deployed a static website using Nginx.
- Used `.dockerignore` to optimize Docker builds.
- Learned how Docker layer caching speeds up rebuilds.
- Understood why Dockerfile instruction order affects build performance.

---

# Conclusion

Day 31 focused on creating reproducible Docker images using Dockerfiles. I learned how Docker executes instructions layer by layer, how caching improves build speed, and how to build and deploy a static web application using Nginx. Understanding Dockerfiles is a key skill for building efficient, portable, and production-ready containerized applications.
