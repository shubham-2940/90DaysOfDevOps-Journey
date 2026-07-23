# Day 36 – Docker Project: Dockerize a Full Application

## Objective

The objective of this project was to Dockerize a complete full-stack application using Docker and Docker Compose. The application consists of a React frontend, Go backend, and PostgreSQL database. The goal was to containerize each component, connect them using Docker networking, and manage the entire application through Docker Compose.

---

# Project Chosen

## DevBoard MVP

**GitHub Repository:**
https://github.com/trainwithshubham/devboard

### Why I chose this project

I selected DevBoard MVP because it represents a real-world full-stack application. It includes multiple services that communicate with each other, making it an excellent project to understand production-like Docker workflows.

This project helped me learn:

- Dockerizing frontend and backend separately
- Multi-stage Docker builds
- Working with PostgreSQL containers
- Docker networking
- Docker volumes
- Environment variables
- Docker Compose
- Container communication
- Running an entire application using a single command

---

# Tech Stack

Frontend
- React
- Vite

Backend
- Golang
- Gin Framework

Database
- PostgreSQL 16

Containerization
- Docker
- Docker Compose

---

# Project Structure

```
devboard/
│
├── backend/
│   ├── Dockerfile
│   ├── main.go
│   ├── go.mod
│   └── go.sum
│
├── frontend/
│   ├── Dockerfile
│   ├── package.json
│   ├── vite.config.js
│   └── src/
│
├── init/
│   └── postgres/
│       ├── 01_schema.sql
│       └── 02_seed.sql
│
├── docker-compose.yml
├── .env
└── README.md
```

---

# Dockerfile (Frontend)

```dockerfile
# Stage 1 - Build React application
FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

# Stage 2 - Production Image
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install --omit=dev

COPY --from=build /app/dist ./dist

COPY vite.preview.config.js ./vite.config.js

EXPOSE 4173

CMD ["vite","preview","--host","0.0.0.0","--port","4173"]
```

### Explanation

### FROM node:20-alpine AS build

Uses a lightweight Alpine Linux image to build the React application.

### WORKDIR /app

Sets the working directory inside the container.

### COPY package*.json ./

Copies dependency files.

### RUN npm ci

Installs project dependencies.

### COPY . .

Copies application source code.

### RUN npm run build

Creates the optimized production build.

### Second Stage

A new clean Node image is used.

Only production files are copied.

This reduces image size significantly.

---

# Dockerfile (Backend)

```dockerfile
FROM golang:1.22-alpine AS build

WORKDIR /app

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN CGO_ENABLED=0 go build -o devboard-backend .

FROM alpine:latest

RUN adduser -D -u 10001 app

WORKDIR /app

COPY --from=build /app/devboard-backend .

USER app

EXPOSE 8080

CMD ["./devboard-backend"]
```

### Explanation

Uses a multi-stage build.

Downloads Go dependencies.

Builds a static binary.

Copies only the binary into the final image.

Runs the application as a non-root user for better security.

---

# .dockerignore

```
node_modules
.git
.gitignore
README.md
Dockerfile
dist
.env
```

## Why use .dockerignore?

- Smaller build context
- Faster Docker builds
- Keeps unnecessary files out of the image

---

# Docker Compose

The project uses Docker Compose to manage all services together.

Services include:

- Frontend
- Backend
- PostgreSQL

Docker Compose provides:

- Automatic networking
- Environment variable management
- Persistent database storage
- Service dependency management

---

# Docker Compose Features

## Frontend

- Built from frontend Dockerfile
- Exposes port 8080
- Connected to custom network

---

## Backend

- Built from backend Dockerfile
- Exposes port 8081
- Uses environment variables
- Connects with PostgreSQL

---

## PostgreSQL

- Uses postgres:16-alpine
- Database persistence through Docker volumes
- Automatically executes initialization SQL scripts
- Healthcheck enabled

---

# Environment Variables

Example .env

```
POSTGRES_USER=devboard
POSTGRES_PASSWORD=devboard
POSTGRES_DB=devboard

DATABASE_URL=postgres://devboard:devboard@postgres:5432/devboard?sslmode=disable
```

Using environment variables keeps sensitive configuration outside the application code.

---

# Docker Network

A custom bridge network is created.

```
devboard-net
```

Benefits

- Container-to-container communication
- DNS resolution by container name
- Better isolation

---

# Docker Volume

A named volume is used for PostgreSQL.

Benefits

- Persistent database storage
- Data survives container recreation

---

# Healthcheck

Healthchecks ensure PostgreSQL is ready before the backend starts.

Example

```yaml
healthcheck:
  test: ["CMD-SHELL","pg_isready -U devboard"]
  interval: 10s
  timeout: 5s
  retries: 5
```

---

# Running the Project

Clone repository

```
git clone https://github.com/trainwithshubham/devboard.git
```

Navigate

```
cd devboard
```

Start application

```
docker compose up --build
```

Run in detached mode

```
docker compose up -d
```

Stop application

```
docker compose down
```

---

# Challenges Faced

## 1. Git checkout failed

Issue

```
pathspec 'advanced' did not match
```

Solution

I was running the command outside the repository. After navigating into the cloned project, the command worked correctly.

---

## 2. Frontend build failed

Issue

```
npm ci returned code 137
```

Reason

EC2 instance ran out of memory.

Solution

Created a swap file to provide additional virtual memory, allowing the build to complete successfully.

---

## 3. PostgreSQL tables missing

Issue

```
relation "projects" does not exist
relation "tasks" does not exist
```

Reason

Database initialization scripts were not executed because the database had already been initialized before mounting the SQL files.

Solution

Removed the PostgreSQL container and recreated it with the correct initialization script mount so that the schema and seed SQL files were executed.

---

## 4. Docker Network Error

Issue

```
network devboard-net not found
```

Solution

Created the custom Docker network before starting the PostgreSQL container.

---

## 5. No Space Left on Device

Issue

```
no space left on device
```

Reason

The EC2 root volume became full due to Docker images, build cache, and containers.

Solution

Cleaned unused Docker resources using Docker prune commands and freed up disk space.

---

# Security Improvements

Implemented the following best practices:

- Multi-stage builds
- Lightweight Alpine images
- Non-root user
- Docker Compose
- Environment variables
- Persistent volumes
- Custom network
- Healthchecks

---

# Final Image Sizes

| Image | Approximate Size |
|--------|-----------------:|
| Frontend | ~170 MB |
| Backend | ~20 MB |
| PostgreSQL | Official Image |

(Replace with your actual sizes using `docker images`.)

---

# Docker Hub Images

Frontend

```
docker pull trainwithshubham/devboard-frontend
```

Backend

```
docker pull trainwithshubham/devboard-backend
```

Replace these with your own Docker Hub repository names if you pushed them under your account.

---

# Testing the Complete Flow

The application was tested from scratch using the following process:

1. Removed all containers
2. Removed all images
3. Pulled images from Docker Hub
4. Started services using Docker Compose
5. Verified frontend, backend, and PostgreSQL communication
6. Confirmed the application worked successfully

---

# Learning Outcomes

Through this project, I learned:

- Multi-stage Docker builds
- Building production-ready Docker images
- Docker networking
- Docker volumes
- Docker Compose
- PostgreSQL containerization
- Running containers as non-root users
- Environment variable management
- Healthchecks
- Debugging Docker build failures
- Debugging container networking
- Database initialization using Docker

---

# Conclusion

This project provided hands-on experience with containerizing a real-world full-stack application. I successfully Dockerized the frontend, backend, and PostgreSQL database, managed them using Docker Compose, implemented production best practices such as multi-stage builds and non-root users, and resolved common issues related to memory, storage, networking, and database initialization. This exercise closely resembles real-world Docker workflows used in modern DevOps environments.
