<h1 align="left"> Salary | Detailed documentation</h1>

---

## Author Table

| Author | Created on | Version | Last updated by | Last edited on | PRE Reviewer | L0 Reviewer    | L1 Reviewer  | L2 Reviewer              |
| ------ | ---------- | ------- | --------------- | -------------- | ------------ | -------------- | ------------ | ------------------------ |
| Arjun  | 11-09-2026 | v1.0    | Arjun           | 19-09-2026     | Team         | Rajnish Sharma | Pritam/Komal | Abhishek/Manish Nautiyal |


---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [Key Objectives](#3-key-objectives)
4. [Pre-Requisites](#4-pre-requisites)
   * [4.1 System Requirements](#41-system-requirements)
   * [4.2 Dependencies](#42-dependencies)
      * [4.2.1 Build Time Dependencies](#421-build-time-dependencies)
      * [4.2.2 Run Time Dependencies](#422-run-time-dependencies)
      * [4.2.3 Other Dependencies](#423-other-dependencies)
   * [4.3 Important Ports](#43-important-ports)
      * [4.3.1 Inbound Traffic](#431-inbound-traffic)
      * [4.3.2 Outbound Traffic](#432-outbound-traffic)
5. [Architecture](#5-architecture)
   * [5.1 Architecture Overview](#51-architecture-overview)
   * [5.2 Core Components](#52-core-components)
   * [5.3 API Request Flow](#53-api-request-flow)
   * [5.4 Caching Mechanism](#54-caching-mechanism)
   * [5.5 Database Layer](#55-database-layer)
   * [5.6 Monitoring and Metrics](#56-monitoring-and-metrics)
6. [Application Startup Workflow](#6-application-startup-workflow)
7. [Health Check](#7-health-check)
8. [Logging and Monitoring](#8-logging-and-monitoring)
   * [8.1 Application Logs](#81-application-logs)
   * [8.2 Health Monitoring](#82-health-monitoring)
   * [8.3 Metrics Monitoring](#83-metrics-monitoring)
9. [Troubleshooting](#9-troubleshooting)
10. [FAQs](#10-faqs)
11. [How to Bring Up the Salary API](#11-how-to-bring-up-the-salary-api)
12. [Contacts](#12-contacts)
13. [References](#13-references)

---

## 1. Introduction

The Salary API is a Java-based microservice developed for managing salary-related operations independently within the OT-Microservices architecture. It uses Spring Boot, ScyllaDB, and Redis to provide scalable and efficient salary management services.

---

## 2. Purpose

The purpose of the Salary API is to manage employee salary records in a scalable, secure, and maintainable way. It enables independent deployment, faster data access using caching, and reliable distributed data storage.

---

## 3. Key Objectives

| Objective | Description |
|---|---|
| Centralized Salary Management | Provide a centralized system to manage employee salary-related operations and data. |
| Independent Service Deployment | Enable salary-related services to be deployed and updated independently without affecting other services. |
| Faster Response Time | Reduce API response time and improve performance using Redis caching. |
| Reliable Distributed Storage | Ensure high availability and reliable distributed data storage using ScyllaDB. |
| Fault Isolation | Improve fault isolation so issues in one service do not impact the entire application. |
| Horizontal Scalability | Support scaling the application horizontally by adding more service instances when needed. |
| Easier Troubleshooting & Maintenance | Simplify debugging, monitoring, and maintenance through modular service architecture. |
| Better Observability | Improve observability using health checks, metrics, and monitoring endpoints. |
| REST-Based Communication | Provide RESTful APIs for frontend applications and internal microservice communication. |

---

## 4. Pre-Requisites

Before deploying the Salary API, ensure that the following hardware, software, and security requirements are met.

---

## 4.1 System Requirements

### Hardware Specifications

|Hardware	|Minimum Recommendation|
|-----| -------|
|Cloud Platform	|AWS|
|Service|	Amazon EC2|
|Processor Architecture	|Linux AMD64 / x86_64|
|Operating System|	Ubuntu 24.04 LTS|

---

## 4.2 Dependencies

### 4.2.1 Build Time Dependencies

| Name | Version | Description |
|---|---|---|
| Java (JDK) | 11 or above | Required to compile the application |
| Maven | 3.x | Build and dependency management |

---

### 4.2.2 Run Time Dependencies

| Name | Version | Description |
|---|---|---|
| Java Runtime (JRE) | 17 | Required to run the application |
| ScyllaDB | Cassandra compatible | Primary database for salary data |
| Redis | Latest stable | Cache management |
| Migrate| Latest | Database schema migration |
---

### 4.2.3 Other Dependencies

| Name | Version | Description |
|---|---|---|
| Prometheus | Optional | Metrics scraping |
| Swagger UI | Bundled | API documentation and testing |

---

## 4.3 Important Ports

### 4.3.1 Inbound Traffic

| Port | Description |
|---|---|
| 9042 | Used by ScyllaDB |

---

### 4.3.2 Outbound Traffic

| Port | Description |
|---|---|
| 8082 | Used by embedded Tomcat server |

---

## 5. Architecture

## 5.1 Architecture Overview

The Salary API follows a microservices-based architecture where all salary-related operations are handled independently by a dedicated Spring Boot service.

<details>
<summary><b>Click to View Architecture</b></summary>

<br>

<img width="1774" height="887" alt="image" src="https://github.com/user-attachments/assets/c61496a0-d98d-41e5-afb9-efa78566593f" />

</details>


---

## 5.2 Core Components

| Component | Description |
|---|---|
| Client / Frontend | Sends salary-related API requests |
| Salary API | Main Spring Boot microservice handling business logic |
| Redis | In-memory caching layer for faster response |
| ScyllaDB | Distributed NoSQL database storing salary records |
| Prometheus | Metrics collection and monitoring |
| Spring Boot Actuator | Health checks and operational endpoints |

---

## 5.3 API Request Flow

1. Client sends an HTTP request to the Salary API
2. Salary API validates and processes the request
3. Redis cache is checked for existing data
4. If cache hit occurs, response is returned immediately
5. If cache miss occurs:
   * Data is fetched from ScyllaDB
   * Retrieved data is cached in Redis
   * Response is returned to the client
   

<details>
<summary><b>Click to View Architecture</b></summary>

<br>

<img width="1774" height="887" alt="image" src="https://github.com/user-attachments/assets/1342fb22-1e02-4f5a-aa17-c3578b290622" />

</details>


---

## 5.4 Caching Mechanism

Redis is used as a caching layer to improve application performance and reduce direct database calls.

### Advantages of Caching

* Faster response time
* Reduced database load
* Better scalability
* Improved user experience

The API first checks Redis before querying ScyllaDB.

---

## 5.5 Database Layer

ScyllaDB is used as the primary distributed database for salary data storage.

ScyllaDB is Cassandra-compatible and provides:

* High throughput
* Horizontal scalability
* Fault tolerance
* Distributed storage
* Low latency operations

The application communicates with ScyllaDB using Cassandra-compatible drivers.

---

## 5.6 Monitoring and Metrics

The Salary API exposes health and metrics endpoints using Spring Boot Actuator.

Prometheus can scrape metrics from the application for monitoring purposes.

Monitoring helps in:

* Service health tracking
* Performance monitoring
* Error analysis
* Capacity planning
* Operational troubleshooting

---

## 6. Application Startup Workflow

<details>
<summary><b>Click to View Workflow</b></summary>

<br>

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/ba18e1ba-405c-470a-a37a-f3d758d29ceb" />


</details>

---

## 7. Health Check

Run the following command to verify whether the Salary API application is running properly:

```bash
curl http://localhost:8082/actuator/health
```

## Expected Output

```json
{"status":"UP","components":{"cassandra":{"status":"UP"}}}
```

---

## 8. Logging and Monitoring

The Salary API provides operational logging and monitoring capabilities for troubleshooting and observability.

## 8.1 Application Logs

Logs help identify:

* Startup failures
* Runtime exceptions
* Database connectivity issues
* Cache failures
* API processing errors

### View Logs

```bash
tail -f ~/salary.log
```

---

## 8.2 Health Monitoring

> Health checks are exposed using Spring Boot Actuator.

### Health Endpoint

```bash
curl http://localhost:8082/actuator/health
```

---

## 8.3 Metrics Monitoring

Prometheus-compatible metrics can be exposed for:

* API response time
* Request count
* JVM memory usage
* CPU utilization
* Database metrics

---


## 9. Troubleshooting

| Issue                        | Resolution                                        | Command                                                    |    
| ---------------------------- | ------------------------------------------------- | ---------------------------------------------------------- | 
| Application not starting     | Verify Java version and application configuration | `java -version` , `cat src/main/resources/application.yml` |    
| Migration failure            | Validate credentials and migration settings       | `cat migration.json`                                       |  
| Redis connection error       | Ensure Redis service is running and reachable     | `sudo systemctl status redis`                              | 
| Health endpoint DOWN         | Verify database connectivity and service status   | `curl http://localhost:8082/actuator/health`               |
| Port already in use          | Verify which process is using the port            | `sudo ss -ltnp `                                            |
| Application logs not visible | Verify application logs for runtime errors        | `tail -f ~/salary.log`                                     |
---

## 10. FAQs

**Question:** Is this application free?  
**Answer:** Yes, this is an open-source application.

**Question:** Can it be deployed on all cloud platforms?  
**Answer:** Yes, the application is cloud-agnostic and can be deployed on any cloud provider.

**Question:** Is an enterprise version available?  
**Answer:** No, only the open-source version is available.

---

## 11. How to Bring Up the Salary API

To set up and run this application, refer to the official repository documentation:

 [Salary POC README](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-69-ANURAAG/Documentation/OT_MS_Understanding/Salary/POC/README.md)

---

## 12. Contacts

| Name        | Email Address                                                                   |
| ----------- | ------------------------------------------------------------------------------- |
| Arjun Kumar | [arjun.kumar.snaatak@mygurukulam.co](mailto:arjun.kumar.snaatak@mygurukulam.co) |

---

## 13. References

| Topic                                                                                                                      | Description                                    |
| -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| [Jenkins Installation Documentation](https://www.jenkins.io/doc/book/installing/linux/#debianubuntu)                       | Jenkins installation and Linux setup reference |
| [FAQ Structure Reference](https://amplifi.com/user-guide/FAQs.html)                                                        | FAQ documentation structure reference          |
| [Introduction vs Overview Reference](https://thecontentauthority.com/blog/introduction-vs-overview)                        | Guidance for writing introduction sections     |
