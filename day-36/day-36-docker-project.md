# Day 36 – Docker Project: Dockerize a Full Application

## Objective

The objective of this task was to Dockerize a complete full-stack application by creating optimized Docker images, orchestrating multiple services using Docker Compose, and deploying the application using containers. This project demonstrates how Docker is used in real-world environments to package applications consistently across different systems.

---

# Application Chosen

**Project:** DevBoard MVP

I chose the **DevBoard MVP** project because it is a complete full-stack application consisting of:

- React + Vite Frontend
- Go (Gin) Backend API
- PostgreSQL Database

This project allowed me to practice Dockerizing multiple services and managing them using Docker Compose.

---

# Project Architecture

```
                +--------------------+
                |    React Frontend  |
                |      Port 4173     |
                +----------+---------+
                           |
                           |
                    HTTP Requests
                           |
                +----------v---------+
                |    Go Backend API  |
                |      Port 8080     |
                +----------+---------+
                           |
                     PostgreSQL Driver
                           |
                +----------v---------+
                |     PostgreSQL     |
                |      Port 5432     |
                +--------------------+
```

---

# Technologies Used

- Docker
- Docker Compose
- React + Vite
- Go (Gin Framework)
- PostgreSQL
- BuildKit
- Alpine Linux Images

---

# Task 1 – Dockerizing the Application

The repository was cloned from GitHub.

```bash
git clone https://github.com/shubham-2940/devboard.git
```

The project contains three main components:

- frontend
- backend
- PostgreSQL

---

# Task 2 – Dockerfile

## Frontend Dockerfile

### Build Stage

- Uses **Node 20 Alpine**
- Installs dependencies
- Builds React application

### Runtime Stage

- Uses lightweight Node Alpine image
- Creates non-root user
- Copies build artifacts
- Exposes port **4173**

Benefits:

- Smaller image
- Better security
- Faster deployments

---

## Backend Dockerfile

### Build Stage

- Uses Go Alpine image
- Downloads dependencies
- Compiles Go binary

### Runtime Stage

- Uses Alpine Linux
- Creates non-root user
- Copies compiled binary only
- Exposes port **8080**

Benefits:

- Very small image
- Secure runtime
- Fast startup

---

## .dockerignore

A `.dockerignore` file was added to reduce build context.

Ignored files:

```
node_modules
dist
.git
.env
Dockerfile
README.md
```

Benefits:

- Faster builds
- Smaller build context
- Better caching

---

# Task 3 – Docker Compose

Docker Compose was used to orchestrate all services.

Services included:

## Frontend

- Built from local Dockerfile
- Runs on port **4173**

## Backend

- Built from local Dockerfile
- Runs on port **8080**
- Connects to PostgreSQL

## PostgreSQL

- Image:
```
postgres:16-alpine
```

Configured with:

- Persistent Volume
- Custom Network
- Environment Variables
- Health Check

---

## Network

Custom bridge network:

```
devboard-net
```

This allows containers to communicate using service names.

---

## Persistent Storage

Docker Volume was used for PostgreSQL database persistence.

Benefits:

- Database survives container restart
- Persistent application data

---

## Environment Variables

Environment variables were stored using a `.env` file.

Example:

```env
POSTGRES_USER=devboard
POSTGRES_PASSWORD=devboard
POSTGRES_DB=devboard
DATABASE_URL=postgres://devboard:devboard@postgres:5432/devboard?sslmode=disable
```

---

## Health Check

A health check was configured for PostgreSQL.

Example:

```yaml
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U devboard"]
  interval: 5s
  timeout: 5s
  retries: 5
```

This ensures the backend starts only after the database becomes healthy.

---

# Task 4 – Build Images

Frontend Image

```bash
docker build -t devboard-frontend ./frontend
```

Backend Image

```bash
docker build -t devboard-backend ./backend
```

---

# Docker Compose

Application started using:

```bash
docker compose up --build
```

All services were connected successfully through Docker Compose.

---

# Task 5 – Push Images to Docker Hub

Frontend

```bash
docker tag devboard-frontend trainwithshubham/devboard-frontend:latest
docker push trainwithshubham/devboard-frontend:latest
```

Backend

```bash
docker tag devboard-backend trainwithshubham/devboard-backend:latest
docker push trainwithshubham/devboard-backend:latest
```

Docker Hub Repository:

```
https://hub.docker.com/u/trainwithshubham
```

---

# Challenges Faced

## 1. Git Checkout Failed

### Error

```
pathspec 'advanced' did not match any file(s)
```

### Solution

Entered the cloned repository before switching branches and verified available branches.

---

## 2. Docker Build Context Error

### Error

```
path "./frontend" not found
```

### Solution

Moved into the correct project directory before running the Docker build command.

---

## 3. npm ci Failed

### Error

```
Exit Code 137
```

### Reason

Low RAM on EC2 instance.

### Solution

Created swap memory to prevent the build process from being terminated.

---

## 4. PostgreSQL Network Error

### Error

```
network devboard-net not found
```

### Solution

Created a custom Docker network before starting the PostgreSQL container.

---

## 5. Database Tables Missing

### Error

```
relation "projects" does not exist
relation "tasks" does not exist
```

### Reason

Initialization SQL scripts were not executed.

### Solution

Recreated the PostgreSQL container and initialized the database with the SQL schema and seed files.

---

## 6. Disk Space Exhausted

### Error

```
no space left on device
```

### Reason

EC2 root volume became full after multiple Docker builds.

### Solution

Removed unused Docker images, containers, volumes, and build cache using Docker prune commands.

---

# Testing

The application was tested by:

- Building frontend image
- Building backend image
- Running PostgreSQL
- Starting all services using Docker Compose
- Verifying API connectivity
- Accessing frontend through browser
- Creating sample tasks

---

# Final Image Sizes

| Image | Approximate Size |
|--------|-----------------:|
| Frontend | ~170 MB |
| Backend | ~20 MB |
| PostgreSQL | Official Image |

*(Image sizes may vary depending on the latest base images and dependencies.)*

---

# Project Structure

```
devboard/
│
├── frontend/
│   ├── Dockerfile
│   ├── .dockerignore
│   └── ...
│
├── backend/
│   ├── Dockerfile
│   └── ...
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

# Key Learnings

- Dockerizing frontend and backend applications
- Multi-stage Docker builds
- Using Alpine images for lightweight containers
- Creating secure containers using non-root users
- Writing efficient Dockerfiles
- Managing multi-container applications with Docker Compose
- Configuring custom Docker networks
- Using Docker volumes for persistent data
- Managing environment variables with `.env`
- Adding database health checks
- Troubleshooting container networking, database initialization, memory, and storage issues
- Publishing Docker images to Docker Hub

---

# Conclusion

Successfully Dockerized a full-stack application consisting of a React frontend, Go backend, and PostgreSQL database. Implemented multi-stage builds, non-root containers, Docker Compose orchestration, persistent storage, health checks, environment variables, and published images to Docker Hub. This project provided hands-on experience with deploying a production-like containerized application and resolving common real-world Docker issues.
