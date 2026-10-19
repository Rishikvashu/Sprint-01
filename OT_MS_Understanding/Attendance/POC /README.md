# Attendance API Setup

<p align="left">
  <img width="120" height="120" alt="Python Flask" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg" />
</p>

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| Shivam Tomar | 12-09-2026 | v1.0 | Shivam Tomar | 12-09-2026 | Ishaan | Anuj Jain | Piyush Upadhyay |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [AWS EC2 Configuration](#3-aws-ec2-configuration)
4. [Install Required Dependencies](#4-install-required-dependencies)
5. [Setup Python Environment](#5-setup-python-environment)
6. [Clone and Setup Attendance API](#6-clone-and-setup-attendance-api)
7. [PostgreSQL Setup](#7-postgresql-setup)
8. [Redis Setup](#8-redis-setup)
9. [Application Configuration](#9-application-configuration)
10. [Database Migration](#10-database-migration)
11. [Start Attendance API](#11-start-attendance-api)
12. [Verification](#12-verification)
13. [Frequently Asked Questions (FAQs)](#13-frequently-asked-questions-faqs)
14. [Conclusion](#14-conclusion)
15. [Contact Information](#15-contact-information)
16. [References](#16-references)

---

# 1. Introduction

This document provides a concise technical SOP for the deployment and basic verification of the Attendance API microservice on a single AWS EC2 instance running Ubuntu 24.04 LTS.

---

# 2. Prerequisites

* PostgreSQL
* Redis (optional)
* Poetry
* Liquibase

---

# 3. AWS EC2 Configuration

| **Parameter** | **Specification** |
| ------------- | ----------------- |
| Cloud Provider | Amazon Web Services (AWS) |
| Region | `ap-south-1` (Mumbai) |
| Instance Name | `attendance-api-poc` |
| Instance Type | `t3.small` |
| Compute & Memory | 2 vCPUs, 2 GiB RAM |
| Operating System | Ubuntu Server 24.04 LTS |
| Architecture | 64-bit x86 |
| Root Volume | 20 GiB EBS gp3 |
| Application Port | `8080` |

---

# 4. Install Required Dependencies

Install the core build tools, development headers, and utilities:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential python3-pip python3-venv libpq-dev git curl wget
```

<img width="731" height="303" alt="install req dep" src="https://github.com/user-attachments/assets/b91e3e5b-3d34-4195-8d53-36388448807a" />

---

# 5. Setup Python Environment

### Step 1: Install pyenv and Python 3.11
```bash
# Install pyenv build dependencies
sudo apt install -y libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

# Install pyenv
curl https://pyenv.run | bash

# Configure shell environment
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
source ~/.bashrc

# Install and activate Python 3.11.13
pyenv install 3.11.13
pyenv global 3.11.13
python --version
```

**Expected Output:**
```text
Python 3.11.13
```

<img width="415" height="40" alt="image" src="https://github.com/user-attachments/assets/d3550a5a-f620-4892-ad02-9f416c1bda16" />

### Step 2: Install Poetry
```bash
curl -sSL https://install.python-poetry.org | python3 -
export PATH="$HOME/.local/bin:$PATH"
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```

<img width="753" height="116" alt="install poetry" src="https://github.com/user-attachments/assets/04a58378-f54c-4b57-b78c-80f9fd7a61ae" />

---

# 6. Clone and Setup Attendance API

```bash
# Clone the repository
git clone https://github.com/OT-MICROSERVICES/attendance-api.git
cd ~/attendance-api

# Initialize environment and install dependencies
poetry env use 3.11.13
poetry install --no-root --no-interaction --no-ansi
poetry add gunicorn
```

<img width="777" height="363" alt="clone and setup att api " src="https://github.com/user-attachments/assets/10b69cb9-6447-4d57-856a-c8ca0dc2c479" />

---

# 7. PostgreSQL Setup

### Step 1: Install and Enable PostgreSQL
```bash
sudo apt install -y postgresql postgresql-contrib
sudo systemctl enable --now postgresql
```

### Step 2: Create Database and User
```bash
sudo -u postgres psql -c "CREATE DATABASE attendance_db;"
sudo -u postgres psql -c "ALTER USER postgres WITH ENCRYPTED PASSWORD 'password';"
```

### Step 3: Verify Connectivity
```bash
PGPASSWORD=password psql -h 127.0.0.1 -U postgres -d attendance_db -c "SELECT current_database(), current_user;"
```

<img width="773" height="212" alt="verify connectivity sql" src="https://github.com/user-attachments/assets/e7fc6f85-df78-455a-a4a2-6214f0ce2780" />

---

# 8. Redis Setup

### Step 1: Install and Enable Redis
```bash
sudo apt install -y redis-server
sudo systemctl enable --now redis-server
```

### Step 2: Verify Status
```bash
redis-cli ping
```

**Expected Output:**
```text
PONG
```

<img width="766" height="231" alt="redis setup" src="https://github.com/user-attachments/assets/bb2482de-9907-4ffe-a841-d34eb9f55c93" />

---

# 9. Application Configuration

Update `config.yaml` to point to localhost services instead of Docker container addresses:

```yaml
postgres:
  database: attendance_db
  host: 127.0.0.1
  port: 5432
  user: postgres
  password: password

redis:
  host: 127.0.0.1
  port: 6379
  password: ""
```

<img width="775" height="287" alt="10th app config" src="https://github.com/user-attachments/assets/9da2e359-48f3-4820-9fdc-d733097a65d5" />

---

# 10. Database Migration

### Step 1: Install Java 17, Liquibase & PostgreSQL JDBC Driver
```bash
# Install OpenJDK 17
sudo apt install -y openjdk-17-jdk

# Install Liquibase
wget -O- https://repo.liquibase.com/liquibase.store.gpg | sudo gpg --dearmor -o /usr/share/keyrings/liquibase.gpg
echo "deb [signed-by=/usr/share/keyrings/liquibase.gpg] https://repo.liquibase.com/ stable main" | sudo tee /etc/apt/sources.list.d/liquibase.list
sudo apt update && sudo apt install -y liquibase

# Download PostgreSQL JDBC Driver
wget -O /tmp/postgresql-42.7.8.jar https://jdbc.postgresql.org/download/postgresql-42.7.8.jar
sudo cp /tmp/postgresql-42.7.8.jar /opt/liquibase/lib/postgresql-jdbc.jar
```

Ensure `liquibase.properties` contains:
```properties
url=jdbc:postgresql://127.0.0.1:5432/attendance_db
driver=org.postgresql.Driver
username=postgres
password=password
changeLogFile=migration/db.changelog-master.xml
```

<img width="779" height="249" alt="ensure liquibase prop" src="https://github.com/user-attachments/assets/b63090e4-6ebd-438b-bb04-0f4bbe690026" />

### Step 2: Run Database Migrations
```bash
cd ~/attendance-api
make run-migrations
```

**Expected Output:**
```text
Running Changeset: migration/db.changelog-master.xml::1::Abhishek Dubey
UPDATE SUMMARY
Run: 1
Previously run: 0
Filtered out: 0
Total change sets: 1
Liquibase: Update has been successful.
```

<img width="789" height="587" alt="run database migrations" src="https://github.com/user-attachments/assets/a27b5a24-6dd8-4329-90b6-d1e9ba18a0f2" />

---

# 11. Start Attendance API

Start the application with Gunicorn listening on port `8080`:

```bash
cd ~/attendance-api
poetry run gunicorn app:app --log-config log.conf -b 0.0.0.0:8080
```

**Expected Output:**
```text
Starting gunicorn 26.2.0
Listening at: http://0.0.0.0:8080
Using worker: sync
Booting worker with pid: 23550
```

<img width="767" height="341" alt="image" src="https://github.com/user-attachments/assets/9197b174-519c-4b64-8d98-b359f4cb533f" />

---

# 12. Verification

### Application Health
```bash
curl -s http://127.0.0.1:8080/api/v1/attendance/health
```

**Expected Output:**
```json
{
  "message": "Attendance API is running fine and ready to serve requests"
}
```

<img width="736" height="40" alt="image" src="https://github.com/user-attachments/assets/db0ddc0e-4ad5-4d79-af89-7b5c9fcc30c8" />

### Swagger UI Documentation
```bash
curl -I http://127.0.0.1:8080/apidocs/
```

**Expected Output:**
```text
HTTP/1.1 200 OK
```

<img width="749" height="146" alt="image" src="https://github.com/user-attachments/assets/f84836cf-abe6-4161-9ecf-ab1e8f6e845d" />
<img width="1229" height="854" alt="image" src="https://github.com/user-attachments/assets/e868d450-2601-48a7-aba8-d2c50a68f446" />

---

# 13. Frequently Asked Questions (FAQs)

#### Q1: Why is pyenv required on Ubuntu 24.04?
Ubuntu 24.04 ships with Python 3.12 by default. The Attendance API dependencies require Python 3.11. `pyenv` allows seamless installation and isolation of Python 3.11 without conflicting with the system Python interpreter.

#### Q2: Why were PostgreSQL and Redis bound to localhost (127.0.0.1)?
Because all services run on the same single EC2 instance for this POC. Binding database and cache services to `127.0.0.1` prevents them from being exposed publicly to the internet, ensuring security.

#### Q3: Why is Liquibase used instead of standard ORM migrations?
Liquibase provides database-agnostic, version-controlled SQL/XML migrations with explicit tracking tables (`databasechangelog`), ensuring consistent database schema management across environments.

#### Q4: Why was Gunicorn added manually using `poetry add gunicorn`?
Gunicorn is a production WSGI HTTP server needed to serve the Flask application across all network interfaces (`0.0.0.0:8080`). Since the base repository was configured for Docker runtime, Gunicorn was added to the Poetry environment to run the microservice natively on EC2.

#### Q5: Why did `config.yaml` need to be modified from `172.17.0.x` to `127.0.0.1`?
The repository default configuration targeted Docker container network IPs (`172.17.0.3` and `172.17.0.4`). Because PostgreSQL and Redis run natively on the EC2 instance in this POC, host addresses were updated to `127.0.0.1`.

---

# 14. Conclusion

The Attendance API was successfully deployed and configured on a single AWS EC2 instance running Ubuntu 24.04 LTS. Local PostgreSQL and Redis services were configured, database schema migrations were completed using Liquibase, and the application was verified through health checks and Swagger UI endpoints on port 8080.

---

# 15. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Shivam Tomar | [shivam.tomar.snaatak@mygurukulam.co](mailto:shivam.tomar.snaatak@mygurukulam.co) |

---

# 16. References

| **Resource / Tool** | **Official Reference Link** |
| ------------------- | --------------------------- |
| Attendance API Repository | [github.com/OT-MICROSERVICES/attendance-api](https://github.com/OT-MICROSERVICES/attendance-api) |
| Python Documentation | [docs.python.org/3.11](https://docs.python.org/3.11/) |
| Poetry Documentation | [python-poetry.org/docs](https://python-poetry.org/docs/) |
| PostgreSQL 16 Documentation | [postgresql.org/docs/16](https://www.postgresql.org/docs/16/) |
| Redis Documentation | [redis.io/docs](https://redis.io/docs/) |
| Liquibase Documentation | [docs.liquibase.com](https://docs.liquibase.com/) |
| Gunicorn Documentation | [docs.gunicorn.org](https://docs.gunicorn.org/) |
