# Day 34 – Docker Compose: Real-World Multi-Container Apps

## Objective

The goal of Day 34 was to build a real-world multi-container application using Docker Compose. Instead of running containers individually, I learned how Docker Compose manages multiple services together and simplifies application deployment.

---

# Project Stack

I created a simple 3-service application consisting of:

- **Flask Web Application**
- **PostgreSQL Database**
- **Redis Cache**

Docker Compose automatically created a network so all three services could communicate with each other.

---

# Project Structure

```
day34-app-stack/
│── app.py
│── Dockerfile
│── requirements.txt
└── docker-compose.yml
```

---

# Task 1 – Build a 3-Service Application Stack

### Web Service
- Created a simple Flask application.
- Displayed a "Hello Docker Compose!" message.
- Verified connectivity with PostgreSQL and Redis.

### Database Service
- Used the official PostgreSQL image.
- Configured:
  - Database Name
  - Username
  - Password

### Cache Service
- Used the official Redis image.
- Verified connectivity from the Flask application.

### Outcome

Successfully launched all three services together using a single command.

```bash
docker compose up -d
```

---

# Task 2 – depends_on & Healthchecks

To ensure proper startup order, I configured the web application to wait until PostgreSQL became healthy.

### Added

- `depends_on`
- PostgreSQL `healthcheck`
- `condition: service_healthy`

Example:

```yaml
depends_on:
  db:
    condition: service_healthy
```

### Healthcheck

Docker repeatedly checks PostgreSQL using:

```yaml
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U admin -d mydb"]
```

### Result

When the stack starts:

1. PostgreSQL starts.
2. Docker waits until PostgreSQL is healthy.
3. Flask starts.
4. Redis starts alongside the application.

This prevents the application from trying to connect to a database that isn't ready.

---

# Task 3 – Restart Policies

Explored different restart policies.

### restart: always

```yaml
restart: always
```

Automatically attempts to restart the container whenever it stops.

### restart: on-failure

```yaml
restart: on-failure
```

Restarts the container only if it exits due to an error.

### Observation

- Verified that the restart policy was applied using:

```bash
docker inspect postgres-app-db --format='{{.HostConfig.RestartPolicy.Name}}'
```

- On my EC2 instance (Docker Engine 29.1.3), manually killing the PostgreSQL container left it in the `Exited (137)` state instead of automatically restarting. The restart policy was correctly configured, but the expected restart behavior was not observed in my environment.

### When to use each restart policy

**restart: always**

- Databases
- Redis
- Nginx
- Web Applications
- Production services

**restart: on-failure**

- Background workers
- Batch jobs
- Scripts
- Services that should retry only after failures

---

# Task 4 – Custom Dockerfiles in Compose

Instead of pulling a pre-built image for the application, Docker Compose built the image using my own Dockerfile.

Example:

```yaml
web:
  build: .
```

After modifying the Flask application, I rebuilt everything using a single command.

```bash
docker compose up -d --build
```

This rebuilt the image and recreated the application container.

---

# Task 5 – Named Networks & Volumes

## Custom Network

Created a dedicated bridge network.

```yaml
networks:
  app-network:
    driver: bridge
```

Benefits:

- Better organization
- Controlled communication between services
- Easier management for larger projects

---

## Named Volume

Created a persistent volume for PostgreSQL.

```yaml
volumes:
  postgres-data:
```

Mounted it inside PostgreSQL.

```yaml
volumes:
  - postgres-data:/var/lib/postgresql/data
```

### Benefit

Database data remains safe even if the PostgreSQL container is removed.

---

## Labels

Added labels to identify each service.

Example:

```yaml
labels:
  project: "Day34"
  service: "Web Application"
```

Benefits:

- Better organization
- Easier filtering
- Helpful in production environments

---

# Task 6 – Scaling (Bonus)

Attempted to scale the Flask application.

Command:

```bash
docker compose up -d --scale web=3
```

### Observation

Scaling failed because every Flask container attempted to bind to the same host port (`5000`).

Only one container can use a specific host port at a time.

### Why doesn't simple scaling work?

Since all replicas try to expose the same host port, Docker cannot assign the port to multiple containers simultaneously.

In production environments, a reverse proxy or load balancer (such as Nginx or HAProxy) distributes incoming traffic among multiple application instances.

---

# Commands Used

### Build and Start

```bash
docker compose up -d
```

### Build Again After Changes

```bash
docker compose up -d --build
```

### Stop Containers

```bash
docker compose down
```

### View Running Containers

```bash
docker ps
```

### View Compose Services

```bash
docker compose ps
```

### View Logs

```bash
docker compose logs
```

### Check Networks

```bash
docker network ls
```

### Check Volumes

```bash
docker volume ls
```

### Inspect Restart Policy

```bash
docker inspect postgres-app-db --format='{{.HostConfig.RestartPolicy.Name}}'
```

### Scale Application

```bash
docker compose up -d --scale web=3
```

---

# Key Learnings

- Learned how Docker Compose manages multiple containers together.
- Built a complete 3-service application stack.
- Understood service dependencies using `depends_on`.
- Learned the importance of health checks for reliable startup.
- Explored restart policies and their use cases.
- Built custom Docker images directly from Docker Compose.
- Used named networks for service communication.
- Used named volumes for persistent database storage.
- Added labels for better organization.
- Understood why application scaling requires a load balancer when using fixed host port mappings.

---

# Conclusion

Day 34 provided practical experience in building and managing a production-like multi-container application using Docker Compose. It demonstrated how to coordinate multiple services, manage dependencies, persist data, organize containers, and understand real-world deployment considerations. These concepts form the foundation for deploying modern containerized applications efficiently.
