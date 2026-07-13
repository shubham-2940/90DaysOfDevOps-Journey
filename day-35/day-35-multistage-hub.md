# Day 35 – Multi-Stage Builds & Docker Hub

## Objective

The goal of this task was to learn how to create optimized Docker images using **Multi-Stage Builds**, publish them to **Docker Hub**, and apply Docker image best practices to improve image size, security, and maintainability.

---

# Task 1: The Problem with Large Images

## Simple Node.js Application

### app.js

```javascript
console.log("Hello from Docker!");
```

### package.json

```json
{
  "name": "day35-app",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  }
}
```

### Single-Stage Dockerfile

```dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./

COPY . .

CMD ["node", "app.js"]
```

### Build Image

```bash
docker build -t day-35-singlestage .
```

### Run Container

```bash
docker run --rm day-35-singlestage
```

### Output

```
Hello from Docker!
```

### Image Size

```bash
docker images
```

| Image | Size |
|--------|------|
| day-35-singlestage | **1.62 GB** |

---

# Observation

The image size is very large because the full **node:22** image contains many packages and tools that are not required to run a simple application.

---

# Task 2: Multi-Stage Build

## Multi-Stage Dockerfile

```dockerfile
# Stage 1 - Builder
FROM node:22 AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

# Stage 2 - Runtime
FROM node:22-alpine

WORKDIR /app

COPY --from=builder /app .

CMD ["node", "app.js"]
```

### Build Image

```bash
docker build -t day35-multistage .
```

### Run Container

```bash
docker run --rm day35-multistage
```

### Output

```
Hello from Docker!
```

### Image Size

| Image | Size |
|--------|------|
| day35-multistage | **230 MB** |

---

# Image Size Comparison

| Build Type | Size |
|------------|------|
| Single Stage | **1.62 GB** |
| Multi Stage | **230 MB** |

---

# Why is the Multi-Stage Image Smaller?

- Uses a lightweight **node:22-alpine** base image.
- Build process happens in a separate stage.
- Only the required application files are copied to the final image.
- Unnecessary build dependencies are excluded.
- Results in:
  - Faster image downloads
  - Faster deployments
  - Lower storage usage
  - Smaller attack surface

---

# Task 3: Push to Docker Hub

## Login

```bash
docker login
```

## Tag Image

```bash
docker tag day35-multistage <your-dockerhub-username>/day35-multistage:v1
```

Example

```bash
docker tag day35-multistage johndoe/day35-multistage:v1
```

## Push Image

```bash
docker push <your-dockerhub-username>/day35-multistage:v1
```

## Remove Local Image

```bash
docker rmi <your-dockerhub-username>/day35-multistage:v1
docker rmi day35-multistage
```

## Pull Again

```bash
docker pull <your-dockerhub-username>/day35-multistage:v1
```

## Verify

```bash
docker run --rm <your-dockerhub-username>/day35-multistage:v1
```

Output

```
Hello from Docker!
```

---

# Task 4: Docker Hub Repository

## Repository Verification

Verified that the image was successfully uploaded to Docker Hub.

## Repository Description

```
A simple Node.js application demonstrating Docker Multi-Stage Builds, Docker Hub image publishing, and Docker image optimization as part of the 90 Days of DevOps challenge.
```

---

## Understanding Image Tags

Available tags:

- latest
- v1

### Pull Specific Version

```bash
docker pull <your-dockerhub-username>/day35-multistage:v1
```

### Pull Latest Version

```bash
docker pull <your-dockerhub-username>/day35-multistage:latest
```

or

```bash
docker pull <your-dockerhub-username>/day35-multistage
```

### Difference

| Specific Tag | latest |
|--------------|--------|
| Downloads exactly that version | Downloads the image currently tagged as latest |
| Ideal for production deployments | Good for development/testing |

---

# Task 5: Docker Image Best Practices

## Optimized Dockerfile

```dockerfile
# Stage 1 - Build
FROM node:22-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install --omit=dev

COPY . .

# Stage 2 - Runtime
FROM node:22-alpine

WORKDIR /app

RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup

COPY --from=builder /app .

USER appuser

CMD ["node", "app.js"]
```

---

## Best Practices Applied

### 1. Minimal Base Image

Used:

```dockerfile
FROM node:22-alpine
```

instead of

```dockerfile
FROM node:22
```

Result:

Smaller image with fewer unnecessary packages.

---

### 2. Non-Root User

Created a dedicated user:

```dockerfile
RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup

USER appuser
```

Benefit:

Improves container security by avoiding running applications as the root user.

---

### 3. Combined RUN Commands

Instead of multiple RUN instructions:

```dockerfile
RUN addgroup ...
RUN adduser ...
```

Combined into one:

```dockerfile
RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup
```

Benefit:

Reduces image layers.

---

### 4. Specific Image Tag

Used:

```dockerfile
node:22-alpine
```

instead of

```dockerfile
node:latest
```

Benefit:

Ensures consistent and reproducible builds.

---

# Final Image Size Comparison

| Image | Size |
|--------|------|
| Single Stage | **1.62 GB** |
| Multi Stage | **230 MB** |
| Best Practice | **230 MB** |

---

# Overall Observation

- The single-stage build produced the largest image because it included the full Node.js runtime and all build components.
- The multi-stage build significantly reduced the image size by separating the build environment from the runtime environment.
- The best-practice image maintained the optimized size while improving security by using a non-root user and a minimal base image.
- For this simple application, the best-practice image size remained similar to the multi-stage image because the additional optimizations mainly improve security and maintainability rather than reducing image size.

---

# Commands Used

```bash
docker build -t day-35-singlestage .

docker build -t day35-multistage .

docker build -t day35-best-practice:v1 .

docker images

docker run --rm day35-best-practice:v1

docker login

docker tag day35-multistage <username>/day35-multistage:v1

docker push <username>/day35-multistage:v1

docker pull <username>/day35-multistage:v1
```

---

# Key Learnings

- Understood the difference between single-stage and multi-stage Docker builds.
- Learned how multi-stage builds create smaller and more efficient images.
- Published Docker images to Docker Hub.
- Learned Docker image versioning using tags.
- Understood the difference between `latest` and version-specific tags.
- Applied Docker image best practices:
  - Minimal base image
  - Multi-stage build
  - Non-root user
  - Combined RUN commands
  - Specific base image tags
- Compared image sizes before and after optimization.
- Learned how image optimization improves deployment speed, storage efficiency, and container security.

---

# Conclusion

In this task, I successfully built a simple Node.js application using both single-stage and multi-stage Dockerfiles. I optimized the image by using a lightweight Alpine base image, publishing it to Docker Hub, and following Docker image best practices. The optimized image size was reduced from **1.62 GB** to **230 MB** (approximately **86% smaller**), demonstrating the importance of multi-stage builds in creating efficient, secure, and production-ready Docker images.
