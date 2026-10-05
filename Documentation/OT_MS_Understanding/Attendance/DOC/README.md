# Attendance API – Application Documentation

<p align="left">
  <img width="120" height="120" alt="Python Flask" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg" />
</p>

---

# Author Table

| **Author**    | **Created On** | **Version** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ------------- | -------------- | ----------- | ------------------ | --------------- | --------------- | --------------- |
| Vashishtha Prakash | 09-09-2026     | v1.1      | 22-09-2026         | Sunny/Shubham   | Shreya / Nikita  | Piyush Upadhyay   |


---

# Table of Contents

# Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites-in-tabular-format)
3. [System Requirements](#3-system-requirements)
4. [Dependencies](#4-dependencies)
5. [Ports Configuration](#5-ports-configuration)
6. [Architecture and Data Flow](#6-architecture-and-data-flow)
7. [Step-by-Step Installation and Setup](#7-step-by-step-installation-and-setup)
8. [Application Validation](#8-application-validation)
9. [Monitoring and Health Checks](#9-monitoring-and-health-checks)
10. [Logging](#10-logging)
11. [Disaster Recovery and High Availability](#11-disaster-recovery-and-high-availability)
12. [Troubleshooting](#12-troubleshooting)
13. [Frequently Asked Questions (FAQs)](#13-frequently-asked-questions-faqs)
14. [Recommendations](#14-recommendations)
15. [Conclusion](#15-conclusion)
16. [Contact Information](#16-contact-information)
17. [References](#17-references)

---

# 1. Introduction

This POC of Attendance API is about checking whether the said API will work as configured in the repo. This API is a Python-based microservice designed for managing attendance records through structured REST APIs.

---

# 2. Prerequisites in tabular format

| Category         | Prerequisites                                                      |
| ---------------- | ------------------------------------------------------------------ |
| **AWS**          | AWS Account, IAM Permissions, VPC, Public Subnet, Internet Gateway |
| **EC2**          | EC2 Instance, Security Group, `.pem` Key Pair                      |
| **Local Access** | SSH Client, Private Key (`chmod 400`)                              |
| **Python**       | Python 3.11.x, `pyenv`, Poetry                                     |
| **Database**     | PostgreSQL 16, Redis                                               |
| **Migration**    | OpenJDK 17, Liquibase 5.x, PostgreSQL JDBC Driver                  |
| **Application**  | Gunicorn                                                           |

---

# 3. System Requirements

### 3.1 AWS EC2 Specification

| **Parameter** | **Configuration** |
| ------------- | ----------------- |
| **Instance Name** | `attendance-api-poc` |
| **AMI** | Ubuntu Server 24.04 LTS (64-bit x86) |
| **Instance Type** | `t3.small` (2 vCPU, 2 GiB RAM) |
| **Root Volume** | 20 GiB (EBS gp3, 3000 IOPS, 125 MiB/s, Encrypted) |
| **Private IP** | `172.31.14.43` |
| **Public IP** | Allocated dynamically (e.g., `13.126.157.163`) |

### 3.2 Software Versions Verified

| **Software** | **Version** | **Purpose** |
| ------------ | ----------- | ----------- |
| **Python** | `3.11.13` | Application runtime |
| **Poetry** | `2.4.3` | Dependency management & packaging |
| **PostgreSQL** | `16.15` | Persistent relational database |
| **Redis** | `7.x` | In-memory cache engine |
| **Java** | `OpenJDK 17.0.20` | Runtime environment for Liquibase |
| **Liquibase** | `5.0.4` | Database schema migrations |
| **PostgreSQL JDBC** | `42.7.8` | Database driver for Liquibase migrations |
| **Gunicorn** | `26.2.0` | WSGI application server |
| **pyenv** | `2.8.5` | Python version manager |

---

# 4. Dependencies

### 4.1 Build-Time Dependencies
* `build-essential`, `python3-dev`: Native C compilers and header files.
* `libpq-dev`: PostgreSQL client development libraries required for building `psycopg2`.
* `git`, `make`: Version control and build automation utilities.

```bash
sudo apt install -y build-essential python3-dev python3-pip python3-venv libpq-dev git make
```

### 4.2 Runtime Dependencies
* **Web Framework:** `Flask`, `Flasgger` (Swagger UI).
* **ORM & Database:** `Peewee`, `psycopg2`.
* **Caching:** `redis-py`.
* **Metrics & Validation:** `prometheus-flask-exporter`, `Voluptuous`, `PyYAML`.
* **Server:** `gunicorn`.

---

# 5. Ports Configuration

### 5.1 Inbound Security Group Rules

| **Port** | **Protocol** | **Source** | **Purpose** |
| -------- | ------------ | ---------- | ----------- |
| `22` | TCP | Admin IP (`/32`) | Secure SSH remote administration |
| `8080` | TCP | `0.0.0.0/0` | External Attendance API and Swagger access |

### 5.2 Internal Service Bindings

| **Service** | **Bind Address** | **Port** | **Access Scope** |
| ----------- | ---------------- | -------- | ---------------- |
| **PostgreSQL** | `127.0.0.1` | `5432` | Localhost only (Internal database) |
| **Redis** | `127.0.0.1` | `6379` | Localhost only (Internal cache) |
| **Gunicorn / Flask** | `0.0.0.0` | `8080` | Bound across interfaces for external access |

---

# 6. Architecture and Data Flow

<img width="959" height="304" alt="image" src="https://github.com/user-attachments/assets/a5502d3e-529b-45b0-bc89-da9a6f2bcb8d" />

---

# 7. Step-by-Step Installation and Setup

## Step 1: System Update & Build Dependencies

Connect to the EC2 instance and install core dependencies:

```bash
# Update package index and upgrade packages
sudo apt update && sudo apt upgrade -y

# Install build essentials, dev headers, and git
sudo apt install -y build-essential python3-dev python3-pip python3-venv libpq-dev git curl wget make
```

---

## Step 2: Python 3.11 Setup via pyenv

The application strictly requires **Python 3.11**. Ubuntu 24.04 ships with Python 3.12, so `pyenv` is used to manage Python 3.11 cleanly:

```bash
# Install pyenv prerequisites
sudo apt install -y libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

# Install pyenv
curl https://pyenv.run | bash

# Configure shell environment
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
source ~/.bashrc

# Install Python 3.11.13 and set as local environment
pyenv install 3.11.13
pyenv global 3.11.13

# Verify Python version
python --version
# Output: Python 3.11.13
```

---

## Step 3: Poetry Installation & Repository Setup

```bash
# Install Poetry
curl -sSL https://install.python-poetry.org | python3 -
export PATH="$HOME/.local/bin:$PATH"
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc

# Clone Attendance API repository
cd ~
git clone https://github.com/OT-MICROSERVICES/attendance-api.git
cd ~/attendance-api

# Install dependencies into isolated virtual environment
poetry env use 3.11.13
poetry install --no-root --no-interaction --no-ansi

# Add Gunicorn WSGI server to Poetry environment
poetry add gunicorn
```

---

## Step 4: PostgreSQL Installation & Database Setup

```bash
# Install PostgreSQL server
sudo apt install -y postgresql postgresql-contrib

# Start and enable PostgreSQL service
sudo systemctl enable --now postgresql

# Create application database and configure postgres user password
sudo -u postgres psql -c "CREATE DATABASE attendance_db;"
sudo -u postgres psql -c "ALTER USER postgres WITH ENCRYPTED PASSWORD 'password';"

# Validate database connectivity
PGPASSWORD=password psql -h 127.0.0.1 -U postgres -d attendance_db -c "SELECT current_database(), current_user;"
```

---

## Step 5: Redis Cache Setup

```bash
# Install and start Redis server
sudo apt install -y redis-server
sudo systemctl enable --now redis-server

# Verify Redis operational status
redis-cli ping
# Output: PONG
```

---

## Step 6: Application Configuration Update

The repository default configuration targets Docker container addresses (`172.17.0.3` / `172.17.0.4`). Update `config.yaml` to point to localhost:

```bash
cd ~/attendance-api
cp config.yaml config.yaml.backup
```

Edit `config.yaml` so it reflects the native host bindings:

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

---

## Step 7: Java, Liquibase & JDBC Setup

Liquibase requires Java 17 and the PostgreSQL JDBC driver to perform database migrations:

```bash
# 1. Install OpenJDK 17
sudo apt install -y openjdk-17-jdk

# 2. Install Liquibase from official repository
wget -O- https://repo.liquibase.com/liquibase.store.gpg | sudo gpg --dearmor -o /usr/share/keyrings/liquibase.gpg
echo "deb [signed-by=/usr/share/keyrings/liquibase.gpg] https://repo.liquibase.com/ stable main" | sudo tee /etc/apt/sources.list.d/liquibase.list
sudo apt update && sudo apt install -y liquibase

# 3. Download and install PostgreSQL JDBC Driver
wget -O /tmp/postgresql-42.7.8.jar https://jdbc.postgresql.org/download/postgresql-42.7.8.jar
sudo cp /tmp/postgresql-42.7.8.jar /opt/liquibase/lib/postgresql-jdbc.jar
```

Update `liquibase.properties`:

```properties
url=jdbc:postgresql://127.0.0.1:5432/attendance_db
driver=org.postgresql.Driver
username=postgres
password=password
changeLogFile=migration/db.changelog-master.xml
```

---

## Step 8: Execute Database Migrations

Run Liquibase migrations using the Makefile target:

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

Verify table creation in PostgreSQL:
```bash
PGPASSWORD=password psql -h 127.0.0.1 -U postgres -d attendance_db -c "\dt"
```
*Tables created:* `records`, `databasechangelog`, `databasechangeloglock`.

---

## Step 9: Launch Application with Gunicorn

Start the Attendance API bound to `0.0.0.0:8080`:

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

---

# 8. Application Validation

### 8.1 Local Health Endpoint Check
```bash
curl -s http://127.0.0.1:8080/api/v1/attendance/health | jq .
```
**Response:**
```json
{
  "message": "Attendance API is running fine and ready to serve requests"
}
```

### 8.2 Swagger UI Documentation Check
```bash
curl -I http://127.0.0.1:8080/apidocs/
```
**Response:** `HTTP/1.1 200 OK`

Access via browser:
`http://<EC2_PUBLIC_IP>:8080/apidocs/`

### 8.3 External Public API Check
From an external terminal or web browser:
```bash
curl -s http://<EC2_PUBLIC_IP>:8080/api/v1/attendance/health
```

### 8.4 Validation Summary Matrix

| **Component** | **Validation Method** | **Expected Result** | **Status** |
| ------------- | --------------------- | ------------------- | ---------- |
| **Python Runtime** | `python --version` | `Python 3.11.13` | **PASS** |
| **Poetry Environment** | `poetry run python -c "import flask"` | Clean execution without errors | **PASS** |
| **PostgreSQL DB** | `psql -h 127.0.0.1 -U postgres` | Authenticated connection | **PASS** |
| **Redis Cache** | `redis-cli ping` | `PONG` | **PASS** |
| **Liquibase Migration** | `make run-migrations` | `Update has been successful` | **PASS** |
| **Local Health Check** | `curl /api/v1/attendance/health` | HTTP 200 JSON status | **PASS** |
| **Swagger UI** | `curl -I /apidocs/` | `HTTP/1.1 200 OK` | **PASS** |
| **External Access** | `curl http://<PUBLIC_IP>:8080/...` | Valid JSON response | **PASS** |

---

# 9. Monitoring and Health Checks

### 9.1 Prometheus Metrics Endpoint
The application integrates `prometheus-flask-exporter` to expose performance metrics:
* **Endpoint:** `GET /metrics`
* **Port:** `8080`
* **Exposed Metrics:** HTTP request counts, response latency histograms, error rates, and Python process runtime statistics.

### 9.2 Recommended Production Monitoring Metrics

| **Metric** | **Target** | **Alert Threshold** | **Severity** |
| ---------- | ---------- | ------------------- | ------------ |
| **API Availability** | `/api/v1/attendance/health` | 2 consecutive non-200 responses | Critical |
| **API Latency** | HTTP P95 latency | > 500ms over 5 minutes | High |
| **HTTP 5xx Error Rate** | Error percentage | > 2% of total traffic | Critical |
| **Host CPU Utilization** | EC2 CPU metric | > 80% for 10 minutes | High |
| **Host Memory Usage** | EC2 RAM utilization | > 85% utilization | High |
| **PostgreSQL Connections** | Active connection pool | > 80% max connections | High |

---

# 10. Logging

### 10.1 Logging Configuration
The application initializes logging through `log.conf` passed directly to Gunicorn:
```bash
poetry run gunicorn app:app --log-config log.conf -b 0.0.0.0:8080
```

### 10.2 Recommended Production Logging Architecture
* **Forwarding:** Install AWS CloudWatch Agent, Fluentbit, or Promtail to stream stdout/stderr logs.
* **Storage & Analysis:** Centralize logs in **Amazon CloudWatch Logs**, **OpenSearch**, or an **ELK Stack**.
* **Alerting:** Set up metric filters on error log patterns (e.g., `ERROR`, `CRITICAL`, `Traceback`) to alert on-call teams via SNS/Slack.

---

# 11. Disaster Recovery and High Availability

### 11.1 Production DR Strategy
* **Database Backups:** Automated daily snapshots and point-in-time recovery (PITR) with a 30-day retention window on Amazon RDS.
* **Stateless Compute:** Multi-AZ Auto Scaling Groups deployed across distinct Availability Zones.
* **Infrastructure as Code (IaC):** Complete provisioning defined via Terraform or AWS CloudFormation for deterministic environment replication.

---

# 12. Troubleshooting

| **Issue** | **Root Cause** | **Resolution** |
| --------- | -------------- | -------------- |
| `Permission denied (publickey)` | Private key permissions too open | Run `chmod 400 ~/attendance-api-poc-key.pem` inside WSL/Linux. |
| `Cannot find database driver: org.postgresql.Driver` | Liquibase missing PostgreSQL JDBC JAR | Download `postgresql-42.7.8.jar` and copy to `/opt/liquibase/lib/`. |
| `Command not found: gunicorn` | Gunicorn not packaged in initial Poetry env | Run `poetry add gunicorn` inside the project directory. |
| `Connection refused: 172.17.0.3:5432` | Config referencing Docker container IPs | Update `config.yaml` and `liquibase.properties` host to `127.0.0.1`. |
| `Liquibase download returned 404` | Deprecated GitHub archive URL | Install Liquibase directly via official APT repository. |
| `External curl timeout on port 8080` | Security Group missing inbound rule or bound to 127.0.0.1 | Ensure Gunicorn binds to `0.0.0.0:8080` and AWS SG allows inbound TCP 8080. |
| `Python 3.12 syntax / package failure` | Default Ubuntu 24.04 Python used | Use `pyenv` to enforce `Python 3.11.13` for the virtualenv. |

---

# 13. Frequently Asked Questions (FAQs)

#### Q1: Why is pyenv required on Ubuntu 24.04?
Ubuntu 24.04 ships with Python 3.12 by default. The Attendance API dependencies and lockfile require Python 3.11. `pyenv` allows seamless installation and isolation of Python 3.11 without conflicting with the system Python interpreter.

#### Q2: Why were PostgreSQL and Redis bound to localhost (127.0.0.1)?
Because all services were co-located on the single EC2 instance for this POC. Binding database and cache services to `127.0.0.1` prevents them from being exposed to the public internet, maintaining strong security.

#### Q3: Why is Liquibase used instead of standard ORM migrations?
Liquibase provides database-agnostic, version-controlled SQL/XML migrations with explicit tracking tables (`databasechangelog`), ensuring consistent database schema evolution across environments.

#### Q4: What happens if the EC2 instance is stopped and restarted?
Without an Elastic IP, the public IPv4 address will change upon restart. Always verify the newly assigned public IP from the AWS EC2 console for SSH and external API testing.

---

# 14. Recommendations

1. **Decouple Database and Cache:** Migrate PostgreSQL to **Amazon RDS Multi-AZ** and Redis to **Amazon ElastiCache**.
2. **Implement HTTPS Termination:** Place the application behind an **Application Load Balancer (ALB)** with an AWS ACM SSL/TLS certificate.
3. **Containerize the Workload:** Build standardized Docker images and deploy via **Amazon ECS (Fargate)** or **Amazon EKS** using Kubernetes manifests.
4. **Secure Secret Management:** Externalize database credentials from `config.yaml` to **AWS Secrets Manager** or **HashiCorp Vault**.
5. **Establish CI/CD Pipelines:** Implement automated testing, linting, migration execution, and image deployment via GitHub Actions or GitLab CI.

---

# 15. Conclusion

The **Attendance API** was successfully deployed, configured, and validated on an AWS EC2 instance (`t3.small` running Ubuntu 24.04 LTS) which successfully established Python 3.11 environment isolation via `pyenv` and dependency packaging with `Poetry` , Schema deployment using `Liquibase` and PostgreSQL JDBC , Multi-process WSGI serving with `Gunicorn`. 

---

# 16. Contact Information

| **Name** | **Email**                 |
| -------- | ------------------------- |
| Vashishtha Prakash   | vashishtha.prakash.snaatak@mygurukulam.co |

---

# 17. References

| **Resource** | **URL** |
| ------------ | ------- |
| Attendance API Repository | [github.com/OT-MICROSERVICES/attendance-api](https://github.com/OT-MICROSERVICES/attendance-api) |
| Documentation Template Wiki | [github.com/OT-MICROSERVICES/documentation-template/wiki](https://github.com/OT-MICROSERVICES/documentation-template/wiki/Application-Template) |
| Python Documentation | [docs.python.org/3.11/](https://docs.python.org/3.11/) |
| Flask Web Framework | [flask.palletsprojects.com](https://flask.palletsprojects.com/) |
| Poetry Dependency Manager | [python-poetry.org/docs](https://python-poetry.org/docs/) |
| PostgreSQL 16 Documentation | [postgresql.org/docs/16/](https://www.postgresql.org/docs/16/) |
| Redis Documentation | [redis.io/docs/](https://redis.io/docs/) |
| Liquibase Documentation | [docs.liquibase.com](https://docs.liquibase.com/) |
| Gunicorn WSGI Server | [docs.gunicorn.org](https://docs.gunicorn.org/) |
| Prometheus Python Exporter | [github.com/rycus86/prometheus_flask_exporter](https://github.com/rycus86/prometheus_flask_exporter) |
