# Employee API – Application Documentation

---

# Author Table

| **Author**    | **Created On** | **Version** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ------------- | -------------- | ----------- | ------------------ | --------------- | --------------- | --------------- |
| Anshul Kotiya | 13-09-2026     | v1.0        | 13-09-2026         | Shubham Rathi / Sunny   | Shreya/Nikita   | Piyush Upadhyay |

---

# Table of Contents

1. [Purpose](#1-purpose)
2. [Pre-requisites](#2-pre-requisites)
3. [System Requirements](#3-system-requirements)
4. [Dependencies](#4-dependencies)

   * [4.1 Build Time Dependency](#41-build-time-dependency)
   * [4.2 Run Time Dependency](#42-run-time-dependency)
5. [Important Ports](#5-important-ports)
6. [Architecture](#6-architecture)

   * [6.1 Architecture Diagram](#61-architecture-diagram)
   * [6.2 Architecture Components](#62-architecture-components)


7. [Monitoring](#7-monitoring)
8. [Disaster Recovery](#8-disaster-recovery)
9. [High Availability](#9-high-availability)
10. [Troubleshooting](#10-troubleshooting)
11. [FAQs](#11-faqs)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

# 1. Purpose

The purpose of this document is to provide an overview of the Employee API and explain its setup, architecture, dependencies, configuration, monitoring, and troubleshooting.

---

# 2. Pre-requisites

Before setting up the Employee API, ensure that the following requirements are available.

| **Requirement**          | **Purpose**                                                  |
| ------------------------ | ------------------------------------------------------------ |
| **Linux / Supported OS** | Provides the environment to build and run the application    |
| **Go**                   | Required to build and run the Employee API                   |
| **Git**                  | Required to clone and manage the source code                 |
| **Make**                 | Required to run project-defined build and migration commands |
| **ScyllaDB**             | Primary database used to store employee information          |
| **curl**                 | Used for API and health endpoint validation                  |

> **Note:** Redis is optional and is required only when caching is enabled.

---

# 3. System Requirements

| **Hardware Specification** | **Minimum Recommendation**   |
| -------------------------- | ---------------------------- |
| **Processor**              | Dual-core                    |
| **RAM**                    | 2 GB                         |
| **Disk**                   | 5 GB                         |
| **OS**                     | Linux-based operating system |

> **Note:** Actual resource requirements may vary depending on API traffic and database workload.

---

# 4. Dependencies

## 4.1 Build Time Dependency

| **Name**       | **Version / Requirement** | **Description**                                |
| -------------- | ------------------------- | ---------------------------------------------- |
| **Go**         | 1.20 or later             | Used to build the Employee API                 |
| **Go Modules** | Built into Go             | Manages application libraries and dependencies |

Application libraries are managed through the project's `go.mod` file.

## 4.2 Run Time Dependency

| **Name**     | **Version / Requirement** | **Description**                               |
| ------------ | ------------------------- | --------------------------------------------- |
| **Gin**      | 1.9.1                     | Provides HTTP routing and REST API handling   |
| **ScyllaDB** | Required                  | Stores employee information                   |
| **Swagger**  | Required                  | Provides API documentation through Swagger UI |

---

# 5. Important Ports

| **Port** | **Service**  | **Description**                          |
| -------- | ------------ | ---------------------------------------- |
| `8081`   | Employee API | Receives HTTP/REST API requests          |
| `9042`   | ScyllaDB     | Database connectivity                    |
| `6379`   | Redis        | Cache connectivity when Redis is enabled |

---

# 6. Architecture

The Employee API follows a microservice architecture where clients communicate with the Go-based Employee API through HTTP REST APIs.

## 6.1 Architecture Diagram

<img width="556" height="741" alt="Screenshot 2026-09-13 153312" src="https://github.com/user-attachments/assets/5010b0af-6012-44b9-a3b6-1a59cd5baf06" />


## 6.2 Architecture Components

| **Component**         | **Role**                                  |
| --------------------- | ----------------------------------------- |
| **Client / Frontend** | Sends HTTP requests to the Employee API   |
| **Gin**               | Handles HTTP routing and API requests     |
| **Employee API**      | Processes employee-related operations     |
| **ScyllaDB**          | Provides persistent employee data storage |
| **Redis**             | Provides optional caching                 |
| **Swagger**           | Provides API documentation                |

---

# 7. Monitoring

The Employee API provides basic endpoints for monitoring application availability and metrics.

### Health Check

```text
GET /api/v1/employee/health
```

Used to verify Employee API availability.

### Metrics

```text
GET /metrics
```

Exposes application and request-related metrics.


---
# 8. Disaster Recovery

Disaster Recovery ensures that the Employee API and its employee data can be restored after an application or infrastructure failure.

### Recovery Requirements

| **Component** | **Recovery Approach** |
|---------------|------------------------|
| **Employee API** | Rebuild and redeploy the application from the source repository |
| **ScyllaDB** | Restore employee data from database backups |
| **Configuration** | Restore the required `config.yaml` configuration |
| **Database Schema** | Re-run the existing database migrations if required |



---

# 9. High Availability

For production deployment, High Availability can be achieved by running multiple Employee API instances behind a load balancer.

<img width="705" height="397" alt="Screenshot 2026-09-13 162600" src="https://github.com/user-attachments/assets/05300a1e-257e-4fc6-b011-313da2bbf80b" />

Key considerations:

* Multiple Employee API instances
* Load balancer
* Highly available ScyllaDB
* Database replication
* Highly available Redis when required

---

# 10. Troubleshooting

| **Issue**                      | **Possible Cause**                 | **Resolution / Check**                                 |
| ------------------------------ | ---------------------------------- | ------------------------------------------------------ |
| Employee API is not accessible | Application is not running         | Check application process and startup logs             |
| Port `8081` unavailable        | Port conflict                      | Check the process using port `8081`                    |
| ScyllaDB connection refused    | ScyllaDB unavailable               | Verify ScyllaDB on port `9042`                         |

---

# 11. FAQs

#### Q1: Why is ScyllaDB required for the Employee API?

ScyllaDB is the primary database used by the Employee API to store and retrieve employee information. The API requires connectivity to the `employee_db` keyspace for database-related operations.

#### Q2: What happens if port `8081` is already in use?

The Employee API will not be able to start on port `8081`. Check which process is using the port and either stop it or configure the Employee API to use another available port in `main.go`. After changing the port, rebuild and restart the application.

#### Q3: Why does the Employee API require database migrations?

Database migrations create and update the database schema required by the Employee API. Running the existing migrations ensures that the required tables and schema are available in the `employee_db` keyspace.

#### Q4: What happens if the port is changed in `main.go` but not in the Swagger configuration?

The Employee API may start successfully on the new port, but Swagger may try to send API requests to the old port. The Swagger server URL should also be updated to match the Employee API port.

---

# 12. Contact Information

| **Name**          | **Email Address**                                                                   |
| ----------------- | ----------------------------------------------------------------------------------- |
| **Anshul Kotiya** | [anshul.kotiya.snaatak@mygurukulam.co](mailto:anshul.kotiya.snaatak@mygurukulam.co) |

---

# 13. References

| **Resource**                           | **Reference**                                                                        |
| -------------------------------------- | ------------------------------------------------------------------------------------ |
| **Employee API Repository**            | https://github.com/OT-MICROSERVICES/employee-api                                     |
| **Application Documentation Template** | https://github.com/OT-MICROSERVICES/documentation-template/wiki/Application-Template |
