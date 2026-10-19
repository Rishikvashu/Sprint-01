# Salary API Microservice — Local Setup & POC

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| Anurag Maurya | 11-09-2026 | v1 | Anurag Maurya | 11-09-2026 | Divya M | Aayush Verma | Mahesh Kumar/Varun |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Salary API Overview](#2-salary-api-overview)
3. [Architecture](#3-architecture)
4. [Prerequisites](#4-prerequisites)
5. [Dependency Setup](#5-dependency-setup)
6. [Database Setup](#6-database-setup)
7. [Application Configuration](#7-application-configuration)
8. [Build and Run](#8-build-and-run)
9. [API Endpoints](#9-api-endpoints)
10. [POC Validation](#10-poc-validation)
11. [Health Check](#11-health-check)
12. [Redis Caching](#12-redis-caching)
13. [Known Repository Issues](#13-known-repository-issues)
14. [Conclusion](#14-conclusion)
15. [Contact Information](#15-contact-information)
16. [References](#16-references)

---

# 1. Introduction

<div align="justify">

The **Salary API** is a Java Spring Boot microservice in the OT-Microservices repository stack. It manages employee salary records, process dates, and transaction statuses.

This Proof of Concept (POC), for Jira ticket **SCRUM-69**, verified the local setup end to end: dependencies, database keyspace and table creation, schema migration, application build and startup, REST API operations, Spring Actuator health checks, Apache Cassandra 3.11 data persistence, and Redis caching. Cloud deployment has not been completed because the requested cloud platform could not be verified from the available project information.

</div>

---

# 2. Salary API Overview

<div align="justify">

- **Framework:** Spring Boot 3.1.1 with embedded Apache Tomcat.
- **Primary database:** Apache Cassandra 3.11 (keyspace employee_db, table employee_salary).
- **Cache:** Redis, used for read-through lookup caching.
- **Application port:** 8080.
- **Observability:** Spring Boot Actuator and Micrometer Prometheus metrics.
- **API specification:** Springdoc OpenAPI 3.0 and Swagger UI.

*Note: The application uses Cassandra-compatible storage. This local POC used Apache Cassandra 3.11.*

</div>

---

# 3. Architecture

<div align="justify">

### 3.1 Standard Request Flow

An HTTP request passes through the standard Spring Boot layers down to the database.

<details>
<summary> <b>View Diagram: Salary API Request Architecture</b></summary>
<br>

![Salary API Request Architecture](assets/standard%20flow.png)

*Standard request flow from the HTTP client through the Spring Boot layers to the database.*
</details>

### 3.2 Read-Through Redis Caching Flow

Searches by employee ID (GET /api/v1/salary/search?id=&lt;id&gt;) use Redis to speed up reads.

<details>
<summary> <b>View Diagram: Salary API Cache Architecture</b></summary>
<br>

![Salary API Cache Architecture](assets/cache-flow.png)

*Read-through Redis caching flow for the search by ID endpoint.*
</details>

```
Client / GET /salary/search?id=<id>
        ↓
Controller
        ↓
Salary Service
        ↓
Redis Cache (Key: salary-search-id::<id>)
   ├── CACHE HIT  ──> Return cached JSON response
   └── CACHE MISS ──> Query Apache Cassandra 3.11 ──> Store result in Redis (TTL = 1s) ──> Return JSON response
```

</div>

---

# 4. Prerequisites

<div align="justify">

The POC was verified in this environment:

| Component | Verified Local Environment |
| :--- | :--- |
| **Operating System** | Windows 11 Enterprise (WSL2 Ubuntu 24.04 LTS) |
| **Java Development Kit** | OpenJDK 21.0.12 |
| **Build Tool** | Apache Maven 3.8.7 |
| **Container Engine** | Docker Desktop v29.5.3 |
| **Database Engine** | Apache Cassandra 3.11 Docker Container |
| **Cache Engine** | Redis 7.2-alpine Docker Container |

*Note: The source code targets Java 17, but the local build compiled and ran on OpenJDK 21 using a Lombok version override during the Maven build.*

<details>
<summary> <b>View Screenshot: Environment Version Verification</b></summary>
<br>

![Environment Version Verification](assets/01-environment-versions.png)

*Terminal output of the java, mvn, and docker commands showing the verified tool versions.*
</details>

</div>

---

# 5. Dependency Setup

<div align="justify">

A dedicated Docker bridge network (salary-net) keeps the POC separate from other local containers.

### 5.1 Docker Network

```bash
docker network create salary-net
```

<details>
<summary> <b>View Screenshot: Docker Network</b></summary>
<br>

![Docker Network](assets/02-docker-network.png)

*Creation of the salary-net Docker network with the bridge driver.*
</details>

### 5.2 Redis Container

```bash
docker run -d --name salary-redis --network salary-net -p 6379:6379 redis:7.2-alpine redis-server --requirepass password
```

<details>
<summary> <b>View Screenshot: Redis Container</b></summary>
<br>

![Redis Container](assets/03-redis-container.png)

*The salary-redis container running with port 6379 mapped.*
</details>

### 5.3 Cassandra Container

```bash
docker run -d --name salary-scylla --network salary-net -p 9042:9042 cassandra:3.11
```

*Note: Apache Cassandra 3.11 was used because it provides the full CQL protocol compatibility the application driver needs.*

<details>
<summary> <b>View Screenshot: Cassandra Container</b></summary>
<br>

![Cassandra Container](assets/04-cassandra-container.png)

*The salary-scylla container running with port 9042 mapped.*
</details>

</div>

---

# 6. Database Setup

<div align="justify">

### 6.1 Keyspace Creation

Connect to the container with cqlsh and create the employee_db keyspace:

```sql
CREATE KEYSPACE IF NOT EXISTS employee_db WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
```

<details>
<summary> <b>View Screenshot: Keyspace Creation</b></summary>
<br>

![Keyspace Creation](assets/05-keyspace-created.png)

*The employee_db keyspace created and verified in Apache Cassandra 3.11.*
</details>

### 6.2 Schema Definition & Migration

The schema is defined in migration/000001_create_employee_salary_table.up.sql:

```sql
CREATE TABLE IF NOT EXISTS employee_salary (
    id text,
    process_date text,
    name text,
    salary float,
    status text,
    PRIMARY KEY (id, process_date)
) WITH CLUSTERING ORDER BY (process_date DESC);
```

Apply the migration:

```bash
cat migration/000001_create_employee_salary_table.up.sql | docker exec -i salary-scylla cqlsh -k employee_db
```

<details>
<summary> <b>View Screenshot: Database Migration</b></summary>
<br>

![Database Migration](assets/06-database-migration.png)

*The migration running successfully against the employee_db keyspace.*
</details>

Verify that the table exists:

```bash
docker exec salary-scylla cqlsh -e "USE employee_db; DESCRIBE TABLE employee_salary;"
```

<details>
<summary> <b>View Screenshot: Employee Salary Table Schema</b></summary>
<br>

![Employee Salary Table Schema](assets/07-table-schema.png)

*DESCRIBE output showing the columns and composite primary key of employee_salary.*
</details>

</div>

---

# 7. Application Configuration

<div align="justify">

The original configuration in src/main/resources/application.yml and migration.json used hardcoded Docker bridge IPs (172.17.0.3 and 172.17.0.4). For this local run they were changed to 127.0.0.1.

### 7.1 application.yml

```yaml
spring:
  cassandra:
    keyspace-name: employee_db
    contact-points: 127.0.0.1
    port: 9042
    username: scylladb
    password: password
    local-datacenter: datacenter1
  data:
    redis:
      host: 127.0.0.1
      port: 6379
      password: password
```

### 7.2 migration.json

```json
{
  "database": "cassandra://127.0.0.1:9042/employee_db?username=scylladb&password=password"
}
```

*Note: These changes are for local execution only and are not committed.*

<details>
<summary> <b>View Screenshot: Application Configuration</b></summary>
<br>

![Application Configuration](assets/08-application-config.png)

*Editor view showing the local targets (127.0.0.1:9042 and 127.0.0.1:6379) in application.yml and migration.json. Mask any sensitive password values before adding the screenshot.*
</details>

</div>

---

# 8. Build and Run

<div align="justify">

### 8.1 Build Command

With OpenJDK 21, the default Lombok 1.18.28 in Spring Boot 3.1.1 fails with a javac error (JCTree$JCImport qualid). To build without editing pom.xml, pass a Lombok version override on the command line:

```bash
mvn clean package -DskipTests "-Dlombok.version=1.18.32"
```

**Result:** BUILD SUCCESS. The generated JAR is target/salary-0.1.0-RELEASE.jar (about 49.8 MB).

<details>
<summary> <b>View Screenshot: Maven Build Success</b></summary>
<br>

![Maven Build Success](assets/09-maven-build-success.png)

*Maven build finishing with BUILD SUCCESS and producing salary-0.1.0-RELEASE.jar.*
</details>

### 8.2 Run Application

```bash
java -jar target/salary-0.1.0-RELEASE.jar
```

Startup logs:
```
Tomcat started on port(s): 8080 (http) with context path ''
Started SalaryApplication in 30.216 seconds
```

<details>
<summary> <b>View Screenshot: Application Startup</b></summary>
<br>

![Application Startup](assets/10-application-startup.png)

*Startup logs showing Tomcat on port 8080 and SalaryApplication started successfully.*
</details>

</div>

---

# 9. API Endpoints

<div align="justify">

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| **POST** | /api/v1/salary/create/record | Creates a new employee salary record |
| **GET** | /api/v1/salary/search?id=&lt;id&gt; | Finds a salary record by employee ID (uses Redis cache) |
| **GET** | /api/v1/salary/search/all | Returns all salary records |
| **GET** | /actuator/health | Application health and dependency check |
| **GET** | /salary-documentation | Redirects to the Swagger UI |
| **GET** | /salary-api-docs | OpenAPI 3.0 JSON specification |

<details>
<summary> <b>View Screenshot: Swagger UI</b></summary>
<br>

![Swagger UI](assets/11-swagger-ui.png)

*Swagger UI at http://localhost:8080/salary-documentation listing the Salary API routes.*
</details>

</div>

---

# 10. POC Validation

<div align="justify">

### 10.1 Create Record (POST)

Request:
```bash
curl -i -X POST http://localhost:8080/api/v1/salary/create/record \
  -H "Content-Type: application/json" \
  -d '{
    "id": "EMP-POC-9001",
    "name": "Jane Doe",
    "salary": 85000.5,
    "processDate": "2026-09-15",
    "status": "PROCESSED"
  }'
```

Response:
```http
HTTP/1.1 201 Created
Content-Type: application/json

{"id":"EMP-POC-9001","name":"Jane Doe","salary":85000.5,"processDate":"2026-09-15","status":"PROCESSED"}
```

<details>
<summary> <b>View Screenshot: Create Salary Record — POST 201</b></summary>
<br>

![Create Salary Record](assets/12-create-record.png)

*HTTP 201 Created response confirming record EMP-POC-9001 was created.*
</details>

### 10.2 Search Record by ID (GET)

Request:
```bash
curl -i "http://localhost:8080/api/v1/salary/search?id=EMP-POC-9001"
```

Response:
```http
HTTP/1.1 200 OK
Content-Type: application/json

{"id":"EMP-POC-9001","name":"Jane Doe","salary":85000.5,"processDate":"2026-09-15","status":"PROCESSED"}
```

<details>
<summary> <b>View Screenshot: Search Salary Record by ID — GET 200</b></summary>
<br>

![Search Salary Record by ID](assets/13-search-by-id.png)

*HTTP 200 OK response returning the record for employee ID EMP-POC-9001.*
</details>

### 10.3 Search All Records (GET)

Request:
```bash
curl -i "http://localhost:8080/api/v1/salary/search/all"
```

Response:
```http
HTTP/1.1 200 OK
Content-Type: application/json

[{"id":"EMP-POC-9001","name":"Jane Doe","salary":85000.5,"processDate":"2026-09-15","status":"PROCESSED"}]
```

<details>
<summary> <b>View Screenshot: Search All Salary Records — GET 200</b></summary>
<br>

![Search All Salary Records](assets/14-search-all.png)

*HTTP 200 OK response returning all stored salary records.*
</details>

### 10.4 Direct Database Query

Confirm the record was written to Cassandra:

```bash
docker exec salary-scylla cqlsh -e "SELECT * FROM employee_db.employee_salary;"
```

Output:
```
 id           | process_date | name     | salary  | status
--------------+--------------+----------+---------+-----------
 EMP-POC-9001 |   2026-09-15 | Jane Doe | 85000.5 | PROCESSED

(1 rows)
```

<details>
<summary> <b>View Screenshot: Direct Cassandra Database Verification</b></summary>
<br>

![Direct Cassandra Database Verification](assets/15-database-record.png)

*CQL query output confirming record EMP-POC-9001 is stored in employee_db.employee_salary.*
</details>

</div>

---

# 11. Health Check

<div align="justify">

Request:
```bash
curl -s http://localhost:8080/actuator/health
```

Response:
```json
{
  "status": "UP",
  "components": {
    "cassandra": {
      "status": "UP",
      "details": { "version": { "major": 3, "minor": 11, "patch": 19 } }
    },
    "diskSpace": { "status": "UP" },
    "ping": { "status": "UP" },
    "redis": {
      "status": "UP",
      "details": { "version": "7.2.16" }
    }
  }
}
```

<details>
<summary> <b>View Screenshot: Application Health Check</b></summary>
<br>

![Application Health Check](assets/16-health-check.png)

*Actuator health response showing UP for Apache Cassandra 3.11 and Redis 7.2.*
</details>

</div>

---

# 12. Redis Caching

<div align="justify">

Searches through GET /api/v1/salary/search?id=&lt;id&gt; are annotated with @Cacheable("salary-search-id"). On a search, Spring Data Redis creates a cache entry in this format:

```
com.opstree.microservice.salary.salary-search-id::<id>
```

Checking the Redis keys right after a search:

```bash
docker exec salary-redis redis-cli -a password KEYS "*"
```

Output:
```
com.opstree.microservice.salary.salary-search-id::EMP-POC-9001
```

<details>
<summary> <b>View Screenshot: Redis Cache Key Verification</b></summary>
<br>

![Redis Cache Key Verification](assets/17-redis-cache-key.png)

*Redis key com.opstree.microservice.salary.salary-search-id::EMP-POC-9001 created after the search.*
</details>

*Note: The cache TTL in SalaryApplication.java is currently **1 second** (entryTtl(Duration.ofSeconds(1))), after which the key expires.*

<details>
<summary> <b>View Screenshot: Redis Cache TTL</b></summary>
<br>

![Redis Cache TTL](assets/18-redis-ttl.png)

*Redis CLI TTL output confirming the 1-second expiry.*
</details>

</div>

---

# 13. Known Repository Issues

<div align="justify">

The POC found these repository-level issues:

1. **Lombok and JDK 21 incompatibility:** building on JDK 21 needs -Dlombok.version=1.18.32 on the Maven command line.
2. **Short cache TTL:** the Redis TTL is hardcoded to 1 second in SalaryApplication.java, which is too short for useful caching.
3. **Primary key mismatch:** the migration defines a composite key (id, process_date), but the Employee.java entity marks only id with @PrimaryKey.
4. **Repository type mismatch:** EmployeeRepository extends CassandraRepository with UUID as the ID type, while Employee.id is a String.
5. **Non-existent ID returns 500:** searching for an ID that does not exist returns HTTP 500 instead of a clean HTTP 404, because the single-row query mapping fails when no partition matches.

<details>
<summary> <b>View Screenshot: Non-Existent Employee ID — HTTP 500 (Known Repository Issue / Negative Test)</b></summary>
<br>

![Non-Existent Employee ID](assets/19-negative-test-500.png)

*HTTP 500 error when searching for the non-existent ID EMP-NON-EXISTENT.*
</details>

</div>

---

# 14. Conclusion

<div align="justify">

The Salary API microservice was built and started locally for Jira ticket **SCRUM-69**. Apache Cassandra 3.11 and Redis were configured and verified, the schema migration was applied, and the core REST flows (create, search, search/all) were validated end to end. Data persistence in Cassandra and read caching in Redis were also confirmed. Cloud deployment has not been completed because the requested cloud platform could not be verified from the available project information.

The POC also found a few mapping and configuration issues that can be fixed in future maintenance tasks.

</div>

---

# 15. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Anurag Maurya | [anurag.maurya.snaatak@mygurukulam.co](mailto:anurag.maurya.snaatak@mygurukulam.co) |

---

# 16. References

| Resource | Link |
| :--- | :--- |
| Spring Boot 3.1 Documentation | https://docs.spring.io/spring-boot/docs/3.1.1/reference/html/ |
| Spring Data Cassandra | https://docs.spring.io/spring-data/cassandra/docs/current/reference/html/ |
| Apache Cassandra Documentation | https://cassandra.apache.org/doc/latest/ |
| Redis Documentation | https://redis.io/docs/ |
| Springdoc OpenAPI | https://springdoc.org/ |
