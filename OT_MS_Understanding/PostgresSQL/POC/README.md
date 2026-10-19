<h1 align="center">PostgreSQL Setup & Run for POC</h1>

<p align="center">
  <img src="https://www.postgresql.org/media/img/about/press/elephant.png" width="120"/>
</p>

<p align="center">
  <a href="https://www.postgresql.org/docs/">
    <img src="https://img.shields.io/badge/Database-PostgreSQL-blue?style=for-the-badge" />
  </a>
  <a href="https://docs.liquibase.com/">
    <img src="https://img.shields.io/badge/Tool-Liquibase-orange?style=for-the-badge" />
  </a>
  <a href="https://github.com/OT-MICROSERVICES/attendance-api">
    <img src="https://img.shields.io/badge/Service-AttendanceAPI-green?style=for-the-badge" />
  </a>
</p>

| Author       | Created on | Version | Last updated by | Last edited on | Pre Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer  |
| ------------ | ---------- | ------- | --------------- | -------------- | ------------ | ----------- | ----------- | ------------ |
| Maqbool Alam| 07/09/2026 | 1.1     | Maqbool Alam   | 15/09/2026     | Team         | Rajnish | Pritam/Komal | Abhishek/Manish Nautiyal |

---

## Table of Contents

<details>
<summary>Table of Contents</summary>

* [Introduction](#introduction)
* [What is PostgreSQL](#what-is-postgresql)
* [Why PostgreSQL](#why-postgresql)
* [Architecture Context](#architecture-context)
* [Installation Steps](#installation-steps)
* [Database Setup](#database-setup)
* [Verification](#verification)
* [Troubleshooting](#troubleshooting)
* [Summary](#summary)

</details>

---

## Introduction

PostgreSQL is a powerful open-source relational database management system. It stores data in tables and uses SQL to manage and query data. It provides features such as ACID transactions, data integrity, concurrency, scalability, and extensibility, making it suitable for both small and large applications.

## What is PostgreSQL

PostgreSQL is an open-source relational database management system (RDBMS).

* Supports SQL queries (SELECT, JOIN, GROUP BY)
* ACID compliant
* Highly reliable and production-ready

> For Detail Documentation Follow [Link](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-76-SARTHAK/Documentation/OT_MS_Understanding/PostgresSQL/DOC/README.md)

---

## Why PostgreSQL

| Reason          | Explanation                                      |
| --------------- | ------------------------------------------------ |
| Structured Data | Attendance has tabular format (ID, status, date) |
| SQL Queries     | Easy filtering using WHERE, GROUP BY             |
| Reliability     | Strong consistency guarantees                    |

---

## Architecture Context
><img width="auto" height="500" alt="ChatGPT Image Apr 26, 2026, 11_20_27 PM" src="https://github.com/user-attachments/assets/98192cd0-1f38-4f11-ab44-37c29eb27361" />

As per project structure, PostgreSQL is used only by attendance-api.

---
## Installation Steps

### Step 1 — Update System

```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2 — Install PostgreSQL

```bash
sudo apt install postgresql postgresql-contrib -y
```

### Step 3 — Start Service

```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

### Step 4 — Verify

```bash
sudo systemctl status postgresql@16-main.service
```
<img width="1193" height="348" alt="image" src="https://github.com/user-attachments/assets/fbe030b4-7c79-46bb-b2db-f5189b28a167" />


---

## Database Setup

### Access PostgreSQL

```bash
sudo -u postgres psql
```

### Create Database

```sql
CREATE DATABASE attendance_db;
```

```sql
\l
```

Expected Database:

* attendance_db

### Set Password

```sql
ALTER USER postgres PASSWORD 'password';
```

### Exit

```bash
\q
```

---

## Verification

### Connect Database

```bash
psql -U postgres -h 127.0.0.1 -d attendance_db
```

### Create Table

```sql
CREATE TABLE records (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    status VARCHAR(20) NOT NULL,
    date DATE NOT NULL
);
```

### List Tables

```sql
\dt
```

Expected tables:

* records


### Check Coloumns

```sql
\d records
```

Expected Column:
```
 Column  |          Type
---------+-----------------------
 id      | integer
 name    | character varying(100)
 status  | character varying(20)
 date    | date
```

### Insert Data
```sql
INSERT INTO records (id, name, status, date)
VALUES
(101, 'Rahul Kumar', 'Present', '2026-09-11');
```

### Fetch Data
```sql
SELECT id, name, status, date
FROM records
ORDER BY id DESC;
```

Expected Coloumns:

```
 id  |     name      | status  |    date
-----+---------------+---------+------------
 101 | Rahul Kumar   | Present | 2026-09-11
```

### Fetch By id

```sql
SELECT id, name, status, date
FROM records
WHERE id = 101;
```

### Update Record

```sql
UPDATE records
SET status = 'Absent'
WHERE id = 101;
```
### Delete Record

```
DELETE FROM records
WHERE id = 101;
```

### Drop Table 

```
DROP TABLE records;
```

### Truncate Table
```
TRUNCATE TABLE records;
```

<img width="1091" height="926" alt="image" src="https://github.com/user-attachments/assets/b4ab52dc-7cc3-439f-87e7-d822a6b8968f" />

<img width="1056" height="789" alt="image" src="https://github.com/user-attachments/assets/ca43c429-cd6f-4196-ba4a-6dc95048dc56" />

---

## Troubleshooting

### PostgreSQL not starting

```bash
sudo systemctl restart postgresql
```

### Connection refused

* Check port 5432
* Verify service is running

### Authentication error

```bash
psql -U postgres -h 127.0.0.1
```

(Use TCP instead of socket)

---

## Summary

* PostgreSQL is the source of truth for attendance data
* Fully integrated with attendance-api

---

## FAQs

**Q1. Why use PostgreSQL instead of NoSQL here?**

> Attendance data is structured and benefits from SQL queries like filtering, grouping, and date-based operations.

**Q2. Why use 127.0.0.1 instead of Docker IP (172.x)?**

> For local POC setup, services run on the same host, so localhost ensures stable connectivity.

**Q3. How to verify PostgreSQL is working correctly?**

> Use `psql` to connect and run `\dt` to check tables or test API endpoints using curl.

---

## Contact Information

| Name         | Email                                                                             | 
| ------------ | --------------------------------------------------------------------------------- |
| Maqbool Alam| [maqbul.alam.snaatak@mygurukulam.co](mailto:maqbul.alam.snaatak@mygurukulam.co) |

---

## References

| Resource                 | Link                                                                       | Description                                           |
| ------------------------ | -------------------------------------------------------------------------- | ----------------------------------------------------- |
| PostgreSQL Official Docs | [https://www.postgresql.org/docs/](https://www.postgresql.org/docs/)       | Official PostgreSQL documentation                     |
| OT Microservices Repo    | [https://github.com/OT-MICROSERVICES](https://github.com/OT-MICROSERVICES) | Source code for microservices project                 |

---
