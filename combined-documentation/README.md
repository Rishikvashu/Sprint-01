# 📘 Sprint-01 — OT-Microservices Understanding: Complete Study Guide & Interview Prep

> **Ek hi file se poora Sprint-01 ka study.** Is repo (`Rishikvashu/Sprint-01`) ki saari branches ka content (19 tickets ke docs/POCs) ek jagah concept-wise organize kiya hai: **OT-Microservices** ke 5 services (Attendance, Employee, Salary, Notification, Frontend), unke databases (**PostgreSQL, ScyllaDB/Cassandra, Redis**), poora stack ek saath chalana, aur **SSL/TLS (Nginx + Certbot + Let's Encrypt)**.
>
> **Source:** Har section repo ki branches ke docs/POCs se bana hai. Jo cheezein main ne interview ke liye **extra** jodi hain unpar `🆕 Extra` tag hai, taaki tum sahi bata sako ki "repo mein kya tha" aur "mujhe aur kya aata hai". ⭐ = jo tickets ke author `Vashishtha Prakash` hain (tumhare tickets, inhe sabse deep padho).
>
> **Sprint-0 guide se link:** Poetry, Gunicorn, venv, Go, React/npm, Make, Migrate (golang-migrate), Ansible, SonarQube, Git jaise generic concepts Sprint-0 README mein detail se hain. Yahan unhe repeat nahi kiya, sirf pointer diya hai ("Sprint-0 §N").

---

## 📑 Table of Contents

1. [Sprint-01 Ka Map](#1-sprint-01-ka-map)
2. [Interview Mein Kaise Bolna Hai (60-second Pitch)](#2-interview-mein-kaise-bolna-hai-60-second-pitch)
3. [OT-Microservices: Architecture (Big Picture)](#3-ot-microservices-architecture-big-picture)
4. [⭐ Attendance API (Python / Flask) — SCRUM-65 (POC) + SCRUM-66 (DOC)](#4--attendance-api-python--flask--scrum-65-poc--scrum-66-doc)
5. [Employee API (Go / Gin) — SCRUM-67 (POC) + SCRUM-68 (DOC)](#5-employee-api-go--gin--scrum-67-poc--scrum-68-doc)
6. [Salary API (Java / Spring Boot) — SCRUM-69 (POC) + SCRUM-70 (DOC)](#6-salary-api-java--spring-boot--scrum-69-poc--scrum-70-doc)
7. [Notification Worker (Python) — SCRUM-63 (POC) + SCRUM-64 (DOC)](#7-notification-worker-python--scrum-63-poc--scrum-64-doc)
8. [Frontend (React.js + Nginx) — SCRUM-71 (POC) + SCRUM-72 (DOC)](#8-frontend-reactjs--nginx--scrum-71-poc--scrum-72-doc)
9. [Poora Stack Ek Saath Chalana + Flows — SCRUM-79 (Run OTMS) + SCRUM-80 (Full Stack DOC)](#9-poora-stack-ek-saath-chalana--flows--scrum-79-run-otms--scrum-80-full-stack-doc)
10. [PostgreSQL — SCRUM-75 (POC) + SCRUM-76 (DOC)](#10-postgresql--scrum-75-poc--scrum-76-doc)
11. [⭐ ScyllaDB — SCRUM-77 (Run Locally/EC2) + SCRUM-78 (DOC)](#11--scylladb--scrum-77-run-locallyec2--scrum-78-doc)
12. [Redis — SCRUM-73 (POC) + SCRUM-74 (DOC)](#12-redis--scrum-73-poc--scrum-74-doc)
13. [⭐ SSL/TLS with Nginx + Certbot + Let's Encrypt — SCRUM-114 (POC)](#13--ssltls-with-nginx--certbot--lets-encrypt--scrum-114-poc)
14. [Cross-cutting Concepts (poore sprint mein baar-baar aane wale)](#14-cross-cutting-concepts-poore-sprint-mein-baar-baar-aane-wale)
15. [Quick Comparison Tables](#15-quick-comparison-tables)
16. [Master Interview Q&A (Sprint-01)](#16-master-interview-qa-sprint-01)
17. [Last-Minute Revision Cheat Sheet](#17-last-minute-revision-cheat-sheet)
18. [Gaps / Inconsistencies (Honest Notes)](#18-gaps--inconsistencies-honest-notes)

---

# 1. Sprint-01 Ka Map

## 1.1 Branch → Ticket → Topic

Repo mein har ticket ki apni branch hai. `main` mein sirf title line (`# Sprint-01`) hai, asli content branches mein hai.

| Branch | Ticket | Topic | Type | Section |
|---|---|---|---|---|
| `Scrum-63` | SCRUM-63 | Notification: setup aur run (POC) | POC | §7 |
| `Scrum-64` | SCRUM-64 | Notification: documentation | DOC | §7 |
| `Scrum-65` | SCRUM-65 | Attendance API: setup (POC) | POC | §4 |
| `SCRUM-66` ⭐ | SCRUM-66 | **Attendance API: application documentation** | DOC | §4 |
| `Scrum-67` | SCRUM-67 | Employee API: local setup without Docker (POC) | POC | §5 |
| `Scrum-68` | SCRUM-68 | Employee API: documentation | DOC | §5 |
| `Scrum-69` | SCRUM-69 | Salary API: local setup (POC) | POC | §6 |
| `Scrum-70` | SCRUM-70 | Salary API: documentation | DOC | §6 |
| `Scrum-71` | SCRUM-71 | Frontend: manual setup (POC) | POC | §8 |
| `Scrum-72` | SCRUM-72 | Frontend: documentation | DOC | §8 |
| `Scrum-73` | SCRUM-73 | Redis: standalone + master-replica (POC) | POC | §12 |
| `Scrum-74` | SCRUM-74 | Redis: documentation | DOC | §12 |
| `Scrum-75` | SCRUM-75 | PostgreSQL: setup aur run (POC) | POC | §10 |
| `Scrum-76` | SCRUM-76 | PostgreSQL: documentation | DOC | §10 |
| `Scrum-77` | SCRUM-77 | ScyllaDB: setup on AWS EC2 ("Run Locally") | POC | §11 |
| `Scrum-78` / `SCRUM---78` ⭐ | SCRUM-78 | **ScyllaDB: documentation** | DOC | §11 |
| `Scrum-79` | SCRUM-79 | Full stack: saare components ke saath run | POC | §9 |
| `Scrum-80` | SCRUM-80 | Full stack: architecture aur flows | DOC | §3, §9 |
| `Sprint-114` ⭐ | SCRUM-114 | **SSL: Nginx + Certbot + Let's Encrypt (POC)** | POC | §13 |

> 📝 **Dhyan:** `Scrum-78` aur `SCRUM---78` dono branches ka content bilkul same hai (duplicate branch). Branch ka naam `Sprint-114` hai par ticket **SCRUM-114** hai. SSL ka DOC (SCRUM-113) aur DNS ke tickets ki branches mujhe nahi mili, unka concept §13 mein 🆕 Extra ke roop mein hai.

## 1.2 Pattern: har component ke liye 2 tickets

```text
Component  →  DOC  (kya hai, architecture, dependencies, ports, monitoring, DR/HA, troubleshooting, FAQs)
           →  POC  (asli setup: commands chalake verify karna)
```
Interview mein ye clearly bolo: "Har component ko pehle **documentation** se samjha, phir **POC** se hands-on verify kiya."

## 1.3 Docs ka common template (OT-Microservices documentation template)

Saare docs ek hi structure follow karte hain (isse bhi interview point banta hai: "documentation standardized thi"):

```text
Author Table (Author, Created On, Version, Last Edited, L0/L1/L2 Reviewer)
Introduction / Purpose
Pre-requisites  →  System Requirements  →  Dependencies (Build-time / Run-time / Other)
Important Ports (Inbound / Outbound)
Architecture (+ diagram)
Installation / Setup steps
Validation / Health checks
Monitoring  →  Logging
Disaster Recovery  →  High Availability
Troubleshooting  →  FAQs
Conclusion  →  Contact Information  →  References
```

---

# 2. Interview Mein Kaise Bolna Hai (60-second Pitch)

### 🎤 Sprint-01 ka overall pitch
> "Sprint-01 mein humne **OT-Microservices** naam ka polyglot Employee Management System samjha aur chalaya. Isme 5 services hain: Employee API (Go), Attendance API (Python/Flask), Salary API (Java/Spring Boot), Notification Worker (Python) aur React Frontend, jo Nginx ke peeche chalta hai. Data ke liye har service ka apna database hai: Employee aur Salary ke liye ScyllaDB/Cassandra, Attendance ke liye PostgreSQL, caching ke liye Redis. Har component ka maine documentation aur POC banaya, aur poora stack bina Docker ke Ubuntu/EC2 par manually chalaya. Alag se maine Nginx par Certbot aur Let's Encrypt se HTTPS (SSL/TLS) ka POC kiya."

### 🎤 Tumhare 3 tickets (⭐)
- **SCRUM-66 Attendance API documentation:** "Attendance API ek Python/Flask microservice hai jo PostgreSQL 16 aur optional Redis use karta hai. Maine EC2 (Ubuntu 24.04) par Python 3.11 pyenv se, dependencies Poetry se, schema migration Liquibase se, aur app Gunicorn se port 8080 par chalayi, aur health, Swagger aur Prometheus metrics se verify kiya."
- **SCRUM-78 ScyllaDB documentation:** "ScyllaDB ek Cassandra-compatible, high-performance distributed NoSQL database hai (shard-per-core architecture). Maine uska installation, configuration, CQL operations, monitoring, DR aur HA document kiya."
- **SCRUM-114 SSL POC:** "Domain ko Hostinger DNS se AWS EC2 ke Elastic IP par point kiya, Nginx par Certbot (Let's Encrypt) se certificate liya, HTTP→HTTPS redirect lagaya, aur `certbot renew --dry-run` se auto-renewal verify kiya."

---

# 3. OT-Microservices: Architecture (Big Picture)

## 3.1 Components

| Component | Tech | Kaam | Database / Dependency |
|---|---|---|---|
| **Frontend** | React.js (Node 16.15.1) | Web UI jahan employee, attendance, salary records create/search hote hain | Koi database nahi (stateless) |
| **NGINX** | Nginx | Reverse proxy, **single entry point** | - |
| **Employee API** | **Go** (Gin 1.9.1) | Employee records | **ScyllaDB** (`employee_db` keyspace), Redis optional |
| **Attendance API** | **Python** (Flask, Gunicorn) | Attendance records | **PostgreSQL** (`attendance_db`), Redis optional |
| **Salary API** | **Java** (Spring Boot 3.1.1) | Salary records | **ScyllaDB/Cassandra** (`employee_db.employee_salary`), **Redis** (cache) |
| **Notification Worker** | **Python** | Email notifications (asynchronous) | Elasticsearch (POC mein), SMTP (MailHog) |
| **ScyllaDB** | Cassandra-compatible NoSQL | Employee + Salary data | - |
| **PostgreSQL** | RDBMS | Attendance data | - |
| **Redis** | In-memory store | Caching / supporting operations | - |

> 🔑 **Polyglot microservices:** har service alag language mein hai, **apna database khud own karta hai**, aur sab **REST APIs** se baat karte hain.

## 3.2 Architecture diagram

```text
                      User (Browser)
                           |
                           v
                  React Frontend (static build)
                           |  HTTP (port 80)
                           v
                  ┌─────────────────┐
                  │      NGINX      │   ← single entry point / reverse proxy
                  └───┬─────┬─────┬─┘
                      │     │     │
          ┌───────────┘     │     └────────────┐
          v                 v                  v
   Employee API (Go)  Attendance API (Py)  Salary API (Java)
     :8080                 :8081                :8082
          │                  │                   │ \
          v                  v                   v  v
      ScyllaDB           PostgreSQL           ScyllaDB   Redis (cache)
       :9042               :5432               :9042      :6379

   Notification Worker (Py) — alag chalta hai (frontend path se nahi)
        │   Employee / Salary / Attendance se data leta hai
        └─> Email (SMTP)     [Notification Worker :5000 in api mode]
```

## 3.3 Component communication

| Source | Destination | Purpose |
|---|---|---|
| User | React Frontend | UI interaction |
| React Frontend | NGINX | API requests |
| NGINX | Employee / Salary / Attendance API | Respective operations |
| Employee API, Salary API | ScyllaDB | Employee aur salary data |
| Attendance API | PostgreSQL | Attendance data |
| Application services | Redis | Supporting in-memory operations |
| Notification Service | Employee / Salary / Attendance APIs | Required data retrieve |
| Notification Service | Email | Notifications bhejna |

## 3.4 Request flow

```text
User → React UI → NGINX → (route by path) → Backend API → Database → response wapas
```
- **Notification flow alag hai:** ye normal frontend→nginx path mein nahi aata. Alag trigger hota hai (scheduled / on-demand), data leta hai aur email bhejta hai.

## 3.5 Ports ka cheat table (⚠️ docs mein ports alag-alag hain)

| Service | Port (Run OTMS / Frontend doc) | Docs mein aur kya mila |
|---|---|---|
| Employee API | **8080** | Employee DOC mein 8081 likha hai |
| Attendance API | **8081** | Attendance POC/DOC standalone chalaya toh 8080 |
| Salary API | **8082** | Salary POC mein 8080, Salary DOC mein 8082 |
| Notification (api mode) | **5000** | - |
| Frontend | 3000 (dev/serve) ya 80 (Nginx) | - |
| PostgreSQL | 5432 | - |
| ScyllaDB (CQL) | 9042 | - |
| Redis | 6379 | Replica POC mein 6380 |
| MailHog SMTP / UI | 1025 / 8025 | - |
| Elasticsearch | 9200 | - |

> 📝 **Yaad rakhne wali baat:** Jab saare services ek hi machine par saath chalte hain tab ports alag-alag hone zaroori hain (**8080/8081/8082**), isliye "Run with all components" (SCRUM-79) ka mapping final maano. Jab ek service akeli chalayi (POC), tab woh apna default port (jaise 8080) use kar sakti hai.

---
# 4. ⭐ Attendance API (Python / Flask) — SCRUM-65 (POC) + SCRUM-66 (DOC)

## 4.1 Kya hai?

**Attendance API** ek **Python-based microservice** hai jo attendance records ko structured **REST APIs** se manage karta hai. Repo: `OT-MICROSERVICES/attendance-api`.

| Pehlu | Detail |
|---|---|
| Framework | **Flask** + **Flasgger** (Swagger UI) |
| Database | **PostgreSQL 16** (`attendance_db`) |
| Cache | **Redis 7.x** (optional) |
| Migration | **Liquibase 5.0.4** (Java 17 chahiye) |
| Server | **Gunicorn** (WSGI) |
| Metrics | `prometheus-flask-exporter` (`/metrics`) |
| Python | **3.11.13** (pyenv se) |
| Dependency mgmt | **Poetry 2.4.3** |

**Runtime libraries:** Flask, Flasgger, `psycopg2` (PostgreSQL driver), `redis-py`, `prometheus-flask-exporter`, PyYAML, gunicorn.

## 4.2 Deployment environment (POC: single AWS EC2)

| Parameter | Value |
|---|---|
| Cloud / Region | AWS, `ap-south-1` (Mumbai) |
| Instance | `attendance-api-poc`, `t3.small` (2 vCPU, 2 GiB) |
| OS | Ubuntu Server 24.04 LTS (x86_64) |
| Root volume | 20 GiB EBS gp3 (encrypted) |
| App port | 8080 |

**Security Group (inbound):**

| Port | Source | Purpose |
|---|---|---|
| 22 | Admin IP `/32` | SSH (sirf admin IP se) |
| 8080 | `0.0.0.0/0` | API aur Swagger external access |

**Internal bindings:** PostgreSQL `127.0.0.1:5432`, Redis `127.0.0.1:6379` (**localhost only**, internet par expose nahi), Gunicorn `0.0.0.0:8080`.

## 4.3 Architecture / Data flow

```text
Client ──HTTP:8080──> Gunicorn (Flask app: app:app)
                         ├──> PostgreSQL (127.0.0.1:5432, attendance_db)
                         └──> Redis      (127.0.0.1:6379, optional cache)

Schema migration (alag step):  make run-migrations → Liquibase → PostgreSQL
```

## 4.4 Step-by-step setup (jo POC mein kiya)

**Step 1: System + build dependencies**
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y build-essential python3-dev python3-pip python3-venv libpq-dev git curl wget make
```
`libpq-dev` PostgreSQL client headers deta hai, jo `psycopg2` ko build karne ke liye zaroori hain.

**Step 2: Python 3.11 via pyenv** (kyun: Ubuntu 24.04 mein default Python **3.12** hai, par app 3.11 maangta hai)
```bash
sudo apt install -y libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev \
  libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
curl https://pyenv.run | bash
# ~/.bashrc mein PYENV_ROOT, PATH aur `eval "$(pyenv init -)"` add karo
pyenv install 3.11.13
pyenv global 3.11.13
python --version        # Python 3.11.13
```

**Step 3: Poetry + app clone**
```bash
curl -sSL https://install.python-poetry.org | python3 -
export PATH="$HOME/.local/bin:$PATH"

git clone https://github.com/OT-MICROSERVICES/attendance-api.git
cd ~/attendance-api
poetry env use 3.11.13
poetry install --no-root --no-interaction --no-ansi
poetry add gunicorn        # repo Docker ke liye bani thi, native run ke liye gunicorn add kiya
```

**Step 4: PostgreSQL**
```bash
sudo apt install -y postgresql postgresql-contrib
sudo systemctl enable --now postgresql
sudo -u postgres psql -c "CREATE DATABASE attendance_db;"
sudo -u postgres psql -c "ALTER USER postgres WITH ENCRYPTED PASSWORD 'password';"
PGPASSWORD=password psql -h 127.0.0.1 -U postgres -d attendance_db -c "SELECT current_database(), current_user;"
```

**Step 5: Redis**
```bash
sudo apt install -y redis-server
sudo systemctl enable --now redis-server
redis-cli ping          # PONG
```

**Step 6: `config.yaml` update** (Docker IPs → localhost)
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
> Repo ki default config mein Docker container IPs (`172.17.0.3`, `172.17.0.4`) the. Native run mein PostgreSQL/Redis isi machine par hain, isliye `127.0.0.1`.

**Step 7: DB migration (Liquibase)**
```bash
sudo apt install -y openjdk-17-jdk
# Liquibase ko official APT repo se install karo (GitHub archive URL deprecated, 404 deta hai)
wget -O- https://repo.liquibase.com/liquibase.store.gpg | sudo gpg --dearmor -o /usr/share/keyrings/liquibase.gpg
echo "deb [signed-by=/usr/share/keyrings/liquibase.gpg] https://repo.liquibase.com/ stable main" | sudo tee /etc/apt/sources.list.d/liquibase.list
sudo apt update && sudo apt install -y liquibase

# PostgreSQL JDBC driver
wget -O /tmp/postgresql-42.7.8.jar https://jdbc.postgresql.org/download/postgresql-42.7.8.jar
sudo cp /tmp/postgresql-42.7.8.jar /opt/liquibase/lib/postgresql-jdbc.jar
```
`liquibase.properties`:
```properties
url=jdbc:postgresql://127.0.0.1:5432/attendance_db
driver=org.postgresql.Driver
username=postgres
password=password
changeLogFile=migration/db.changelog-master.xml
```
```bash
cd ~/attendance-api && make run-migrations
# Output: Run: 1 ... Liquibase: Update has been successful.
```

**Step 8: App start**
```bash
poetry run gunicorn app:app --log-config log.conf -b 0.0.0.0:8080
# Output: Starting gunicorn ... Listening at: http://0.0.0.0:8080 ... Using worker: sync
```
`app:app` = file `app.py` ka `app` object. `--log-config log.conf` logging config deta hai.

## 4.5 Verification

```bash
curl -s http://127.0.0.1:8080/api/v1/attendance/health | jq .
# {"message": "Attendance API is running fine and ready to serve requests"}

curl -I http://127.0.0.1:8080/apidocs/            # Swagger UI → HTTP/1.1 200 OK
curl -s http://<EC2_PUBLIC_IP>:8080/api/v1/attendance/health   # external check
```

| Component | Validation | Expected |
|---|---|---|
| Python | `python --version` | 3.11.13 |
| Poetry env | `poetry run python -c "import flask"` | No error |
| PostgreSQL | `psql -h 127.0.0.1 -U postgres` | Connect hota hai |
| Redis | `redis-cli ping` | `PONG` |
| Liquibase | `make run-migrations` | `Update has been successful` |
| Health | `/api/v1/attendance/health` | HTTP 200 JSON |
| Swagger | `/apidocs/` | 200 OK |
| External access | `curl http://<PUBLIC_IP>:8080/...` | JSON |

## 4.6 Monitoring aur Logging

**Prometheus endpoint:** `GET /metrics` (port 8080): request counts, latency histograms, error rates, process stats.

| Metric | Target | Alert threshold | Severity |
|---|---|---|---|
| API availability | `/api/v1/attendance/health` | 2 consecutive non-200 | Critical |
| Latency | HTTP P95 | > 500 ms for 5 min | High |
| 5xx error rate | % of traffic | > 2% | Critical |
| Host CPU | EC2 CPU | > 80% for 10 min | High |
| Host memory | RAM | > 85% | High |
| PostgreSQL connections | Active pool | > 80% of max | High |

**Logging:** `log.conf` Gunicorn ko diya jaata hai. Production mein CloudWatch Agent / Fluent Bit / Promtail se logs forward karo, **CloudWatch Logs / OpenSearch / ELK** mein store karo, aur `ERROR`/`CRITICAL`/`Traceback` par alert (SNS/Slack).

## 4.7 DR aur HA (docs ki recommendations)

| Area | Recommendation |
|---|---|
| DB backups | Automated daily snapshots + PITR (30-day retention) on **Amazon RDS** |
| Compute | Multi-AZ **Auto Scaling Group** (stateless) |
| IaC | Terraform / CloudFormation se environment reproduce |
| Decouple | PostgreSQL → **RDS Multi-AZ**, Redis → **ElastiCache** |
| HTTPS | **ALB + ACM** certificate |
| Containers | Docker image → ECS (Fargate) / EKS |
| Secrets | **AWS Secrets Manager** / Vault (config.yaml mein passwords nahi) |
| CI/CD | GitHub Actions / GitLab CI (tests, lint, migration, image deploy) |

## 4.8 Troubleshooting

| Issue | Root cause | Fix |
|---|---|---|
| `Permission denied (publickey)` | `.pem` key permissions khuli | `chmod 400 key.pem` |
| `Cannot find database driver: org.postgresql.Driver` | Liquibase ko JDBC JAR nahi mila | JAR `/opt/liquibase/lib/` mein copy karo |
| `gunicorn: command not found` | Poetry env mein gunicorn nahi | `poetry add gunicorn` |
| `Connection refused: 172.17.0.3:5432` | Config mein Docker IPs | `config.yaml` aur `liquibase.properties` mein `127.0.0.1` |
| Liquibase download `404` | Purana GitHub archive URL | Official APT repo se install |
| External curl timeout on 8080 | SG rule missing ya `127.0.0.1` par bind | Gunicorn `0.0.0.0:8080` + SG TCP 8080 |
| Python 3.12 par syntax/package failure | System Python use ho raha | `pyenv` se 3.11.13 enforce |

## 4.9 Why? (FAQs jo docs mein hain)

- **pyenv kyun?** Ubuntu 24.04 mein Python 3.12 default hai, app aur lockfile 3.11 maangte hain. pyenv system Python ko chhede bina 3.11 deta hai.
- **PostgreSQL/Redis localhost par kyun bind?** Sab ek hi EC2 par hain; `127.0.0.1` se internet par expose nahi hote (security).
- **Liquibase kyun, ORM migrations kyun nahi?** Database-agnostic, version-controlled SQL/XML migrations, `databasechangelog` tracking table ke saath, environments mein consistent schema.
- **Gunicorn manually kyun add kiya?** Repo Docker runtime ke liye bani thi; native EC2 run ke liye production WSGI server chahiye tha.
- **`config.yaml` ke IPs kyun badle?** Default `172.17.0.x` Docker network ke IPs the.
- **EC2 stop/start par kya hota hai?** Elastic IP na ho toh public IP badal jaata hai.

## 4.10 🆕 Extra concepts (interview ke liye)

| Concept | Matlab |
|---|---|
| **Flask** | Micro web framework (routing, request/response). Production mein Gunicorn ke saath |
| **WSGI** | Python web apps aur web servers ke beech standard interface |
| **Liquibase changelog** | `db.changelog-master.xml` mein changesets (id + author). Apply hone par `DATABASECHANGELOG` table mein record hota hai |
| **pyenv vs venv** | pyenv Python **versions** manage karta hai; venv ek project ka **packages** isolate karta hai |
| **Health vs readiness** | Health = process zinda hai. Readiness = dependencies (DB/Redis) ke saath ready hai |

(Poetry, Gunicorn, venv ka detail: **Sprint-0 §9**.)

## 4.11 🎯 Attendance API — Interview Q&A

**Q1. Attendance API ka stack batao.** Python/Flask, PostgreSQL 16, optional Redis, Liquibase migrations, Gunicorn, Poetry, Swagger (Flasgger), Prometheus metrics.

**Q2. Ubuntu 24.04 par Python 3.11 kyun aur kaise?** Default 3.12 hai par app 3.11 maangta hai. `pyenv install 3.11.13` + `pyenv global`.

**Q3. Poetry se dependencies kaise install kiye?** `poetry env use 3.11.13` → `poetry install --no-root` → `poetry add gunicorn`.

**Q4. Database migration kaise chalayi?** Liquibase (Java 17 + PostgreSQL JDBC) se `make run-migrations`.

**Q5. App ko EC2 par kaise chalaya aur verify kiya?** Gunicorn `0.0.0.0:8080`; health endpoint, `/apidocs/` (Swagger) aur external public-IP curl se.

**Q6. "Connection refused 172.17.0.3" ka matlab?** Config Docker network IPs par point kar rahi hai; native run mein `127.0.0.1` karo.

**Q7. Is setup ko production-ready kaise banaoge?** RDS Multi-AZ + ElastiCache, ALB + ACM (HTTPS), Auto Scaling Group, Secrets Manager, Docker/ECS ya EKS, CI/CD, CloudWatch monitoring.

**Q8. Security ke liye kya kiya?** DB/Redis `127.0.0.1` par bind, SSH sirf admin IP (`/32`), key `chmod 400`, EBS encrypted.

**Q9. Health check se kya pata chalta hai aur metrics kahan?** `/api/v1/attendance/health` app ki availability; `/metrics` Prometheus format mein request/latency data.

---

# 5. Employee API (Go / Gin) — SCRUM-67 (POC) + SCRUM-68 (DOC)

## 5.1 Kya hai?

**Employee API** ek **Go** backend microservice hai jo employee information manage karta hai. Repo: `OT-MICROSERVICES/employee-api`.

| Pehlu | Detail |
|---|---|
| Language | **Go** (1.20+) |
| Web framework | **Gin 1.9.1** (HTTP routing, REST) |
| Database | **ScyllaDB** (keyspace `employee_db`) |
| Cache | **Redis** (optional, sirf jab caching enabled ho) |
| API docs | **Swagger** (`swaggo` + `gin-swagger`) |
| Build tool | **Make** (`make build`, `make swagger`) |
| Dependency mgmt | **Go modules** (`go.mod`) |

> Go aur Make ke concepts: **Sprint-0 §11, §13**. Migrate tool: **Sprint-0 §14**.

## 5.2 Prerequisites, System requirements, Ports

| Requirement | Kyun |
|---|---|
| Linux/Ubuntu (WSL bhi chalta hai) | Environment |
| Go | Build + run |
| Git | Clone |
| Make | Project commands |
| ScyllaDB | Primary DB |
| `cqlsh` | ScyllaDB connectivity verify |
| `curl` | API validate |
| Redis | Optional |

System: dual-core, 2 GB RAM, 5 GB disk.

| Port | Service |
|---|---|
| `8081` (DOC) / `8080` (Run OTMS) | Employee API |
| `9042` | ScyllaDB |
| `6379` | Redis |

## 5.3 Architecture

```text
Client / Frontend ──HTTP──> Gin router ──> Employee API ──> ScyllaDB (employee_db)
                                                    └────> Redis (optional cache)
                          Swagger UI (docs)       /metrics (Prometheus)
```

## 5.4 Local setup without Docker (POC flow)

```bash
cd /mnt/c/OT-MICROSERVICES
git clone https://github.com/OT-MICROSERVICES/employee-api.git && cd employee-api

# 1. ScyllaDB chal raha hai?
sudo systemctl status scylla-server

# 2. config.yaml aur migration.json: Docker IPs ki jagah ScyllaDB host = localhost
cat config.yaml
cat migration.json

# 3. Migration (employee_db keyspace + tables)
#    (existing migrations run karo)

# 4. Build
make build

# 5. Run
export GIN_MODE=release
./employee-api

# 6. Swagger regenerate karna ho toh
make swagger && make build
```

**Verification (POC checklist):** health API, detailed health API (ScyllaDB connectivity), employee search, Prometheus metrics, Swagger UI, Swagger JSON, **Docker used = No**.

| Endpoint | Kaam |
|---|---|
| `GET /api/v1/employee/health` | App availability |
| `GET /api/v1/employee/search/all` | Saare employees |
| `GET /metrics` | Prometheus metrics |
| `/swagger/index.html` | Swagger UI |

## 5.5 Monitoring, DR, HA (DOC se)

- **Monitoring:** `/api/v1/employee/health` aur `/metrics`.
- **Disaster Recovery:**

| Component | Recovery |
|---|---|
| Employee API | Source repo se rebuild + redeploy |
| ScyllaDB | Database backups se restore |
| Configuration | `config.yaml` restore |
| Schema | Zaroorat par migrations dobara chalao |

- **High Availability:** multiple API instances + **load balancer**, highly available ScyllaDB (replication), HA Redis (agar use ho).

## 5.6 Troubleshooting aur Best practices

| Issue | Cause | Fix |
|---|---|---|
| API accessible nahi | App chal nahi raha | Process aur startup logs dekho |
| Port unavailable | Port conflict | `sudo ss -ltnp \| grep :8081` (ya 8080) |
| ScyllaDB connection refused | Scylla down | Port `9042` verify, `systemctl status scylla-server` |

**FAQs:**
- *ScyllaDB kyun?* Primary DB hai; API ko `employee_db` keyspace chahiye.
- *Port busy ho toh?* Process stop karo ya `main.go` mein port badlo, rebuild + restart.
- *Migrations kyun?* Schema/tables banane ke liye.
- *Port `main.go` mein badla par Swagger mein nahi badla toh?* App naye port par chalegi par Swagger purane port par request bhejega. Swagger server URL bhi update karo.

**Best practices (POC doc):** credentials commit mat karo · `go mod tidy` · `go fmt ./...` · local ScyllaDB ka disk space dekho · graceful shutdown (Ctrl+C) · `.gitignore` mein binary, `.env`, `.vscode/`.

## 5.7 🎯 Employee API — Interview Q&A

**Q1. Employee API ka stack?** Go + Gin, ScyllaDB, optional Redis, Swagger (swaggo), Make.

**Q2. Bina Docker ke kaise chalaya?** Local ScyllaDB, config/migration mein Docker IPs ki jagah `localhost`, `make build`, `./employee-api`.

**Q3. `GIN_MODE=release` kyun?** Debug logs band, production-like behaviour.

**Q4. Swagger kaise generate hota hai?** `make swagger` (swaggo annotations se), phir `make build`.

**Q5. Health aur metrics endpoints?** `/api/v1/employee/health` aur `/metrics`.

**Q6. Port conflict par kya karoge?** `ss -ltnp` se process dhundo, stop karo ya port badlo (Swagger URL bhi).

**Q7. DR ka plan?** Repo se rebuild, ScyllaDB backups restore, config restore, migrations re-run.

---

# 6. Salary API (Java / Spring Boot) — SCRUM-69 (POC) + SCRUM-70 (DOC)

## 6.1 Kya hai?

**Salary API** ek **Java Spring Boot** microservice hai jo employee salary records, process dates aur transaction status manage karta hai. Repo: `OT-MICROSERVICES/salary-api`.

| Pehlu | Detail |
|---|---|
| Framework | **Spring Boot 3.1.1** + embedded **Tomcat** |
| Database | **Cassandra-compatible** (ScyllaDB ya Apache Cassandra 3.11), keyspace `employee_db`, table `employee_salary` |
| Cache | **Redis** (read-through lookup cache) |
| Observability | **Spring Boot Actuator** + **Micrometer Prometheus** |
| API docs | **Springdoc OpenAPI 3.0** + Swagger UI |
| Build | **Maven** (`mvnw`) |
| Java | Source Java 17 (runtime JRE 17); POC mein JDK 21 par chalayi |
| Migration | `migrate` tool (`migration.json`) |

> Maven: **Sprint-0 §4**. Redis caching ka detail: **§12**.

## 6.2 Objectives (DOC)

Centralized salary management · independent deployment · **Redis se fast response** · reliable distributed storage (ScyllaDB) · fault isolation · horizontal scalability · easier troubleshooting · better observability (health/metrics) · REST communication.

## 6.3 Architecture aur Request flow

```text
Client → Controller → Salary Service → Redis cache check
                                          ├── CACHE HIT  → cached JSON return
                                          └── CACHE MISS → Cassandra/Scylla query
                                                           → Redis mein store (TTL) → JSON return
```
- Cache sirf **search by ID** par lagta hai: `GET /api/v1/salary/search?id=<id>` (`@Cacheable("salary-search-id")`).
- Redis key format: `com.opstree.microservice.salary.salary-search-id::<id>`.

## 6.4 Endpoints

| Method | Endpoint | Kaam |
|---|---|---|
| POST | `/api/v1/salary/create/record` | Naya salary record |
| GET | `/api/v1/salary/search?id=<id>` | ID se record (Redis cache) |
| GET | `/api/v1/salary/search/all` | Saare records |
| GET | `/actuator/health` | Health (Cassandra, Redis, disk, ping) |
| GET | `/salary-documentation` | Swagger UI |
| GET | `/salary-api-docs` | OpenAPI 3.0 JSON |

## 6.5 POC: local setup (Docker containers ke saath dependencies)

**Environment (verified):** WSL2 Ubuntu 24.04, OpenJDK 21.0.12, Maven 3.8.7, Docker Desktop, Cassandra 3.11 container, Redis 7.2-alpine.

```bash
# 1. Alag Docker network
docker network create salary-net

# 2. Redis (password ke saath)
docker run -d --name salary-redis --network salary-net -p 6379:6379 \
  redis:7.2-alpine redis-server --requirepass password

# 3. Cassandra 3.11 (ScyllaDB ki jagah, full CQL compatibility ke liye)
docker run -d --name salary-scylla --network salary-net -p 9042:9042 cassandra:3.11
```

**Keyspace + table:**
```sql
CREATE KEYSPACE IF NOT EXISTS employee_db
  WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};

CREATE TABLE IF NOT EXISTS employee_salary (
    id text,
    process_date text,
    name text,
    salary float,
    status text,
    PRIMARY KEY (id, process_date)
) WITH CLUSTERING ORDER BY (process_date DESC);
```
```bash
cat migration/000001_create_employee_salary_table.up.sql | docker exec -i salary-scylla cqlsh -k employee_db
docker exec salary-scylla cqlsh -e "USE employee_db; DESCRIBE TABLE employee_salary;"
```

**Config (local):** `application.yml` aur `migration.json` mein Docker IPs (`172.17.0.3/.4`) ki jagah `127.0.0.1`.
```yaml
spring:
  cassandra:
    keyspace-name: employee_db
    contact-points: 127.0.0.1
    port: 9042
    local-datacenter: datacenter1
  data:
    redis:
      host: 127.0.0.1
      port: 6379
```
```json
{ "database": "cassandra://127.0.0.1:9042/employee_db?username=scylladb&password=password" }
```

**Build + run:**
```bash
mvn clean package -DskipTests "-Dlombok.version=1.18.32"
java -jar target/salary-0.1.0-RELEASE.jar
# Tomcat started on port(s): 8080 ... Started SalaryApplication in ~30 s
```
> JDK 21 par default Lombok 1.18.28 `javac` error deta hai, isliye Lombok version command line se override kiya (pom.xml edit kiye bina).

**API tests:**
```bash
curl -i -X POST http://localhost:8080/api/v1/salary/create/record \
  -H "Content-Type: application/json" \
  -d '{"id":"EMP-POC-9001","name":"Jane Doe","salary":85000.5,"processDate":"2026-09-15","status":"PROCESSED"}'
# 201 Created

curl -i "http://localhost:8080/api/v1/salary/search?id=EMP-POC-9001"     # 200
curl -i "http://localhost:8080/api/v1/salary/search/all"                  # 200
docker exec salary-scylla cqlsh -e "SELECT * FROM employee_db.employee_salary;"
curl -s http://localhost:8080/actuator/health                             # status UP (cassandra + redis)
docker exec salary-redis redis-cli -a password KEYS "*"                   # cache key dikhegi
```

## 6.6 Salary API ke "Known Repository Issues" (POC ne dhunde)

| # | Issue | Detail |
|---|---|---|
| 1 | Lombok + JDK 21 incompatibility | `-Dlombok.version=1.18.32` se build |
| 2 | **Cache TTL sirf 1 second** | `SalaryApplication.java` mein `entryTtl(Duration.ofSeconds(1))`, itni short TTL se caching ka fayda nahi |
| 3 | Primary key mismatch | Migration mein composite key `(id, process_date)`, par entity mein sirf `id` par `@PrimaryKey` |
| 4 | Repository type mismatch | `CassandraRepository` ka ID type `UUID`, par `Employee.id` `String` hai |
| 5 | Non-existent ID par **HTTP 500** | Clean 404 hona chahiye |

> 🔑 Interview mein ye batana bahut strong hai: "POC mein sirf chalaya nahi, repository ke bugs bhi identify kiye."

## 6.7 DOC: requirements, ports, monitoring, troubleshooting

| Cheez | Detail |
|---|---|
| Cloud (DOC) | AWS EC2, Ubuntu 24.04 LTS, x86_64 |
| Build-time | JDK 11+, Maven 3.x |
| Run-time | JRE 17, ScyllaDB (Cassandra compatible), Redis, `migrate` |
| Other | Prometheus (optional), Swagger UI (bundled) |
| Port (DOC) | Tomcat `8082`, ScyllaDB `9042` (DOC mein inbound/outbound labels aapas mein ulte likhe hain) |

```bash
curl http://localhost:8082/actuator/health
tail -f ~/salary.log
```

| Issue | Resolution | Command |
|---|---|---|
| App start nahi | Java version aur config | `java -version`, `cat src/main/resources/application.yml` |
| Migration failure | Credentials + migration config | `cat migration.json` |
| Redis connection error | Redis chal raha hai? | `sudo systemctl status redis` |
| Health DOWN | DB connectivity | `curl .../actuator/health` |
| Port in use | Process dhundo | `sudo ss -ltnp` |
| Logs | Runtime errors | `tail -f ~/salary.log` |

**FAQs:** free/open-source hai · cloud-agnostic · enterprise version nahi.

## 6.8 🆕 Extra concepts

| Concept | Matlab |
|---|---|
| **Spring Boot Actuator** | Production-ready endpoints (`/actuator/health`, metrics) |
| **`@Cacheable`** | Method ka result cache mein; dobara call par DB nahi, cache se |
| **Read-through cache** | Pehle cache dekho, miss par DB se laao aur cache mein rakho |
| **TTL** | Cache entry kitni der valid. Bahut chhoti TTL = cache bekaar, bahut lambi = stale data |
| **Partition key vs clustering key** | `PRIMARY KEY (id, process_date)`: `id` = partition key (data kis node par), `process_date` = clustering key (partition ke andar sorting) |
| **Lombok** | Java boilerplate (getters/setters) annotations se generate |

## 6.9 🎯 Salary API — Interview Q&A

**Q1. Salary API ka stack?** Spring Boot 3.1.1 (Tomcat), Cassandra-compatible DB (ScyllaDB/Cassandra), Redis cache, Actuator + Prometheus, Springdoc OpenAPI, Maven.

**Q2. Caching kaise kaam karti hai?** Search-by-ID par `@Cacheable`; Redis hit par seedha response, miss par DB se laa kar Redis mein store (read-through).

**Q3. Cache TTL 1 second kyun problem hai?** Entry itni jaldi expire hoti hai ki cache ka koi fayda nahi; TTL business need ke hisaab se badhao.

**Q4. JDK 21 par build kyun fail hua aur kaise fix kiya?** Spring Boot 3.1.1 ka Lombok 1.18.28 JDK 21 ke saath incompatible; `-Dlombok.version=1.18.32` se override.

**Q5. Cassandra 3.11 kyun use kiya, ScyllaDB kyun nahi?** App Cassandra-compatible hai; Cassandra 3.11 container se full CQL protocol compatibility mili (local POC ke liye).

**Q6. Non-existent ID par 500 kyun aata hai?** Single-row query mapping fail hoti hai jab partition nahi milti; clean 404 return karna chahiye (repo ka known issue).

**Q7. `PRIMARY KEY (id, process_date)` ka matlab?** `id` partition key, `process_date` clustering key; ek employee ke saare salary records ek partition mein, date ke descending order mein.

**Q8. Health endpoint kya dikhata hai?** Actuator `/actuator/health`: overall status aur components (Cassandra, Redis, disk space, ping).

**Q9. Spring Boot app ko production mein kaise chalaoge?** JAR ko systemd service ya container ki tarah, health checks, JVM memory tuning, secrets externalize.

---
# 7. Notification Worker (Python) — SCRUM-63 (POC) + SCRUM-64 (DOC)

## 7.1 Kya hai?

**Notification Worker (Notification API)** OT-Microservices ka **asynchronous email notification service** hai (Python). Repo: `OT-MICROSERVICES/notification-worker`. Ye frontend ke normal request path ka hissa nahi hai; alag trigger hota hai (scheduled ya on-demand), data leta hai aur email bhejta hai.

| Pehlu | Detail |
|---|---|
| Language | Python 3.8+/3.9+ (`pip`, `venv`) |
| Email | SMTP server (POC mein **MailHog** mock SMTP) |
| Data source (POC) | **Elasticsearch** 7.8+ (index `employee-management`) |
| Config | `config.yaml` + env var `CONFIG_FILE` |
| Libraries | Elasticsearch Python client, `schedule` library |
| System libs | `build-essential`, `libxml2-dev`, `libxslt1-dev` |

## 7.2 Modes (`--mode`)

| Mode | Kaam |
|---|---|
| `external` | Ek baar chalke email bhejta hai aur exit karta hai (salary slip alerts, on-demand) |
| `scheduled` | Periodic background run (emails automatically dispatch) |
| `api` | HTTP API ki tarah chalta hai (Run OTMS mein port `5000`, `POST /notification/send`) |

## 7.3 DOC: installation aur config

```bash
sudo apt update
sudo apt install -y python3 python3-pip python3-venv build-essential libxml2-dev libxslt1-dev
cd Snaatak/notification-worker
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

`config.yaml`:
```yaml
smtp:
  from: "test@example.com"
  username: "test"
  password: "test_password"
  smtp_server: "localhost"
  smtp_port: "1025"
elasticsearch:
  username: "elastic"
  password: "elastic_password"
  host: "localhost"
  port: 9200
```
`CONFIG_FILE` ko **persistent** banao:
```bash
export CONFIG_FILE=$(pwd)/config.yaml
echo "export CONFIG_FILE=$(pwd)/config.yaml" >> ~/.bashrc
python3 notification_api.py --mode external
```

| Check | Command | Expected |
|---|---|---|
| Python | `python3 --version` | 3.9+ |
| CONFIG_FILE | `echo $CONFIG_FILE` | `config.yaml` ka path |
| Run | `python3 notification_api.py --mode external` | Email bhejta hai, exit 0 |

## 7.4 POC: end-to-end test (MailHog + Elasticsearch)

**Prerequisites:** Ubuntu 22.04+ / WSL2, Python 3.8+, Docker, Elasticsearch 7.8+ (9200), ports 22, 9200, 1025 (SMTP), 8025 (MailHog UI).

```bash
# 1. MailHog (mock SMTP server) Docker mein
sudo docker run -d -p 1025:1025 -p 8025:8025 --name mailhog mailhog/mailhog
sudo docker ps | grep mailhog

# 2. App clone + requirements
git clone https://github.com/OT-MICROSERVICES/notification-worker.git && cd notification-worker
pip3 install -r requirements.txt --break-system-packages

# 3. Elasticsearch index banao
curl -X PUT "http://127.0.0.1:9200/employee-management" -H 'Content-Type: application/json'

# 4. Test employee data (email_id field zaroori)
curl -X POST "http://127.0.0.1:9200/employee-management/_doc/101" -H 'Content-Type: application/json' \
  -d '{"name":"Rahul Kumar","email_id":"rahul.kumar@otms.com","department":"DevOps","status":"Active"}'

# 5. Run
export CONFIG_FILE="$(pwd)/config.yaml"
python3 notification_api.py --mode external
```
**Verify:** `curl "http://127.0.0.1:9200/employee-management/_search?pretty"` (data ingested) aur browser mein `http://localhost:8025` (MailHog UI mein email dikhni chahiye).

> `--break-system-packages` Ubuntu 24.04 ke PEP 668 restriction ko bypass karta hai. Behtar tareeka **venv** hai (DOC mein venv hi use hua). Detail: **Sprint-0 §9.2**.

## 7.5 Troubleshooting

| Issue | Cause | Fix |
|---|---|---|
| `KeyError: 'email_id'` | Elasticsearch documents mein `email_id` field nahi | `_search` se documents check karo |
| SMTP connection refused | MailHog container band / port 1025 | `docker ps \| grep mailhog` |
| Elasticsearch connection refused | ES down / bind alag | `curl -I http://127.0.0.1:9200` |
| Config file error / cannot parse | `CONFIG_FILE` missing ya galat | `export CONFIG_FILE=$(pwd)/config.yaml` |
| Email send nahi | SMTP host/port/credentials | `config.yaml` check |

## 7.6 Best practices (DOC)

| Practice | Description |
|---|---|
| Containerized runtime | Docker se dev/prod same |
| Secret management | Credentials env variables se, files mein hardcode nahi |
| Auto-restart | systemd service ya Docker restart policy |
| Log rotation | `logrotate` se disk full hone se bachao (**Sprint-0 §8.7**) |

## 7.7 FAQs

- **Elasticsearch kyun, PostgreSQL kyun nahi?** POC doc ke hisaab se PostgreSQL Attendance data ke liye reserved hai, employee master records Elasticsearch mein index hue the (fast search).
- **Email test kaise karein bina real SMTP ke?** **MailHog**: sab outgoing emails port 1025 par pakadta hai aur UI port 8025 par dikhata hai.

## 7.8 🆕 Extra concepts

| Concept | Matlab |
|---|---|
| **Worker / background job** | Service jo request-response ke bajaye kaam queue/schedule se karti hai |
| **SMTP** | Email bhejne ka protocol (25/587/465; yahan mock 1025) |
| **MailHog** | Dev/test ke liye fake SMTP server + web inbox |
| **Elasticsearch** | Search/analytics engine (JSON documents, index, `_search`) |

## 7.9 🎯 Notification — Interview Q&A

**Q1. Notification Worker kya karta hai?** Asynchronous email notifications bhejta hai (jaise salary slip), data Elasticsearch/APIs se leke SMTP se bhejta hai.

**Q2. Ye baaki services se alag kaise hai?** Frontend→Nginx path mein nahi aata; scheduled ya on-demand chalta hai.

**Q3. `external` aur `scheduled` mode mein farak?** `external` ek baar chalke exit; `scheduled` periodic background run.

**Q4. MailHog kyun?** Real emails bheje bina test karne ke liye: SMTP 1025, web UI 8025.

**Q5. `CONFIG_FILE` env var kyun?** App ko config file ka path batata hai; `~/.bashrc` mein daalne se persistent rehta hai.

**Q6. `KeyError: 'email_id'` kaise solve karoge?** Documents mein `email_id` key hona chahiye; `_search` se verify, data seed theek karo.

---

# 8. Frontend (React.js + Nginx) — SCRUM-71 (POC) + SCRUM-72 (DOC)

## 8.1 Kya hai?

**Frontend** OT-Microservices ka **ReactJS** web UI hai: employees aur admins ek hi interface se employee, attendance aur salary records create/search karte hain (REST APIs ko seedha call karne ke bajaye). Repo: `OT-MICROSERVICES/frontend`.

- **Stateless** hai, apna database nahi.
- Build ke liye **Node.js 16.15.1** (+ npm), run ke liye static file server (**`serve`** ya **Nginx**).
- **Directly depend:** Employee API (Go), Attendance API (Python), Salary API (Java). **Indirect:** Notification Worker (frontend usse call nahi karta).

> React aur npm ke concepts: **Sprint-0 §12**. Nginx/systemd basics: **Sprint-0 §8.3**.

## 8.2 Requirements aur Ports

| Cheez | Value |
|---|---|
| Node.js / npm | 16.15.1 (npm bundled) |
| Web server | Nginx (latest) |
| Instance (POC) | t2.small, 2 GB RAM, 8 GB disk, Ubuntu |
| Inbound ports | 22 (SSH), 80 (Nginx), 3000 (dev/serve) |
| Outbound | 8080 Employee, 8081 Attendance, 8082 Salary |

## 8.3 Setup: DOC wala tareeka (build + `serve`)

```bash
cd ~/OT-Micro/frontend
npm install

# Backend APIs reachable hain? (frontend ke kaam ke liye zaroori)
curl -I http://localhost:8080/api/v1/employee/health
curl -I http://localhost:8081/api/v1/attendance/health
curl -I http://localhost:8082/actuator/health

mkdir -p ~/logs
fuser -k 3000/tcp 2>/dev/null          # port 3000 saaf
npm run build                          # production static build (build/)
nohup npx serve -s build -l 3000 > ~/logs/frontend.log 2>&1 &
```
Open: `http://localhost:3000/`. API-level check: `curl http://localhost:8080/api/v1/employee/search/all | jq` (aur attendance/salary).

## 8.4 Setup: POC wala tareeka (Nginx reverse proxy + systemd)

```bash
sudo apt update
sudo apt install nodejs npm -y && node --version && npm --version
sudo apt install nginx -y

git clone https://github.com/OT-MICROSERVICES/frontend.git && cd frontend
npm install

export NODE_OPTIONS=--openssl-legacy-provider     # purane webpack + naye Node ke liye
npm start                                          # http://<PUBLIC-IP>:3000
```

**Nginx reverse proxy** (`/etc/nginx/sites-available/frontend`):
```nginx
server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
```bash
sudo ln -s /etc/nginx/sites-available/frontend /etc/nginx/sites-enabled/frontend
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t                                  # config test
sudo systemctl reload nginx && sudo systemctl status nginx
sudo ss -lntp | grep ':80'
curl http://127.0.0.1
```

**systemd service** (`/etc/systemd/system/frontend.service`) se auto-start:
```bash
sudo systemctl daemon-reload
sudo systemctl enable frontend
sudo systemctl start frontend
sudo systemctl status frontend
```
> Isse reboot ke baad bhi frontend khud start hota hai. Is POC mein **Docker/Kubernetes use nahi hue**.

| Nginx config ka kaam | Detail |
|---|---|
| `listen 80` | HTTP port |
| `proxy_pass 127.0.0.1:3000` | Request React app ko forward |
| `proxy_set_header ...` | Original client info (Host, IP, protocol) backend tak |
| `sites-available` → `sites-enabled` (symlink) | Config enable karne ka Debian/Ubuntu tareeka |
| `nginx -t` | Reload se pehle syntax test |

## 8.5 Monitoring, Logging, Troubleshooting

| Check | Command / URL | Expected |
|---|---|---|
| UI reachable | `http://localhost:3000/` | Page load |
| Employee data | `curl :8080/api/v1/employee/search/all` | Records |
| Attendance data | `curl :8081/api/v1/attendance/search` | Records |
| Salary data | `curl :8082/api/v1/salary/search` | Records |

`tail -f ~/logs/frontend.log`

| Issue | Cause | Fix |
|---|---|---|
| **Blank page** after deploy | Backend APIs unreachable | Teeno APIs ke health endpoints check |
| `npm install`/`build` fail | Node version mismatch | Node **16.15.1** (Dockerfile mein pinned) |
| Port in use | Kuch aur 3000 par | `fuser -k 3000/tcp` ya dusra port |
| Ek page par data nahi | Us service ka backend down / unmigrated | Us service ka health aur logs |

**Stop:** `fuser -k 3000/tcp`.

**FAQs:** blank page = koi API unreachable · `npm run build` fail = Node version · Frontend Notification Worker ko call nahi karta · bina Docker ke chal sakta hai (`npm install && npm run build`, phir `build/` serve).

## 8.6 🎯 Frontend — Interview Q&A

**Q1. Frontend kya hai aur kin services par depend hai?** React UI, stateless; Employee, Attendance, Salary APIs par direct depend, Notification par nahi.

**Q2. Nginx kyun, seedha `npm start` kyun nahi?** `npm start` dev server hai. Production mein Nginx port 80 par serve karta hai, proxy/headers, aur (baad mein) TLS/caching deta hai.

**Q3. Reverse proxy kya hai?** Client ki request leke backend ko forward karta hai (`proxy_pass`) aur response wapas deta hai; client backend ko directly nahi dekhta.

**Q4. `NODE_OPTIONS=--openssl-legacy-provider` kyun?** Purana webpack naye Node ke OpenSSL 3 ke saath chalane ke liye legacy provider chahiye.

**Q5. Frontend ko systemd service kyun banaya?** Crash/reboot ke baad automatic restart aur `systemctl` se management.

**Q6. Blank page aaye toh kya check karoge?** Backend APIs ke health endpoints, browser console/network tab, Nginx/frontend logs.

**Q7. `sites-available` aur `sites-enabled` mein farak?** `available` mein saari configs, `enabled` mein sirf active (symlinks).

---

# 9. Poora Stack Ek Saath Chalana + Flows — SCRUM-79 (Run OTMS) + SCRUM-80 (Full Stack DOC)

## 9.1 OT-Microservices kya hai?

**OT-Microservices** ek **polyglot microservices-based Employee Management System** hai: har service **independently deployed**, **apna database own karti hai**, aur **REST APIs** se baat karti hai. **NGINX** single entry point hai.

(Architecture diagram aur communication table: **§3**.)

## 9.2 Application flows (SCRUM-80)

| Flow | Kaise chalta hai |
|---|---|
| **Overall** | User → React UI → NGINX → right backend service → uska database |
| **Employee** | Frontend → NGINX → Employee API → ScyllaDB |
| **Salary** | Frontend → NGINX → Salary API → ScyllaDB (+ Redis cache) |
| **Attendance** | Frontend → NGINX → Attendance API → PostgreSQL |
| **Notification** | Frontend path se alag; trigger hota hai, Employee/Salary/Attendance se data leta hai, email bhejta hai |

## 9.3 Run with all components (SCRUM-79): order matters

```bash
# 0. Logs folder
mkdir -p ~/logs

# 1. Pehle ports saaf karo
fuser -k 8080/tcp 2>/dev/null
fuser -k 8081/tcp 2>/dev/null
fuser -k 8082/tcp 2>/dev/null
fuser -k 5000/tcp 2>/dev/null

# 2. Infrastructure start (databases + cache + proxy)
sudo systemctl start scylla-server
sudo systemctl start postgresql
sudo systemctl start redis
sudo systemctl start nginx

# 3. Backend services (nohup + background + log file)
cd ~/OT-Micro/employee-api     && nohup go run main.go > ~/logs/employee-api.log 2>&1 &
cd ~/OT-Micro/attendance-api   && nohup poetry run gunicorn app:app -b 0.0.0.0:8081 > ~/logs/attendance.log 2>&1 &
cd ~/OT-Micro/salary-api       && nohup ./mvnw spring-boot:run > ~/logs/salary.log 2>&1 &
cd ~/OT-Micro/notification-worker && pip3 install -r requirements.txt \
      && nohup python3 notification_api.py --mode api > ~/logs/notification.log 2>&1 &

# 4. Frontend build + Nginx restart
cd ~/OT-Micro/frontend && npm install && npm run build && sudo systemctl restart nginx
```

| Service | Start command (short) | Port | Log |
|---|---|---|---|
| Employee (Go) | `go run main.go` | 8080 | `~/logs/employee-api.log` |
| Attendance (Python) | `poetry run gunicorn app:app -b 0.0.0.0:8081` | 8081 | `~/logs/attendance.log` |
| Salary (Java) | `./mvnw spring-boot:run` | 8082 | `~/logs/salary.log` |
| Notification (Python) | `python3 notification_api.py --mode api` | 5000 | `~/logs/notification.log` |
| Frontend | `npm run build` + Nginx | 80 | `/var/log/nginx/*.log` |

## 9.4 Verification

```bash
http://localhost/                                   # Frontend UI

curl http://localhost:8080/api/v1/employee/search/all | jq
curl http://localhost:8081/api/v1/attendance/search | jq
curl http://localhost:8082/api/v1/salary/search/all | jq

# Swagger
curl -I http://localhost:8080/swagger/index.html
curl -I http://localhost:8081/apidocs/
curl -I http://localhost:8082/swagger-ui/index.html

# Notification test
curl -X POST http://localhost:5000/notification/send \
  -H "Content-Type: application/json" \
  -d '{"id":"1","name":"<name>","email":"<email>"}'

# Logs
tail -f ~/logs/*.log /var/log/nginx/access.log /var/log/nginx/error.log

# Stop
fuser -k 8080/tcp; fuser -k 8081/tcp; fuser -k 8082/tcp; fuser -k 5000/tcp; sudo systemctl stop nginx
```

## 9.5 FAQs / Troubleshooting (Run OTMS)

| Problem | Reason | Fix |
|---|---|---|
| Services start nahi ho rahe | Ports occupied ya DB/Redis band | `fuser -k <port>/tcp`, infra pehle start |
| **502 Bad Gateway** (Nginx) | Backend service down | `curl` se 8080/8081/8082 check |
| APIs empty data de rahi | DB initialize/migration nahi hui | Migrations run karo |
| Notification fail | Python dependencies missing | `pip3 install -r requirements.txt` |
| Frontend load nahi | React build nahi bana / Nginx restart nahi | `npm run build`, Nginx restart |

> 🔑 **Start order ka logic:** pehle **infrastructure** (DB, Redis, Nginx), phir **backends**, phir **frontend**. Backend ko DB chahiye; frontend ko backend; isliye ulta order fail hota hai.

## 9.6 🎯 Full Stack — Interview Q&A

**Q1. OT-Microservices ka architecture samjhao.** Polyglot microservices: React UI → Nginx → Employee(Go)/Attendance(Python)/Salary(Java); ScyllaDB (Employee+Salary), PostgreSQL (Attendance), Redis (cache); Notification worker alag.

**Q2. "Database per service" ka kya fayda?** Loose coupling, independent deploy/scale, fault isolation, har service ke liye sahi DB chunne ki azaadi.

**Q3. Ek hi machine par saare services kaise chalaye?** Alag ports (8080/8081/8082/5000), `nohup ... &` se background, logs `~/logs/`, pehle infra phir backend phir frontend.

**Q4. 502 Bad Gateway ka matlab?** Nginx backend tak pahunch nahi paya (backend down ya galat port). Backend ko direct `curl` se check karo.

**Q5. `nohup` aur `&` kya karte hain?** `&` background mein chalata hai, `nohup` terminal band hone par bhi process ko chalne deta hai, output redirect `> file 2>&1`.

**Q6. Notification ko frontend path se alag kyun rakha?** Kaam asynchronous hai (emails); user request ko block nahi karna chahiye.

**Q7. Production mein ye manual setup kaise behtar karoge?** Docker/Kubernetes, systemd/health checks, CI/CD, managed DBs, centralized logging/monitoring, secrets manager.

---

# 10. PostgreSQL — SCRUM-75 (POC) + SCRUM-76 (DOC)

## 10.1 Kya hai?

**PostgreSQL** ek powerful **open-source relational database (RDBMS)** hai: data **tables** mein, **SQL** se query. **ACID** transactions, integrity, concurrency, extensibility.

**OT-Microservices mein:** PostgreSQL **sirf Attendance API** use karta hai (default DB `attendance_db`, port `5432`, user `postgres`). Schema migrations **Liquibase** se.

### Kyun PostgreSQL (Attendance ke liye)?

| Reason | Explanation |
|---|---|
| Structured data | Attendance tabular hai (ID, status, date) |
| SQL queries | `WHERE`, `GROUP BY`, date-based filters aasaan |
| Reliability | Strong consistency (ACID) |

## 10.2 Key features / Purposes (DOC)

| Feature | Description |
|---|---|
| Open source | Free + community support |
| SQL support | Standard SQL |
| **ACID** | Reliable, consistent transactions |
| Indexing | Fast search |
| Security | Authentication, roles, permissions |
| Extensibility | Extensions, custom types/functions |

Purposes: application data storage, data retrieval, transaction processing, relationships between tables, microservices ka DB, data protection (access control + backups).

**Requirements (DOC):** PostgreSQL 14/15/16 · port 5432 · user `postgres` · 2 cores, 2 GB RAM (4 GB recommended), 20 GB disk · Ubuntu 20.04/22.04, Debian, RHEL.

## 10.3 Install + setup (POC)

```bash
sudo apt update && sudo apt install -y postgresql postgresql-contrib
sudo systemctl enable --now postgresql
sudo systemctl status postgresql@16-main.service        # version-specific cluster service

sudo -u postgres psql                                    # superuser shell (peer auth)
```
```sql
CREATE DATABASE attendance_db;
\l                                   -- databases list
ALTER USER postgres PASSWORD 'password';
\q
```

## 10.4 Verification: SQL operations (POC)

```bash
psql -U postgres -h 127.0.0.1 -d attendance_db          # TCP se connect
```
```sql
CREATE TABLE records (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    status VARCHAR(20) NOT NULL,
    date DATE NOT NULL
);

\dt                    -- tables
\d records             -- columns

INSERT INTO records (id, name, status, date) VALUES (101, 'Rahul Kumar', 'Present', '2026-09-11');
SELECT id, name, status, date FROM records ORDER BY id DESC;
SELECT * FROM records WHERE id = 101;
UPDATE records SET status = 'Absent' WHERE id = 101;
DELETE FROM records WHERE id = 101;
TRUNCATE TABLE records;
DROP TABLE records;
```

| psql meta-command | Kaam |
|---|---|
| `\l` | Databases list |
| `\c dbname` | Database badlo |
| `\dt` | Tables list |
| `\d table` | Table structure |
| `\du` | Users/roles |
| `\q` | Exit |

| `DELETE` vs `TRUNCATE` vs `DROP` | Farak |
|---|---|
| `DELETE` | Rows hataata hai (`WHERE` laga sakte ho), rollback ho sakta hai |
| `TRUNCATE` | Saare rows tezi se hataata hai, table structure rehta hai |
| `DROP` | Table hi hata deta hai |

## 10.5 Troubleshooting

| Issue | Fix |
|---|---|
| PostgreSQL start nahi | `sudo systemctl restart postgresql` |
| Connection refused | Port 5432 aur service status check |
| **Authentication error** | **Socket ki jagah TCP use karo**: `psql -U postgres -h 127.0.0.1` |

> 🔑 **Peer vs password auth:** `sudo -u postgres psql` Unix socket par **peer** authentication use karta hai (OS user = DB user). `-h 127.0.0.1` TCP par **password** authentication use karta hai, isliye `ALTER USER ... PASSWORD` zaroori hai.

## 10.6 Best practices (DOC)

Regular backups · strong authentication aur proper permissions · frequently queried columns par indexes · database monitor karo (performance, storage) · updates/security patches.

## 10.7 🆕 Extra concepts

| Concept | Matlab |
|---|---|
| **ACID** | Atomicity (sab ya kuch nahi), Consistency, Isolation, Durability |
| **Primary key / Foreign key** | Row ki unique pehchan / doosri table se relation |
| **Index** | Search tez karne wali data structure (writes thode slow) |
| `pg_dump` / `pg_restore` | Logical backup / restore |
| `pg_hba.conf` | Kaun, kahan se, kaise authenticate kar sakta hai |
| `postgresql.conf` → `listen_addresses` | Kis interface par sunna hai (default localhost) |
| **MVCC** | Readers writers ko block nahi karte (multi-version) |
| **Connection pooling** | PgBouncer jaise tools se connections reuse |

## 10.8 🎯 PostgreSQL — Interview Q&A

**Q1. Attendance ke liye PostgreSQL kyun, NoSQL kyun nahi?** Data structured hai aur SQL filtering/grouping/date queries ka fayda milta hai, strong consistency chahiye.

**Q2. Ye POC mein `127.0.0.1` kyun, Docker IP (172.x) kyun nahi?** Services ek hi host par hain, localhost stable hai.

**Q3. `sudo -u postgres psql` aur `psql -h 127.0.0.1` mein farak?** Pehla Unix socket (peer auth), doosra TCP (password auth).

**Q4. ACID kya hai?** Transactions ki reliability ki 4 guarantees.

**Q5. `DELETE`, `TRUNCATE`, `DROP`?** Rows (condition ke saath) / saare rows / poori table.

**Q6. Migrations kaise manage hote hain?** Liquibase (version-controlled changelog).

**Q7. PostgreSQL ko HA kaise banaoge?** Streaming replication (primary + replica), failover tool (Patroni), ya managed **RDS Multi-AZ**; regular backups + PITR.

**Q8. Backup kaise lete ho?** `pg_dump` (logical), base backup + WAL archiving (PITR), ya RDS snapshots.

---
# 11. ⭐ ScyllaDB — SCRUM-77 (Run Locally/EC2) + SCRUM-78 (DOC)

## 11.1 ScyllaDB kya hai?

**ScyllaDB** ek **distributed NoSQL wide-column database** hai jo **Cassandra-compatible** hai: **CQL** (Cassandra Query Language) aur Cassandra drivers/applications ke saath chalta hai. Focus: **high throughput + low latency**.

**OT-Microservices mein:** **Employee API** aur **Salary API** ka primary database (keyspace `employee_db`, port `9042`).

## 11.2 Key features (DOC)

| Feature | Description |
|---|---|
| Distributed architecture | Data cluster ke multiple nodes mein distribute |
| **High performance** | **Shard-per-core** architecture se resources ka efficient use |
| Horizontal scaling | Nodes add karke capacity badhao |
| Cassandra compatibility | CQL aur Cassandra-compatible clients |
| Replication | Nodes aur datacenters ke beech data replicate |
| Fault tolerance | Node failure mein bhi cluster chalta rehta hai |
| Change Data Capture (CDC) | Database changes downstream ke liye capture |
| Backup & restore | Data protection |

## 11.3 Architecture (concept)

> **Peer-to-peer, shared-nothing**: saare nodes **barabar** hain (koi master nahi). Har node **shard-per-core** design par chalta hai: har CPU core ko apna dedicated data-slice (shard), memory aur local **SSTable** storage milta hai, isliye locking/context-switch kam aur performance zyada.

```text
          Client (CQL driver, port 9042)
                 |  kisi bhi node se connect
        ┌────────┼────────┐
        v        v        v
     Node A   Node B   Node C        ← sab peers (ring), data replicate hota hai
     [core0 shard][core1 shard]...   ← har node mein shard-per-core
```

| Cassandra vs ScyllaDB (🆕 Extra) | Cassandra | ScyllaDB |
|---|---|---|
| Language | Java (JVM, GC pauses) | **C++** (Seastar framework), no JVM |
| Design | Thread-based | **Shard-per-core**, shared-nothing |
| Protocol | CQL | CQL (compatible) |
| Tuning | JVM tuning zaroori | Auto-tuning (`scylla_setup`, IO tuning) |

## 11.4 Prerequisites aur System requirements

| Resource | Recommendation (DOC) |
|---|---|
| CPU | Modern CPU with **SSE4.2** |
| RAM | **16 GB ya 2 GB per logical core** (jo zyada ho); medium/high workload ~64-256 GB |
| Storage | **SSD** strongly recommended |
| Disk:RAM ratio | ~**30:1** general guideline |
| Filesystem | **XFS** recommended |
| Network | 10 Gbps+ large nodes ke liye |
| Access | Root/sudo |

Verification commands: `cat /etc/os-release`, `nproc`, `free -h`, `df -h`, `ping <node-ip>`, `ssh -V`, `sudo -v`, `curl --version`.

**POC instance (SCRUM-77):** AWS `ap-south-1`, `c7i-flex.large` (2 vCPU, ~4 GiB RAM), 30 GB gp3, Ubuntu 24.04, SG: 22 admin IP se, **9042 local only**. (Docs: production mein min 4 GB, ye chhota POC hai.)

## 11.5 Important ports

| Port | Purpose |
|---|---|
| 22 | SSH |
| **7000** | Node-to-node communication (gossip/internode) |
| **9042** | **CQL** client connections |
| 9142 | TLS CQL |
| 10000 | REST/management API |

> Ports ko firewall/Security Group se restrict karo. Internal ports public internet par expose mat karo.

## 11.6 Dependencies

| Runtime | Purpose |
|---|---|
| Linux | OS |
| XFS | Production filesystem |
| systemd | Service management |
| TCP/IP | Client + cluster communication |
| CQL driver | Application connectivity |

| Tool | Purpose |
|---|---|
| `cqlsh` | CQL queries aur schema |
| `nodetool` | Node/cluster administration |
| `scylla_setup` | Initial system/storage/Scylla configuration |
| ScyllaDB Manager | Cluster operations (repair/backup) |
| Monitoring stack | Metrics + alerts |
| Object storage | Backups |

## 11.7 Installation (do tareeke docs mein)

**A) Web installer (POC, SCRUM-77, v2026.3.0):**
```bash
curl -sSf get.scylladb.com/server | sudo bash
scylla --version
sudo scylla_setup
sudo systemctl start scylla-server
sudo systemctl status scylla-server --no-pager -l
nodetool status
cqlsh
```
```sql
DESCRIBE KEYSPACES;
```

**B) APT repository (DOC, SCRUM-78):**
```bash
curl -sSf https://repositories.scylladb.com/scylla/repo/$(lsb_release -sc)/scylladb-2026.1.list \
  | sudo tee /etc/apt/sources.list.d/scylla.list
sudo apt update
sudo apt install scylla -y
scylla --version
sudo scylla_setup
sudo systemctl start scylla-server
sudo systemctl enable scylla-server
sudo systemctl status scylla-server      # active (running)
nodetool status                          # UN = Up + Normal
cqlsh
```

> `nodetool status` mein **`UN`** = **U**p (node chal raha) + **N**ormal (healthy state).

### `scylla_setup` ke options (POC mein jo select kiye)

| Prompt | Selection | Kyun |
|---|---|---|
| Auto-start ScyllaDB service | **Yes** | System ke saath start |
| Check newer version | No | Version fixed |
| Configure **NTP** | **Yes** | Time sync (distributed DB ke liye zaroori) |
| RAID/XFS | No | Single instance POC |
| Coredumps | No | Zaroorat nahi |
| System-wide Scylla settings | No | Default kaafi |
| **I/O tuning (IOTune)** | No | Alag se `scylla_io_setup` se kiya |
| CPU performance governor | No | Zaroorat nahi |
| fstrim | No | Zaroorat nahi |
| ScyllaDB-only mode | No | Instance sirf Scylla ke liye dedicated nahi |
| Remote rsyslog | No | Zaroorat nahi |
| Large-node LimitNOFILES | No | Chhota instance |

> 🔑 **NTP kyun zaroori?** Cassandra/Scylla writes **timestamps** par conflict resolve karte hain (last-write-wins). Nodes ka clock sync na ho toh galat data jeet sakta hai.

## 11.8 Configuration files

| File | Purpose |
|---|---|
| `/etc/scylla/scylla.yaml` | **Main config** |
| `/etc/default/scylla-server` | Service defaults |
| `/etc/scylla/cassandra-rackdc.properties` | Datacenter aur rack config |
| `/etc/scylla.d/io.conf` | I/O tuning (`scylla_io_setup` output) |

## 11.9 Basic CQL operations

| Operation | Purpose |
|---|---|
| `CREATE KEYSPACE` | Logical namespace (RDBMS ke "database" jaisa) |
| `CREATE TABLE` | Table |
| `INSERT` / `SELECT` / `UPDATE` / `DELETE` | CRUD |
| `DESCRIBE` | Schema inspect |

```sql
CREATE KEYSPACE employee_db
  WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
USE employee_db;

CREATE TABLE employee_salary (
  id text, process_date text, name text, salary float, status text,
  PRIMARY KEY (id, process_date)
) WITH CLUSTERING ORDER BY (process_date DESC);

INSERT INTO employee_salary (id, process_date, name, salary, status)
VALUES ('EMP-1', '2026-09-15', 'Jane Doe', 85000.5, 'PROCESSED');
SELECT * FROM employee_salary WHERE id = 'EMP-1';
```

## 11.10 Monitoring

| Metric / Check | Command / Tool |
|---|---|
| Node status | `nodetool status` |
| CPU / Memory / Disk | `top`/`htop`, `free -h`, `df -h` |
| Disk I/O | `iostat` |
| Read/Write latency + throughput | ScyllaDB Monitoring stack |
| Compaction | `nodetool compactionstats` |
| SSTables | `nodetool tablestats` |
| Network | `ss -s`, `sar -n DEV` |
| Service | `sudo systemctl status scylla-server` |
| CQL port | `ss -lntp \| grep 9042` |
| Logs | `journalctl -u scylla-server -n 100`, `-f` |

## 11.11 Disaster Recovery

| Failure | Protection |
|---|---|
| Node failure | Data **replication** |
| Disk failure | Replicated data + backups |
| Data corruption | Backup + restore |
| Accidental deletion | Backup |
| AZ failure | **Multi-AZ** |
| Datacenter failure | **Multi-DC** |
| Region failure | Cross-region strategy + backups |

## 11.12 High Availability

| Component | Recommendation |
|---|---|
| Cluster | Multiple nodes |
| Replication | Appropriate **replication factor** |
| Availability Zones | Nodes AZs mein distribute |
| Datacenters | Zaroorat par multiple DCs |
| Consistency | Application requirement ke hisaab se |
| Backups | Independent backups |
| Monitoring | Health monitoring + alerts |
| Capacity | Node failure sahne layak extra capacity |

## 11.13 Troubleshooting

| Problem | First checks | Command |
|---|---|---|
| Service start nahi | Service, logs, config | `systemctl status scylla-server` |
| Node down | Service + network | `nodetool status` |
| CQL unavailable | Port + RPC config | `ss -lntp \| grep 9042` |
| Node join nahi kar raha | Cluster name, seeds, network | `scylla.yaml` dekho |
| High disk usage | Disk + data dirs | `df -h` |
| High CPU | Processes + workload | `top`/`htop` |
| High latency | CPU, disk, network, queries | Monitoring metrics |
| Cluster communication issue | Firewall + ports | `nc -zv <IP> 7000` |

## 11.14 🆕 Extra concepts (interview mein poochhe jaate hain)

| Concept | Matlab |
|---|---|
| **Keyspace** | Top-level namespace; **replication settings** yahin hoti hain |
| **Replication factor (RF)** | Ek data ki kitni copies (RF=3 = 3 nodes par) |
| **SimpleStrategy vs NetworkTopologyStrategy** | Single DC / production multi-DC (DC-wise RF) |
| **Partition key** | Decide karta hai data kis node par jayega (hash) |
| **Clustering key** | Partition ke andar rows ki sorting |
| **Consistency level** | Read/write mein kitne replicas respond karein (`ONE`, `QUORUM`, `ALL`) |
| **Quorum** | `RF/2 + 1` replicas (RF=3 → 2) |
| **Gossip** | Nodes ek-dusre ki state share karte hain |
| **SSTable / Memtable / Commitlog** | Write path: commitlog + memtable → flush → SSTable |
| **Compaction** | Purani SSTables ko merge karke space/reads optimize |
| **Tombstone** | Delete marker (data turant nahi hatta) |
| **CAP** | Cassandra/Scylla typically **AP** (availability + partition tolerance), consistency tunable |
| **Wide-column store** | Rows mein dynamic columns, partitions ke andar sorted data |

## 11.15 🎯 ScyllaDB — Interview Q&A

**Q1. ScyllaDB kya hai aur kyun use karte hain?** Cassandra-compatible distributed NoSQL DB, high throughput aur low latency ke liye (shard-per-core, C++), horizontally scalable aur fault tolerant.

**Q2. Cassandra se kya alag hai?** C++ (no JVM/GC pauses), shard-per-core shared-nothing design, auto-tuning; CQL aur drivers compatible.

**Q3. Peer-to-peer architecture ka fayda?** Koi single point of failure/master nahi; kisi bhi node se connect, nodes add karke scale.

**Q4. `nodetool status` mein `UN` ka matlab?** Up + Normal, node healthy.

**Q5. `scylla_setup` kya karta hai?** Host kernel aur hardware settings optimize karta hai (NTP, I/O tuning, auto-start waghera).

**Q6. Employee/Salary ke liye ScyllaDB kyun?** Scalable, low latency, replicated storage; Salary mein partition key `id` + clustering `process_date` se ek employee ke records ek hi partition mein sorted.

**Q7. Replication factor aur consistency level ka relation?** RF = copies ki ginti; consistency = kitne replicas se response chahiye. Strong consistency ke liye `R + W > RF` (jaise QUORUM reads + QUORUM writes).

**Q8. Ports kaun se important hain?** 9042 CQL, 7000 inter-node, 9142 TLS CQL, 10000 REST/management, 22 SSH.

**Q9. Production sizing kaise decide karoge?** Workload, dataset size, RF, throughput/latency targets; 16 GB ya 2 GB/core RAM, SSD, XFS, ~30:1 disk:RAM.

**Q10. DR aur HA ke liye kya karoge?** Multiple nodes + appropriate RF, multi-AZ/multi-DC, independent backups (Scylla Manager → object storage), monitoring/alerts.

**Q11. Is POC mein ScyllaDB single node kyun?** Sirf installation/verification ke liye; production mein multi-node cluster chahiye.

---

# 12. Redis — SCRUM-73 (POC) + SCRUM-74 (DOC)

## 12.1 Redis kya hai?

**Redis** (REmote DIctionary Server) ek open-source **in-memory data structure store** hai: **database, cache, message broker aur streaming engine** ki tarah use hota hai. 2009 mein release, **C** mein likha, data **key-value** pairs mein poori tarah **memory** mein, isliye **sub-millisecond** latency; optional persistence disk par.

**OT-Microservices mein:** **Salary API** Redis ko **read-through cache** ki tarah use karta hai (search-by-ID); Employee/Attendance mein **optional** caching.

## 12.2 Purposes (kab use karte hain)

| Use case | Kaise |
|---|---|
| **Caching layer** | Slow DB ke aage, latency aur read load kam |
| **Session store** | User session state app server ke bahar |
| **Leaderboards / ranking** | **Sorted sets** |
| **Rate limiting / counters** | **Atomic increment** operations |
| 🆕 Pub/Sub, queues | Real-time messaging |

## 12.3 Key features

- **In-memory** storage (sub-millisecond)
- **Rich data structures:** strings, hashes, lists, sets, sorted sets, bitmaps, HyperLogLogs, streams, geospatial
- **Persistence:** **RDB** snapshots, **AOF** (append-only file), ya dono
- **Replication** (primary-replica) + **Sentinel** (monitoring + auto failover)
- **Redis Cluster** (sharding + horizontal scaling)
- **Pub/Sub** messaging

## 12.4 Requirements, Ports, Dependencies (DOC)

| Cheez | Detail |
|---|---|
| Version (doc) | Redis 8.8.0 (latest stable, doc ke hisaab se) |
| Instance | 2 vCPU / t3.medium equivalent |
| RAM | 2 GB min; production mein poora working dataset + persistence (fork) ke liye headroom |
| Disk | 10 GB+ |
| OS | Linux (production), macOS (dev), native Windows official nahi |
| Ports | **6379** (Redis), 22 (SSH) |
| Run-time | systemd, OpenSSL (agar TLS) |
| Other | `redis-cli`, `redis-sentinel` (bundled) |

**License (doc):** Redis tri-licensed since March 2024: **AGPLv3** (OSI-approved open source, Redis 8.0 se), **RSALv2**, **SSPLv1** (source-available, restrictions). Self-hosted use free.

## 12.5 Installation (DOC: official Redis APT repo)

```bash
sudo apt-get update && sudo apt-get install -y curl gnupg lsb-release
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" \
  | sudo tee /etc/apt/sources.list.d/redis.list
sudo apt-get update && sudo apt-get install -y redis
sudo systemctl enable --now redis-server
redis-cli ping              # PONG
```
(POC mein simple tareeka: `sudo apt install redis-server` + `sudo service redis-server start`.) Official repo isliye recommended hai kyunki distro repos mein aksar purane versions hote hain.

## 12.6 Configuration (`/etc/redis/redis.conf`)

```conf
bind 127.0.0.1 -::1                 # production mein 0.0.0.0 avoid
requirepass <your-strong-password>  # password authentication
maxmemory 2gb                       # memory cap
maxmemory-policy allkeys-lru        # cap par kaunsi keys hatayein
appendonly yes                      # AOF persistence ON
appendfsync everysec                # AOF har second disk par
save 900 1                          # RDB: 900s mein 1 change
save 300 10
save 60 10000
```
```bash
sudo systemctl restart redis-server    # change apply
```

| Setting | Matlab |
|---|---|
| `bind` | Kis interface par sunna hai |
| `requirepass` | Client password |
| `maxmemory` + `maxmemory-policy` | Memory limit + eviction (`allkeys-lru` = least recently used keys hatao) |
| `appendonly yes` | AOF logging |
| `appendfsync everysec` | Data loss ~1 second tak limit |
| `save <sec> <changes>` | RDB snapshot schedule |

## 12.7 Persistence: RDB vs AOF

| | **RDB** (`dump.rdb`) | **AOF** (`appendonly.aof`) |
|---|---|---|
| Kya | Point-in-time snapshot | Har write command ka log (startup par replay) |
| Size / speed | Compact, restart fast | Bada, restart slower |
| Data loss | Last snapshot ke baad ka data | `everysec` = ~1 second |
| Use | Scheduled off-host backups | Durability |
| Best | **Dono saath (hybrid)** disaster recovery ke liye | |

## 12.8 POC (SCRUM-73): standalone + master-replica

**5.1 Install + start:**
```bash
sudo apt update -y && sudo apt install redis-server -y
sudo service redis-server start && sudo service redis-server status
```

**6.1 CRUD + TTL:**
```bash
redis-cli ping                         # PONG
redis-cli SET poc:user "Palak"
redis-cli GET poc:user
redis-cli SETEX otp:login 60 "852963"  # 60 sec mein expire
redis-cli TTL otp:login                # bacha hua time
redis-cli DEL poc:user                 # delete; GET → (nil)
```

**5.3 Replica instance (port 6380) banana:**
```bash
sudo cp /etc/redis/redis.conf /etc/redis/redis-replica.conf
sudo mkdir -p /var/lib/redis-replica && sudo chown -R redis:redis /var/lib/redis-replica

cat <<EOF | sudo tee -a /etc/redis/redis-replica.conf
port 6380
pidfile /var/run/redis/redis-server-replica.pid
logfile /var/log/redis/redis-server-replica.log
dir /var/lib/redis-replica
replicaof 127.0.0.1 6379
EOF

sudo redis-server /etc/redis/redis-replica.conf --daemonize yes
ps aux | grep redis-server                       # 2 instances dikhne chahiye
redis-cli -p 6379 INFO replication               # connected_slaves:1
redis-cli -p 6380 INFO replication
```

| Parameter | Value | Purpose |
|---|---|---|
| `port` | 6380 | Master (6379) se collision nahi |
| `pidfile` | `/var/run/redis/redis-server-replica.pid` | Replica ka PID |
| `logfile` | `/var/log/redis/redis-server-replica.log` | Alag log |
| `dir` | `/var/lib/redis-replica` | Alag data directory |
| `replicaof` | `127.0.0.1 6379` | Isko master ka replica banao |

**7.1 Replication + read-only validation:**
```bash
redis-cli -p 6379 SET ha_check "Replication Working Perfectly"   # master par write
redis-cli -p 6380 GET ha_check                                    # replica se read
redis-cli -p 6380 SET test_write "Fail"                           # → READONLY error
```

| Check | Result |
|---|---|
| Ping | PONG |
| In-memory read | Microseconds mein |
| TTL expiry | Key khud expire |
| `DEL` | `GET` → `(nil)` |
| Dual daemons | 6379 + 6380 |
| Master-replica link | `connected_slaves:1` |
| Real-time sync | Master write replica par turant |
| **Read-only enforcement** | Replica par write → `READONLY` |

> Replication **asynchronous** hai (master pehle ack karta hai, replica baad mein update hota hai).

## 12.9 Maintenance aur Monitoring

```bash
sudo apt-get update && sudo apt-get install --only-upgrade redis   # upgrade
sudo systemctl restart redis-server

redis-cli ping
redis-cli INFO                  # server, memory, replication, keyspace
redis-cli --latency             # live latency
redis-cli --bigkeys             # bade keys
redis-cli SLOWLOG GET 10        # slow commands
sudo systemctl status redis-server
# Log: /var/log/redis/redis-server.log
```

## 12.10 High Availability aur DR

| Mechanism | Kaam |
|---|---|
| **Replication** | Primary → replicas (data copies) |
| **Sentinel** | Primary monitor karta hai, fail hone par replica ko **auto-promote**, clients ko discovery. **3+ Sentinels** (odd number), alag hosts/AZs mein (quorum) |
| **Redis Cluster** | Data multiple primaries mein **shard**; har primary ka replica; shard fail → replica promote |
| **Managed** | AWS ElastiCache / MemoryDB, Azure Cache, GCP Memorystore |
| **DR** | RDB snapshots (scheduled off-host backup) + AOF (≈1 sec loss) |

## 12.11 Troubleshooting

| Issue | Cause | Solution |
|---|---|---|
| Connection refused / `Could not connect` | Redis down ya bind alag | `systemctl status redis-server`, `bind`/`port` check |
| `NOAUTH Authentication required` | `requirepass` hai par client ne auth nahi kiya | `redis-cli -a <pwd>` ya `AUTH <pwd>` |
| Restart ke baad data gaya | Persistence off | `save` points / `appendonly yes` |
| `MISCONF Redis is configured to save RDB snapshots...` | Disk par likh nahi paa raha (permission/disk full) | Disk space + permissions |
| `READONLY` error | Replica par write | Master (6379) par write karo |
| `Address already in use` | Port collision | Port badlo ya process stop |
| `Cannot allocate memory` | Overcommit policy | `vm.overcommit_memory=1` (`/etc/sysctl.conf`, **Sprint-0 §8.8**) |

## 12.12 Best practices

- `vm.overcommit_memory = 1` (background save fail na ho)
- `maxmemory` + **`allkeys-lru`** (OOM crash se bachao)
- Production mein **`requirepass`** (replica ke liye `masterauth` bhi)
- Har instance ke liye alag `dir`, `pidfile`, `logfile`
- **Hybrid persistence** (RDB + AOF)
- `bind` localhost / private interface, public expose nahi

## 12.13 🆕 Extra concepts

| Concept | Matlab |
|---|---|
| **Cache-aside** | App pehle cache dekhe, miss par DB se laaye aur cache mein daale (app control karta hai) |
| **Read-through** | Cache layer khud DB se laata hai (Salary API mein `@Cacheable` jaisa) |
| **Write-through / write-back** | Write cache + DB mein saath / pehle cache, baad mein DB |
| **TTL** | Key expiry; stale data aur memory control |
| **Eviction policies** | `allkeys-lru`, `volatile-lru`, `allkeys-lfu`, `noeviction` |
| **Cache stampede** | Key expire hote hi bahut saari requests DB par; locks/jitter/early refresh se bachao |
| **Single-threaded** | Commands execute single thread par (atomic) |
| **Common commands** | `SET/GET/DEL/EXPIRE/TTL/INCR/HSET/LPUSH/SADD/ZADD/KEYS(avoid prod)/SCAN` |

## 12.14 🎯 Redis — Interview Q&A

**Q1. Redis kya hai aur kyun fast?** In-memory key-value store; data RAM mein, isliye sub-millisecond latency.

**Q2. Redis kab use karte ho?** Caching, session store, leaderboards (sorted sets), rate limiting/counters, pub/sub.

**Q3. RDB vs AOF?** RDB point-in-time snapshot (compact, kuch data loss possible). AOF har write ka log (`everysec` = ~1 sec loss). Dono saath best.

**Q4. Replication kaise setup kiya?** Alag config (port 6380, alag pid/log/dir) + `replicaof 127.0.0.1 6379`; `INFO replication` se verify.

**Q5. Replica par write kyun fail hoti hai?** Replica read-only hota hai (`READONLY`), data skew se bachne ke liye.

**Q6. Sentinel kya karta hai?** Primary monitor, fail hone par replica ko promote, clients ko naya primary batata hai. 3+ sentinels quorum ke liye.

**Q7. Sentinel vs Cluster?** Sentinel HA/failover (single dataset). Cluster **sharding** (horizontal scale) + HA.

**Q8. `maxmemory-policy allkeys-lru` ka matlab?** Memory limit par sabse kam recently use hui keys hata do.

**Q9. Salary API mein Redis kaise use hota hai?** Search-by-ID par read-through cache; key `...salary-search-id::<id>`, TTL (repo mein 1s, jo bahut kam hai).

**Q10. Cache invalidation kaise karte ho?** TTL, write par key delete/update, event-based invalidation.

**Q11. `vm.overcommit_memory=1` kyun?** Redis background save (fork) ke waqt memory allocate hone ke liye; warna save fail ho sakti hai.

**Q12. Redis production mein secure kaise karoge?** `bind` restrict, `requirepass`/ACL, TLS, firewall/SG, protected-mode.

---
# 13. ⭐ SSL/TLS with Nginx + Certbot + Let's Encrypt — SCRUM-114 (POC)

> Branch ka naam `Sprint-114` hai (folder: `Domain_Security/DNS_SSL/SSL/POC`). SSL ka DOC (SCRUM-113) aur DNS ke docs mujhe nahi mile, isliye unka concept yahan 🆕 Extra ke roop mein diya hai.

## 13.1 POC ka goal

**NGINX, Certbot aur Let's Encrypt** se ek domain (`devsecurity.shop`) ke liye **HTTPS** enable karna. Domain **Hostinger DNS** se **AWS EC2 (Ubuntu)** ke **Elastic IP** par point hai, jahan NGINX chal raha hai.

| Objective | Description |
|---|---|
| SSL setup | Domain ke liye HTTPS |
| Security validation | Encrypted communication confirm |
| DNS configuration | Domain → EC2 server |
| Evidence | Commands + screenshots |
| Auto renewal | Certificate renewal mechanism verify |
| Outcome | HTTPS-secured website |

## 13.2 Prerequisites

| Requirement | Detail |
|---|---|
| Domain | `devsecurity.shop` |
| DNS provider | Hostinger |
| Cloud | AWS EC2, Ubuntu |
| DNS config | Domain → EC2 **Elastic IP** (static; normal public IP restart par badal jaata hai) |
| Web server | NGINX installed + running |
| Ports | **80** (HTTP) aur **443** (HTTPS) open |
| SSL client | **Certbot** |
| CA | **Let's Encrypt** |
| Access | EC2 SSH + Hostinger DNS panel |

## 13.3 Flow

```text
1. DNS: devsecurity.shop + www  ──A record──>  EC2 Elastic IP
2. Browser ──HTTP:80──> NGINX (abhi sirf HTTP)
3. Certbot ──> Let's Encrypt (ACME): "is domain ka control mere paas hai" → challenge (port 80)
4. Let's Encrypt certificate deta hai → Certbot NGINX config mein HTTPS (443) set karta hai
5. HTTP → HTTPS redirect
6. certbot.timer: certificate expire hone se pehle auto-renew
```

## 13.4 Implementation steps (jo POC mein kiye)

### Step 1: DNS verify

Hostinger mein A records:

| Type | Name | Points to | Purpose |
|---|---|---|---|
| A | `@` | EC2 Elastic IP | Root domain |
| A | `www` | EC2 Elastic IP | `www` subdomain |

```bash
nslookup devsecurity.shop
nslookup www.devsecurity.shop
# dono EC2 Elastic IP par resolve hone chahiye
```

### Step 2: SSL se pehle HTTP access check
`http://www.devsecurity.shop` browser mein khulna chahiye (matlab DNS + NGINX + SG port 80 sab theek).

### Step 3: Certbot install
```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
certbot --version
```

### Step 4: Certificate generate
```bash
sudo certbot --nginx -d devsecurity.shop -d www.devsecurity.shop
```
- Certbot **domain ownership validate** karta hai aur **NGINX ko HTTPS ke liye configure** karta hai.
- Prompt aaye toh **`2: Redirect`** chuno, taaki HTTP requests HTTPS par redirect hon:
```text
http://www.devsecurity.shop  --Redirect-->  https://www.devsecurity.shop
```

### Step 5: HTTPS verify
Browser mein `https://www.devsecurity.shop` aur:
```bash
sudo certbot certificates          # certificate + expiry dikhata hai
```

### Step 6: Auto-renewal
```bash
sudo systemctl status certbot.timer        # renewal timer
sudo certbot renew --dry-run               # simulate, asli renew nahi
# Expected: Congratulations, all simulated renewals succeeded
```

## 13.5 Test cases / Validation

| Test | Command | Expected |
|---|---|---|
| DNS | `nslookup devsecurity.shop` | EC2 Elastic IP |
| WWW DNS | `nslookup www.devsecurity.shop` | EC2 Elastic IP |
| NGINX status | `sudo systemctl status nginx` | active (running) |
| HTTP port | `sudo ss -lntp \| grep :80` | NGINX sun raha |
| HTTP access | `curl -I http://www.devsecurity.shop` | Response (ya redirect) |
| Certbot | `certbot --version` | Version |
| HTTPS access | `curl -I https://www.devsecurity.shop` | Successful HTTPS |
| HTTPS port | `sudo ss -lntp \| grep :443` | NGINX 443 par |
| SSL check | `openssl s_client -connect www.devsecurity.shop:443 -servername www.devsecurity.shop` | Valid cert + TLS |
| Redirect | `curl -I http://www.devsecurity.shop` | `301` → HTTPS |
| Cert details | `sudo certbot certificates` | Cert + expiry |
| Renewal | `sudo certbot renew --dry-run` | Simulated renewal success |

**Certificate inspect:**
```bash
echo | openssl s_client -connect www.devsecurity.shop:443 -servername www.devsecurity.shop 2>/dev/null \
  | openssl x509 -noout -subject -issuer -dates
```
`-servername` SNI ke liye hai (ek IP par multiple domains).

## 13.6 Limitations (POC)

| Limitation | Detail |
|---|---|
| Public domain | Validation ke liye domain publicly accessible hona chahiye |
| DNS dependency | Galat DNS = validation fail |
| Port requirement | 80/443 available hone chahiye (validation method ke hisaab se) |
| Certificate expiry | Let's Encrypt certs ki validity limited (**90 din**), renewal zaroori |
| Renewal dependency | Certbot timer/service + server connectivity |
| Server dependency | EC2/Nginx availability aur sahi config |

## 13.7 Rollback plan

```bash
sudo nginx -t                         # pehle config valid hai?
sudo systemctl restart nginx          # valid ho toh restart
sudo nginx -T                         # active config dump, SSL hatani ho toh pehle review
sudo certbot certificates             # cert info
```
Rollback **dhyan se** karo taaki chalti hui HTTP/HTTPS service na ruke.

## 13.8 FAQs (POC se)

| Question | Answer |
|---|---|
| Let's Encrypt free hai? | Haan, free, publicly trusted certificates |
| Validity? | **90 din** |
| Domain validation zaroori? | Haan, issuance se pehle domain control verify hota hai |
| Private key share hoti hai? | Nahi, server par secure rehti hai, expose mat karo |
| Port 443 kyun? | HTTPS ka standard port |
| Port 80 kyun? | HTTP access, HTTP-based domain validation, aur HTTPS par redirect |
| SSL setup ke baad HTTP chal sakta hai? | Sirf HTTPS par redirect karne ke liye |
| Kya SSL DNS lookup bhi encrypt karta hai? | **Nahi.** DNS aur HTTPS alag mechanisms hain; HTTPS connection ban jaane ke baad ka web traffic encrypt karta hai |

## 13.9 🆕 Extra: SSL/TLS concepts

### SSL vs TLS
**SSL** purana naam/protocol hai (ab deprecated). **TLS** (1.2, **1.3**) uska modern version hai. Log abhi bhi "SSL certificate" bolte hain, par actually TLS chalta hai.

### TLS kya deta hai? (3 cheezein)
| Property | Matlab |
|---|---|
| **Encryption** | Beech mein koi data padh nahi sakta |
| **Authentication** | Server wahi hai jo woh keh raha hai (certificate se) |
| **Integrity** | Data raaste mein badla nahi |

### TLS handshake (simplified)
```text
Client ──ClientHello (supported TLS versions, ciphers, SNI)──────────────> Server
Client <──ServerHello + Certificate (public key) ────────────────────────  Server
Client: certificate verify (CA trusted? domain match? expired?)
Client ── key exchange ──> shared session key ban jaata hai
Dono: ab symmetric encryption se data (fast)
```
- **Asymmetric** (public/private key) sirf handshake mein, **symmetric** actual data ke liye (fast).

### Certificate chain of trust
```text
Root CA (browser/OS mein pehle se trusted)
   └─ Intermediate CA (Let's Encrypt)
         └─ Tumhara server certificate (devsecurity.shop)
```
- Server `fullchain.pem` bhejta hai (apna cert + intermediates).

### Certbot ke files (default location)
| File | Kaam |
|---|---|
| `/etc/letsencrypt/live/<domain>/fullchain.pem` | Certificate + chain |
| `/etc/letsencrypt/live/<domain>/privkey.pem` | **Private key** (secret) |
| `/etc/letsencrypt/renewal/<domain>.conf` | Renewal config |

### Certbot ne NGINX mein kya badla? (typical)
```nginx
server {
    listen 443 ssl;
    server_name devsecurity.shop www.devsecurity.shop;
    ssl_certificate     /etc/letsencrypt/live/devsecurity.shop/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/devsecurity.shop/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
}
server {
    listen 80;
    server_name devsecurity.shop www.devsecurity.shop;
    return 301 https://$host$request_uri;      # HTTP → HTTPS redirect
}
```

### ACME challenges
| Challenge | Kaise | Kab |
|---|---|---|
| **HTTP-01** | Port 80 par ek token file serve karke ownership prove | Normal single domain (POC yahi) |
| **DNS-01** | DNS mein TXT record banake prove | **Wildcard** (`*.example.com`) ya jab port 80 band ho |

### Certificate types
**DV** (Domain Validated, Let's Encrypt), **OV** (Organization Validated), **EV** (Extended Validation). Wildcard = ek cert saare subdomains ke liye.

### Common SSL errors
| Error | Reason | Fix |
|---|---|---|
| Certbot "challenge failed" / timeout | DNS galat ya port 80 SG/firewall mein band | `nslookup`, SG inbound 80/443 |
| `NET::ERR_CERT_DATE_INVALID` | Cert expire | `certbot renew`, timer check |
| Cert name mismatch | Domain cert mein nahi (`-d` miss) | Sahi `-d` ke saath dobara |
| Mixed content | HTTPS page par HTTP resources | Resources HTTPS karo |
| Rate limit hit | Let's Encrypt limits | Limits wait/staging use |
| Renewal fail | Timer/connectivity/port 80 | `certbot renew --dry-run` |

### Security hardening (extra)
**HSTS** (`Strict-Transport-Security` header, browser sirf HTTPS use kare), sirf **TLS 1.2/1.3** allow, weak ciphers band, **SSL Labs** (`ssllabs.com/ssltest`) se grade check.

## 13.10 🆕 Extra: DNS basics (folder `DNS_SSL` ka hissa)

| Record | Kaam |
|---|---|
| **A** | Domain → IPv4 address |
| **AAAA** | Domain → IPv6 |
| **CNAME** | Alias (ek naam → doosra naam) |
| **MX** | Mail servers |
| **TXT** | Text (verification, SPF/DKIM) |
| **NS** | Domain ke authoritative nameservers |

```text
Browser → Resolver (ISP/8.8.8.8) → Root → TLD (.shop) → Authoritative NS (Hostinger) → IP
```
- **TTL:** record kitni der cache hoga. Change ke baad **propagation** mein time lagta hai.
- Tools: `nslookup`, `dig devsecurity.shop +short`, `host`.
- **Elastic IP:** AWS ka static public IP (instance stop/start par nahi badalta), isliye DNS A record ke liye zaroori.

## 13.11 🎯 SSL — Interview Q&A

**Q1. SSL/TLS kya hai aur kyun zaroori?** Client-server ke beech encrypted, authenticated, tamper-proof communication (HTTPS). Data sniffing/man-in-the-middle se bachata hai.

**Q2. HTTPS setup ke steps batao (POC).** DNS A records → NGINX HTTP check → Certbot install → `certbot --nginx -d domain -d www.domain` (redirect option) → HTTPS verify → `certbot renew --dry-run`.

**Q3. Certbot kaam kaise karta hai?** ACME protocol se Let's Encrypt ko domain control prove karta hai (HTTP-01: port 80), certificate leta hai, NGINX config HTTPS ke liye update karta hai, aur renewal schedule karta hai.

**Q4. Let's Encrypt ki validity 90 din kyun, auto-renew kaise?** Short validity security ke liye; Certbot ka systemd timer (`certbot.timer`) expiry se pehle renew karta hai. `certbot renew --dry-run` se test.

**Q5. Port 80 ki zaroorat HTTPS ke baad bhi kyun?** HTTP-01 validation aur HTTP → HTTPS redirect ke liye.

**Q6. TLS handshake samjhao.** ClientHello → ServerHello + certificate → client certificate verify karta hai → key exchange → symmetric session key → encrypted data.

**Q7. Certificate chain of trust?** Root CA → Intermediate CA → server certificate. Browser Root CA ko pehle se trust karta hai.

**Q8. Wildcard certificate kaise milta hai?** DNS-01 challenge se (TXT record).

**Q9. Private key leak ho jaaye toh?** Certificate revoke karo, naya key + cert issue karo, server par rotate.

**Q10. Kya HTTPS DNS lookup encrypt karta hai?** Nahi; DNS alag hai (DoH/DoT alag cheezein hain).

**Q11. Certificate expire ho gaya, kya check karoge?** `certbot certificates`, `systemctl status certbot.timer`, `certbot renew --dry-run`, port 80/DNS/SG.

**Q12. DNS A record aur Elastic IP ka relation?** A record domain ko IP par map karta hai; Elastic IP static hai, instance restart par IP nahi badalta, isliye DNS stable rehta hai.

**Q13. Rollback ka plan kya tha?** `nginx -t` se config validate, `nginx -T` se active config review, zaroorat par restart; careful rollback taaki service na ruke.

---

# 14. Cross-cutting Concepts (poore sprint mein baar-baar aane wale)

## 14.1 "Docker IPs → localhost" pattern (har service mein)

Repo ki default configs **Docker container network IPs** (`172.17.0.3`, `172.17.0.4`) par point karti thin. **Bina Docker ke** (native) chalane par ye badalna pada:

| Service | Kahan badla | Naya value |
|---|---|---|
| Attendance | `config.yaml`, `liquibase.properties` | `127.0.0.1` |
| Employee | `config.yaml`, `migration.json` | `localhost` |
| Salary | `application.yml`, `migration.json` | `127.0.0.1` |

> Lesson: **configuration ko code se alag** rakho (12-factor). Environment ke hisaab se host badalna config ka kaam hai, code ka nahi.

## 14.2 Migration tools ka comparison

| Service | Tool | File / config | Tracking |
|---|---|---|---|
| Attendance (PostgreSQL) | **Liquibase** (Java 17 + JDBC) | `migration/db.changelog-master.xml`, `liquibase.properties`, `make run-migrations` | `DATABASECHANGELOG` table |
| Employee / Salary (ScyllaDB) | **migrate** (golang-migrate) | `migration.json` (`cassandra://...`), `migration/000001_..._up.sql` | `schema_migrations` |

> `migration.json` ka `cassandra://host:9042/keyspace` format aur `000001_..._up.sql` naming **golang-migrate** ki pehchan hai (Sprint-0 §14). Salary POC mein `up.sql` seedha `cqlsh` se pipe karke bhi apply kiya gaya.

## 14.3 Observability: health, metrics, API docs

| Service | Health | Metrics | API docs |
|---|---|---|---|
| Employee (Go) | `/api/v1/employee/health` | `/metrics` | `/swagger/index.html` |
| Attendance (Flask) | `/api/v1/attendance/health` | `/metrics` | `/apidocs/` |
| Salary (Spring Boot) | `/actuator/health` | Actuator + Micrometer Prometheus | `/salary-documentation` (Swagger UI), `/salary-api-docs` |
| Notification | (api mode, port 5000) | - | - |

**Health vs metrics vs logs** = observability ke 3 pillars (health/alerts, numeric metrics, text logs). Prometheus metrics scrape karta hai; Grafana dashboard; logs ke liye CloudWatch/ELK.

## 14.4 Security checklist (docs mein jo mila)

- DB/Redis **localhost par bind** (public expose nahi)
- SSH sirf **admin IP (`/32`)** se, `.pem` ka `chmod 400`
- Secrets **config files mein nahi** (Secrets Manager/Vault/env vars)
- Redis `requirepass`, `bind` restrict
- HTTPS (ALB + ACM ya Nginx + Let's Encrypt)
- Security Groups mein sirf zaroori ports
- Credentials Git mein commit nahi

## 14.5 Alag-alag languages ka toolchain (ek nazar mein)

| Service | Language | Dependency / Build | Run (native) |
|---|---|---|---|
| Employee | Go | Go modules, `make build` | `./employee-api` / `go run main.go` |
| Attendance | Python 3.11 | **Poetry** (+ pyenv) | `poetry run gunicorn app:app -b 0.0.0.0:8081` |
| Salary | Java 17 (Spring Boot) | **Maven** (`mvn clean package`, `mvnw`) | `java -jar ...jar` / `./mvnw spring-boot:run` |
| Notification | Python | `pip` + `requirements.txt` (venv) | `python3 notification_api.py --mode <mode>` |
| Frontend | React (Node 16.15.1) | **npm** (`npm install`, `npm run build`) | Nginx static / `npx serve -s build` |

## 14.6 Common troubleshooting flow (kisi bhi service ke liye)

```text
1. Process chal raha hai?            → ps / systemctl status / logs
2. Port sun raha hai?                → ss -ltnp | grep :<port>
3. Health endpoint?                  → curl .../health
4. Dependency (DB/Redis) up hai?     → psql / cqlsh / redis-cli ping
5. Config sahi hai?                  → host/port/credentials (Docker IP to nahi?)
6. Migration chali?                  → empty data / table missing
7. Network/Security Group?           → external access, firewall
8. Logs                              → tail -f ~/logs/*.log, journalctl -u <svc>
```

---

# 15. Quick Comparison Tables

## 15.1 Databases: PostgreSQL vs ScyllaDB vs Redis (🆕 Elasticsearch bhi)

| | **PostgreSQL** | **ScyllaDB** | **Redis** | **Elasticsearch** |
|---|---|---|---|---|
| Type | Relational (SQL) | Wide-column NoSQL (CQL) | In-memory key-value | Search/analytics engine |
| Data model | Tables, rows, joins | Partition + clustering key | Keys + data structures | JSON documents + index |
| Consistency | Strong (ACID) | Tunable (eventual to strong) | Single-node atomic; replicas async | Near real-time |
| Scaling | Vertical (+ replicas) | **Horizontal**, peer-to-peer | Cluster/sharding | Horizontal (shards) |
| Best for | Structured, relational, transactions | Huge write throughput, low latency, scale | Cache, sessions, counters | Full-text search |
| Port | 5432 | 9042 | 6379 | 9200 |
| OT-MS use | **Attendance** | **Employee, Salary** | **Salary cache** (optional others) | **Notification (POC)** |

## 15.2 SQL vs NoSQL: kab kaunsa?

| Chahiye | Choose |
|---|---|
| Joins, transactions, structured relations | SQL (PostgreSQL) |
| Massive scale, high write throughput, simple access patterns | NoSQL (ScyllaDB/Cassandra) |
| Ultra-fast reads, temporary data | Redis |
| Full-text search | Elasticsearch |

## 15.3 Run modes: Docker vs Native (is sprint ka theme)

| | Docker | Native (is sprint mein) |
|---|---|---|
| Setup | Image se, isolated | Har dependency manually install |
| Config | Container network IPs | `127.0.0.1` / localhost |
| Learning | Kam internals | **Zyada** (har component ko samajhna) |
| Use | Production/portability | Understanding, debugging, POC |

---
# 16. Master Interview Q&A (Sprint-01)

## 16.1 Project-level

**Q. Sprint-01 mein tumne kya kiya?**
> "OT-Microservices naam ke Employee Management System ko samjha aur chalaya. 5 services (Go, Python, Java, Python worker, React) aur 3 databases (PostgreSQL, ScyllaDB, Redis) ka documentation aur POC banaya. Mere tickets: Attendance API documentation (SCRUM-66), ScyllaDB documentation (SCRUM-78) aur SSL POC (SCRUM-114). Poora stack bina Docker ke Ubuntu/EC2 par manually chalaya, taaki har component ke internals samajh aayein."

**Q. Bina Docker ke kyun chalaya?**
Repo ki configs Docker IPs par thin. Native chalane se dependencies (Python version, DB, migration tool, ports) khud setup karni padti hain, isse troubleshooting aur architecture ki gehri samajh aati hai.

**Q. Sabse challenging cheez kaunsi thi?** *(apne experience se customize karo)*
Template: "Attendance API mein Python version mismatch (Ubuntu 24.04 par 3.12 vs app ko 3.11) aur Liquibase ka JDBC driver / 404 URL issue. pyenv se Python 3.11 aur official APT repo se Liquibase le ke solve kiya."

**Q. Documentation + POC dono kyun?**
DOC se concept, architecture, DR/HA, monitoring aur troubleshooting samjhaate hain; POC se wahi cheez hands-on verify hoti hai. Dono saath ho toh koi bhi naya member reproduce kar sakta hai.

## 16.2 Scenario-based

**S1. Attendance API `Connection refused 172.17.0.3:5432` de rahi hai.**
Config Docker IPs par point kar rahi hai. `config.yaml` aur `liquibase.properties` mein host `127.0.0.1` karo, aur PostgreSQL chal raha hai (`systemctl status postgresql`) verify karo.

**S2. Frontend par blank page aa raha hai.**
Backend APIs unreachable hongi. Employee/Attendance/Salary ke health endpoints `curl` se check, phir browser network tab aur logs.

**S3. Nginx 502 Bad Gateway de raha hai.**
Backend service down ya galat port. `curl http://localhost:8080/...` seedha check karo, `ss -ltnp` se port dekho, service logs padho.

**S4. Salary API par non-existent ID search karne par 500 aata hai.**
Known issue: single-row query mapping fail hoti hai jab partition nahi milti. Fix: repository mein empty result ko handle karke 404 return karo.

**S5. Salary API cache kaam nahi kar raha jaisa lagta hai.**
Redis TTL sirf 1 second hai (`entryTtl(Duration.ofSeconds(1))`). TTL business need ke hisaab se badhao; key `...salary-search-id::<id>` Redis mein `KEYS`/`TTL` se verify.

**S6. Redis replica par `SET` karne par error.**
`READONLY`, replica read-only hai. Writes master (6379) par karo.

**S7. Certbot "challenge failed" de raha hai.**
DNS A record sahi IP par hai? (`nslookup`), Security Group mein port 80/443 open hai? NGINX chal raha hai? Propagation ka wait karo.

**S8. Certificate expire hone wala hai.**
`sudo certbot certificates`, `systemctl status certbot.timer`, `sudo certbot renew --dry-run`; port 80 aur DNS check.

**S9. Ek hi machine par saare services start karne hain, kaunsa order?**
Pehle infra (scylla-server, postgresql, redis, nginx), phir backends (Employee, Attendance, Salary, Notification), phir frontend build + nginx restart. Pehle ports saaf (`fuser -k`).

**S10. ScyllaDB node `DN` (Down/Normal) dikha raha hai.**
`systemctl status scylla-server`, `journalctl -u scylla-server -n 100`, disk (`df -h`), memory, ports (7000/9042), `scylla.yaml` (seeds, cluster name).

**S11. Notification worker `KeyError: 'email_id'`.**
Elasticsearch documents mein `email_id` field nahi. `_search` se data dekho aur seed data sahi karo.

**S12. Java app JDK 21 par build fail (Lombok error).**
`mvn clean package -DskipTests -Dlombok.version=1.18.32` (pom.xml edit kiye bina override).

## 16.3 Cross-topic / design

**Q. "Database per service" kyun?** Loose coupling, independent scaling/deploy, fault isolation, har kaam ke liye sahi database (SQL vs NoSQL).

**Q. Attendance PostgreSQL par aur Employee/Salary ScyllaDB par kyun?** Attendance structured aur relational queries (filter/group/date) ke liye SQL accha. Employee/Salary high-throughput, scalable, simple access patterns ke liye ScyllaDB.

**Q. Caching kahan lagaoge aur kaise?** Read-heavy lookups par (Salary search by ID). Redis read-through/cache-aside, sahi TTL, invalidation strategy, aur eviction policy (`allkeys-lru`).

**Q. Is architecture ko production-grade kaise banaoge?** Docker/Kubernetes, managed DBs (RDS Multi-AZ, ElastiCache, multi-node Scylla), ALB + HTTPS (ACM/Let's Encrypt), Auto Scaling, secrets manager, CI/CD, centralized logging + Prometheus/Grafana monitoring, backups + DR.

**Q. Reverse proxy ke fayde?** Single entry point, routing, TLS termination, static file serving, buffering, load balancing, backend hide.

**Q. Health, readiness, liveness mein farak?** Health = app zinda hai. Readiness = dependencies ke saath traffic lene layak. Liveness = restart karna chahiye ya nahi (Kubernetes concepts).

**Q. Migrations ko CI/CD mein kaise chalaoge?** Pipeline step ya init container, app deploy se pehle, backward-compatible changes, backup ke baad.

**Q. Polyglot microservices ke fayde aur nuksan?** Fayde: har kaam ke liye best language/tool. Nuksan: alag toolchains, alag deployment/skills, zyada operational complexity.

**Q. HA kaise sochte ho har component ke liye?** API: multiple instances + LB. PostgreSQL: replication + RDS Multi-AZ. ScyllaDB: multi-node, RF, multi-AZ/DC. Redis: replica + Sentinel/Cluster. Nginx: multiple nodes / managed ALB.

## 16.4 ⚡ Rapid-fire

| Sawal | Jawab |
|---|---|
| OT-Microservices services? | Frontend (React), Employee (Go), Attendance (Python), Salary (Java), Notification (Python) |
| Employee API DB? | ScyllaDB (`employee_db`) |
| Attendance API DB? | PostgreSQL (`attendance_db`) |
| Salary API DB + cache? | ScyllaDB/Cassandra + Redis |
| Attendance ka Python version? | 3.11.13 (pyenv) |
| Attendance ka migration tool? | Liquibase |
| Employee/Salary ka migration tool? | migrate (golang-migrate) |
| Attendance server? | Gunicorn (`app:app`) |
| Attendance health URL? | `/api/v1/attendance/health` |
| Employee health URL? | `/api/v1/employee/health` |
| Salary health URL? | `/actuator/health` |
| Swagger URLs? | Employee `/swagger/index.html`, Attendance `/apidocs/`, Salary `/swagger-ui/index.html` ya `/salary-documentation` |
| Metrics endpoint? | `/metrics` (Employee, Attendance), Actuator/Prometheus (Salary) |
| Salary cache key? | `...salary-search-id::<id>` |
| Salary cache TTL (repo)? | 1 second |
| Notification test SMTP? | MailHog (1025 / UI 8025) |
| Notification endpoint (api mode)? | `POST /notification/send` (port 5000) |
| Frontend Node version? | 16.15.1 |
| Frontend production server? | Nginx (port 80), ya `npx serve -s build` |
| PostgreSQL port / user? | 5432 / postgres |
| ScyllaDB ports? | 9042 CQL, 7000 internode, 9142 TLS CQL, 10000 REST |
| `nodetool status` `UN`? | Up + Normal |
| Scylla config file? | `/etc/scylla/scylla.yaml` |
| Redis default port? | 6379 (replica POC: 6380) |
| Redis persistence? | RDB + AOF |
| Redis HA tools? | Replication, Sentinel, Cluster |
| Let's Encrypt validity? | 90 din |
| Certbot command? | `sudo certbot --nginx -d domain -d www.domain` |
| Renewal test? | `sudo certbot renew --dry-run` |
| HTTPS port? | 443 |
| DNS record domain→IP? | A record |

---

# 17. Last-Minute Revision Cheat Sheet

## 17.1 Architecture ek line mein
```text
User → React → NGINX → Employee(Go,8080)/Attendance(Py,8081)/Salary(Java,8082)
       Employee + Salary → ScyllaDB(9042)   Attendance → PostgreSQL(5432)   Salary → Redis(6379)
       Notification Worker (alag) → data leke → Email (SMTP/MailHog)
```

## 17.2 Commands

```bash
# ---- Services / ports ----
sudo systemctl start|stop|status scylla-server postgresql redis nginx
sudo ss -ltnp | grep :<port>        ;  fuser -k <port>/tcp
tail -f ~/logs/*.log

# ---- Health ----
curl -s http://localhost:8080/api/v1/employee/health
curl -s http://localhost:8081/api/v1/attendance/health | jq .
curl -s http://localhost:8082/actuator/health

# ---- Attendance (Python/Poetry/Liquibase) ----
pyenv install 3.11.13 && pyenv global 3.11.13
poetry env use 3.11.13 && poetry install --no-root && poetry add gunicorn
make run-migrations
poetry run gunicorn app:app --log-config log.conf -b 0.0.0.0:8080

# ---- Employee (Go) ----
make build ; export GIN_MODE=release ; ./employee-api ; make swagger

# ---- Salary (Java) ----
mvn clean package -DskipTests "-Dlombok.version=1.18.32"
java -jar target/salary-0.1.0-RELEASE.jar

# ---- Notification ----
export CONFIG_FILE="$(pwd)/config.yaml"
python3 notification_api.py --mode external     # | scheduled | api
docker run -d -p 1025:1025 -p 8025:8025 --name mailhog mailhog/mailhog

# ---- Frontend ----
npm install && npm run build
nohup npx serve -s build -l 3000 > ~/logs/frontend.log 2>&1 &
sudo nginx -t && sudo systemctl reload nginx

# ---- PostgreSQL ----
sudo -u postgres psql          psql -U postgres -h 127.0.0.1 -d attendance_db
\l  \c  \dt  \d <table>  \q

# ---- ScyllaDB ----
sudo scylla_setup ; nodetool status ; cqlsh ; DESCRIBE KEYSPACES;
journalctl -u scylla-server -f

# ---- Redis ----
redis-cli ping ; SET k v ; GET k ; SETEX k 60 v ; TTL k ; DEL k
redis-cli -p 6379 INFO replication ; redis-cli --bigkeys ; redis-cli SLOWLOG GET 10

# ---- SSL ----
nslookup <domain>
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d <domain> -d www.<domain>
sudo certbot certificates ; sudo systemctl status certbot.timer ; sudo certbot renew --dry-run
echo | openssl s_client -connect <domain>:443 -servername <domain> 2>/dev/null | openssl x509 -noout -subject -issuer -dates
```

## 17.3 Top 12 baatein (interview se pehle ek baar padho)

1. OT-MS = **polyglot microservices**, har service ka **apna database**, **REST**, Nginx single entry point.
2. **Employee + Salary → ScyllaDB**, **Attendance → PostgreSQL**, **Salary → Redis** (read-through cache).
3. Repo ki configs ke Docker IPs (`172.17.0.x`) native run mein **`127.0.0.1`** karne pade.
4. Attendance: **pyenv** (3.11 vs system 3.12) + **Poetry** + **Liquibase** + **Gunicorn**.
5. Liquibase = `DATABASECHANGELOG` tracking; migrate = `schema_migrations`.
6. Salary POC ne **5 repo issues** dhunde (Lombok/JDK21, 1s TTL, PK mismatch, UUID/String, 404→500).
7. ScyllaDB: **shard-per-core**, peer-to-peer, CQL 9042, `UN` = healthy, `scylla_setup` (NTP, IO tuning).
8. Cassandra/Scylla: **partition key** (kahan), **clustering key** (sorting), **RF** (copies), **consistency level**.
9. Redis: RDB + AOF, replica **READONLY**, Sentinel (failover) vs Cluster (sharding), `allkeys-lru`.
10. Frontend: Nginx reverse proxy (`proxy_pass 3000`), systemd service, Node **16.15.1**, blank page = backend down.
11. Start order: **infra → backends → frontend**; 502 = backend down.
12. SSL: DNS A record → Certbot (`--nginx`) → HTTP→HTTPS redirect → `certbot.timer` + `--dry-run`; **90 din** validity; port **80 + 443**.

---

# 18. Gaps / Inconsistencies (Honest Notes)

Docs ko dhyan se padhne par ye cheezein mili. Interview mein "maine ye notice kiya" bolna strong lagta hai:

| # | Observation | Detail |
|---|---|---|
| 1 | **Ports docs mein alag-alag** | Employee API: DOC mein `8081`, Run OTMS/Frontend mein `8080`. Attendance: standalone POC mein `8080`, Run OTMS mein `8081`. Salary: POC `8080`, DOC/Run OTMS `8082` |
| 2 | Salary DOC mein inbound/outbound labels ulte | `9042` ko "Inbound" aur `8082` ko "Outbound" likha hai, jabki 8082 Tomcat ka inbound port hai |
| 3 | Salary POC mein **Cassandra 3.11**, DOC mein **ScyllaDB** | Container ka naam `salary-scylla` par image `cassandra:3.11`; app Cassandra-compatible hai |
| 4 | Notification ka data source | POC mein **Elasticsearch** (`employee-management` index), jabki Full Stack DOC kehta hai Notification Employee/Salary/Attendance **APIs** se data leta hai |
| 5 | ScyllaDB version/repo mismatch | SCRUM-77 mein `v2026.3.0` (web installer), SCRUM-78 mein repo list `scylladb-2026.1` |
| 6 | Notification POC `--break-system-packages` | Notification DOC mein venv hai; POC mein system-wide pip |
| 7 | Frontend: do tareeke | DOC: `npm run build` + `serve` (port 3000); POC: `npm start` + Nginx proxy + systemd |
| 8 | `Employee POC` ke kuch sections khali | Config/migration/API verification ke commands output ke bina (cat) ya khali code blocks |
| 9 | PostgreSQL DOC ka "Architecture" section khali | Heading hai, diagram nahi |
| 10 | Redis DOC mein placeholders | Kuch template lines (`Mention the Run-time dependency...`, `<L2 Reviewer>`) reh gayi hain |
| 11 | Salary POC: cloud deployment nahi hua | Doc khud kehta hai requested cloud platform verify nahi ho paaya |
| 12 | Duplicate branch | `Scrum-78` aur `SCRUM---78` ka content identical |
| 13 | SCRUM-113 (SSL DOC) aur DNS tickets | Zip mein nahi the, isliye §13.9-13.10 general concept hain |
| 14 | `main` branch | Sirf `# Sprint-01` title, koi content nahi |
| 15 | Screenshots / diagrams | Docs mein architecture diagrams aur screenshots images the; is guide mein text/ASCII diagrams hain |

---

<p align="center">

**📘 Sprint-01 • OT-Microservices • PostgreSQL • ScyllaDB • Redis • SSL/TLS • Interview Ready**

</p>
