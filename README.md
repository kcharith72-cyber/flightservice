# Flight Service Microservice

This repository contains the **Flight Service** microservice, which connects to a MySQL database. The application is containerized using **Docker** and orchestrated with **Docker Compose**.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Project Structure](#project-structure)
3. [Setup and Configuration](#setup-and-configuration)
4. [Build and Run](#build-and-run)
5. [Testing the Microservice](#testing-the-microservice)
6. [Database Connectivity](#database-connectivity)

---

## Prerequisites

Make sure the following tools are installed:

* Docker
* Docker Compose
* Git (optional, if cloning the repository)

---

## Project Structure

```
flightservice/
│
├─ Dockerfile
├─ docker-compose.yml
├─ requirements.txt
├─ app.py
└─ README.md
```

* **Dockerfile** – Defines the Docker image for the flightservice.
* **docker-compose.yml** – Defines multi-container setup with MySQL and flightservice.
* **requirements.txt** – Python dependencies.
* **app.py** – Flask application serving flight endpoints.

---

## Setup and Configuration

1. Clone the repository:

```bash
git clone https://github.com/Beer34/flightservice.git
cd flightservice
```

2. Update the environment variables in `docker-compose.yml` if needed:

```yaml
environment:
  DB_HOST: mysql_db
  DB_PORT: 3306
  DB_USER: flightuser
  DB_PASSWORD: StrongUserPass123
  DB_NAME: flightdb
```

3. The Python application reads database credentials from environment variables.

---

## Build and Run

1. Build Docker images:

```bash
docker compose build
```

2. Start the services:

```bash
docker compose up -d
```

3. Check running containers:

```bash
docker compose ps
```

* **flightservice**: Runs on port 5000
* **mysql_db**: Runs on port 3306

---

## Testing the Microservice

1. Access the microservice:

```
http://<EC2-Public-IP>:5000
```

Expected output:

```json
{"message": "Welcome to Flight Service"}
```

2. Verify database connection by running a test script:

```python
import mysql.connector
import os

conn = mysql.connector.connect(
    host=os.getenv("DB_HOST", "mysql_db"),
    user=os.getenv("DB_USER", "flightuser"),
    password=os.getenv("DB_PASSWORD", "StrongUserPass123"),
    database=os.getenv("DB_NAME", "flightdb")
)
cursor = conn.cursor()
cursor.execute("SHOW TABLES;")
print(cursor.fetchall())
conn.close()
```

3. Test data persistence by restarting containers:

```bash
docker compose restart
```

Verify that previously inserted records are still present.

---

## Database Connectivity

* MySQL is exposed on port `3306`

* Environment variables define credentials:

  * User: `flightuser`
  * Password: `StrongUserPass123`
  * Database: `flightdb`
  * Host: `mysql_db`

* Data persists using Docker volumes defined in `docker-compose.yml`.

---

This README provides the complete steps to **build, run, and test** the flightservice microservice using Docker Compose.
