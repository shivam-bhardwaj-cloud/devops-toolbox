# Dockerfiles — A Guide to Different Technologies

A structured collection of Dockerfile examples for different programming languages, frameworks, database integrations, and deployment strategies. Each section includes a sample configuration, its use case, key features, and important notes.

---

## 📑 Table of Contents

1. [Python — Flask (Production Ready)](#1-python--flask-production-ready)
2. [Python + MySQL](#2-python--mysql)
3. [Python — Distroless Image](#3-python--distroless-image)
4. [Node.js — Alpine Image](#4-nodejs--alpine-image)
5. [Python — Multi-Stage Build](#5-python--multi-stage-build)
6. [Python — Docker Compose](#6-python--docker-compose)
7. [More Dockerfile Technologies](#7-more-dockerfile-technologies)
8. [Docker Best Practices](#8-docker-best-practices)
9. [How to Add a New Technology](#9-how-to-add-a-new-technology)

---

## 1. Python — Flask (Production Ready)

* **Best for:** Production Python web applications built with Flask.
* **Key features:**
  * Uses a lightweight Python Slim base image.
  * Installs dependencies without retaining pip's download cache.
  * Enables unbuffered Python logs for easier debugging.
  * Runs the application using Gunicorn, a production WSGI server.

### Dockerfile

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV PYTHONUNBUFFERED=1

EXPOSE 5000

CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]
```

### Requirements
* `requirements.txt` must include Flask and Gunicorn.
* The application entry point must match `app:app`.
* Adjust the port and worker count to your application requirements.

---

## 2. Python + MySQL

* **Best for:** Python applications that connect to a MySQL database.
* **Key features:**
  * Separates the application and database into different containers.
  * Uses environment variables for database configuration.
  * Supports persistent database storage.
  * Avoids hardcoding database credentials in the image.

### Dockerfile

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV PYTHONUNBUFFERED=1

EXPOSE 5000

CMD ["python", "app.py"]
```

### compose.yaml

```yaml
services:
  app:
    build: .
    ports:
      - "5000:5000"
    environment:
      DB_HOST: db
      DB_PORT: 3306
      DB_NAME: appdb
      DB_USER: appuser
      DB_PASSWORD: ${DB_PASSWORD}
    depends_on:
      db:
        condition: service_healthy

  db:
    image: mysql:8.4
    environment:
      MYSQL_DATABASE: appdb
      MYSQL_USER: appuser
      MYSQL_PASSWORD: ${DB_PASSWORD}
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    volumes:
      - mysql_data:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  mysql_data:
```

### Important notes
* Configure the Python application to read the database environment variables.
* Store passwords in a local `.env` file or a secrets manager.
* Never commit credentials to version control.
* Implement database connection retry logic when required.

---

## 3. Python — Distroless Image

* **Best for:** Production deployments that need a minimal runtime image and a reduced attack surface.
* **Key features:**
  * Uses a minimal runtime without a conventional shell or package manager.
  * Separates dependency installation from runtime execution.
  * Excludes unnecessary build tools from the final image.
  * Requires careful handling of Python dependencies and native libraries.

### Dockerfile

```dockerfile
FROM python:3.12-slim AS builder

WORKDIR /install

COPY requirements.txt .

RUN pip install --no-cache-dir \
    --prefix=/install/packages \
    -r requirements.txt

FROM gcr.io/distroless/python3-debian12:nonroot

WORKDIR /app

COPY --from=builder /install/packages /usr/local
COPY . .

ENV PYTHONUNBUFFERED=1

EXPOSE 5000

CMD ["-m", "app"]
```

### Important notes
* Verify compatibility with the selected distroless Python runtime.
* Test imports and native dependencies.
* Ensure dependencies are copied into the correct runtime locations.
* For Flask production workloads, configure a compatible production WSGI server.

---

## 4. Node.js — Alpine Image

* **Best for:** Lightweight Node.js applications, APIs, and backend services.
* **Key features:**
  * Uses Alpine Linux for a smaller base image.
  * Installs production dependencies only.
  * Uses a dedicated working directory.
  * Starts the application directly with Node.js.

### Dockerfile

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install --omit=dev

COPY . .

CMD ["node", "server.js"]
```

### Important notes
* Use a currently supported Node.js release.
* For reproducible builds, consider `npm ci --omit=dev` when a valid `package-lock.json` is available.
* Verify Alpine compatibility if the application uses native Node.js modules.

---

## 5. Python — Multi-Stage Build

* **Best for:** Applications that need build tools or compiled dependencies but should not include those tools in the final image.
* **Key features:**
  * Separates dependency installation from runtime execution.
  * Can reduce the final image size.
  * Keeps build-time dependencies out of the runtime stage.

### Dockerfile

```dockerfile
FROM python:3.12-slim AS builder

WORKDIR /build

COPY requirements.txt .

RUN pip install --no-cache-dir \
    --prefix=/install \
    -r requirements.txt

FROM python:3.12-slim

WORKDIR /app

COPY --from=builder /install /usr/local
COPY . .

ENV PYTHONUNBUFFERED=1

CMD ["python", "app.py"]
```

### Important notes
* Use compatible Python versions and base images in both stages.
* Additional system libraries may be required for packages with native dependencies.
* Add a non-root runtime user for production deployments where practical.

---

## 6. Python — Docker Compose

* **Best for:** Running Python applications alongside supporting services such as Redis, PostgreSQL, or MySQL.
* **Key features:**
  * Defines multiple services in one configuration.
  * Simplifies local development.
  * Provides service discovery through service names.
  * Supports health checks, networking, and persistent volumes.

### compose.yaml

```yaml
services:
  app:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - redis

  redis:
    image: redis:7-alpine
```

### Important notes
* The Python application must be configured to connect to Redis using the hostname `redis`.
* Add health checks and persistent storage where appropriate.
* Configure authentication and network access according to your deployment requirements.

---

## 7. More Dockerfile Technologies

This section is reserved for future examples and deployment patterns.

### Python
* Python — FastAPI
* Python — Django
* Python — Flask + Nginx
* Python + PostgreSQL
* Python + Redis
* Python — Alpine Image
* Python — Non-root Container
* Python — Multi-Architecture Build

### JavaScript and TypeScript
* Node.js — Multi-Stage Production Build
* Node.js + PostgreSQL
* React — Nginx Static Hosting
* Next.js — Production Dockerfile

### Other Languages
* Java — Spring Boot
* Go — Minimal Runtime Image
* Go — Distroless Image
* PHP — Apache
* Ruby — Rails

### Docker and Deployment
* Docker Compose — Production Setup
* Docker — Image Optimization
* Docker — Security Best Practices
* Docker — Non-root Containers
* Docker — Health Checks
* Docker — BuildKit and Cache Optimization

---

## 8. Docker Best Practices

* Use supported base image versions.
* Pin image digests when reproducible builds require immutable base images.
* Add a `.dockerignore` file to exclude unnecessary files.
* Avoid storing credentials in Dockerfiles or image layers.
* Use multi-stage builds when they provide meaningful benefits.
* Run containers as non-root users where practical.
* Add health checks where appropriate.
* Scan images for known vulnerabilities.
* Use persistent volumes for data that must survive container replacement.
* Prefer reproducible dependency installation commands.
* Avoid including development dependencies in production images.

### Example `.dockerignore`

```text
.git
.env
.venv
venv/
__pycache__/
*.py[cod]
node_modules/
dist/
build/
.coverage
.pytest_cache/
```

> **Note:** Customize this file for each project. Do not exclude files required by your build.

---

## 9. How to Add a New Technology

Follow this structure for every new Dockerfile example to keep the documentation consistent.

### Technology Name
* **Best for:** Describe the intended use case.
* **Base image:** Specify the image and version.
* **Key features:**
  * Feature 1
  * Feature 2
  * Feature 3

### Dockerfile
```dockerfile
# Add your Dockerfile example here
```

### Build and Run
```bash
docker build -t my-app .
docker run --rm -p 5000:5000 my-app
```

### Important Notes
* Add dependency requirements.
* Explain environment variables.
* Document ports and volumes.
* Mention security considerations and limitations.

---

## Conclusion

This repository serves as a central reference for Dockerfile technologies, from basic application containers to optimized production images. 

The table of contents provides quick navigation to each section, while the standardized structure makes it easy to add new technologies, examples, and best practices over time.
