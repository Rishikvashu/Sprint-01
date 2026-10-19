# OT-Microservices | Full Stack | Detailed Documentation

---

## Author Table

| **Author**    | **Created On** | **Version** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ------------- | -------------- | ----------- | ------------------ | --------------- | --------------- | --------------- |
| Anshul Kotiya | 13-09-2026     | 1.0         | 13-09-2026         | Shubham Rathi / Sunny   | Shreya / Nikita | Piyush Upadhyay |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Application Overview](#2-application-overview)
3. [Overall Architecture](#3-overall-architecture)

   * [3.1 Component Architecture Diagram](#31-component-architecture-diagram)
4. [Application Flows](#4-application-flows)

   * [4.1 Overall Application Flow](#41-overall-application-flow)
   * [4.2 Employee Flow](#42-employee-flow)
   * [4.3 Salary Flow](#43-salary-flow)
   * [4.4 Attendance Flow](#44-attendance-flow)
   * [4.5 Notification Flow](#45-notification-flow)
5. [Component Communication](#5-component-communication)
6. [Conclusion](#6-conclusion)
7. [Contact Information](#7-contact-information)
8. [References](#8-references)

---

# 1. Introduction

This document provides the **overall component architecture** and **various application flow diagrams** of the OT-Microservices application.

---

# 2. Application Overview

OT-Microservices follows a microservices architecture where different business functionalities are handled by independent services.

The application consists of the following major components:

* React Frontend
* NGINX
* Employee API
* Salary API
* Attendance API
* Notification Service
* ScyllaDB
* PostgreSQL
* Redis

The React Frontend communicates with the Employee, Salary, and Attendance APIs through NGINX.

The Employee API and Salary API use ScyllaDB, while the Attendance API uses PostgreSQL.

The Notification Service operates independently from the normal Frontend-to-Backend request path.

---

# 3. Overall Architecture

## 3.1 Component Architecture Diagram

The following diagram represents the overall component architecture of the OT-Microservices application, including the frontend, nginx proxy, backend services, databases, Redis, and Notification Service.

<img width="1150" height="592" alt="Screenshot 2026-09-24 193855" src="https://github.com/user-attachments/assets/4cdda833-da4a-4483-b818-8839ca49857d" />

---

# 4. Application Flows

## 4.1 Overall Application Flow

The following diagram represents the overall application request flow.

A user interacts with the React Frontend. The request is forwarded to NGINX, which routes it to the appropriate backend service. The backend service communicates with its respective database when data access is required.

<img width="1092" height="642" alt="Screenshot 2026-09-14 014132" src="https://github.com/user-attachments/assets/5e6f939d-9c50-4c1a-93fe-a6c5b3624d9e" />



---

## 4.2 Employee Flow

The Employee API handles employee-related operations. Employee requests originate from the frontend and are routed through NGINX to the Employee API.

<img width="1042" height="126" alt="Screenshot 2026-09-14 014250" src="https://github.com/user-attachments/assets/da2076d7-b7dc-409c-8f56-b25561419165" />


---

## 4.3 Salary Flow

The Salary API handles salary-related operations. Salary requests are routed through NGINX to the Salary API, which communicates with ScyllaDB.

<img width="1027" height="138" alt="Screenshot 2026-09-14 014416" src="https://github.com/user-attachments/assets/3548f9f0-8301-4901-ad02-5cebef7b9856" />


---

## 4.4 Attendance Flow

The Attendance API handles attendance-related operations. Attendance requests are routed through NGINX to the Attendance API, which communicates with PostgreSQL.

<img width="1078" height="123" alt="Screenshot 2026-09-14 014454" src="https://github.com/user-attachments/assets/a3f46e00-4eea-475a-a234-dda5e5b941cf" />


---

## 4.5 Notification Flow

The Notification Service works independently from the normal frontend and NGINX request path.

It is triggered separately and communicates with the Employee, Salary, and Attendance APIs to retrieve the required application data before sending notifications.

<img width="1207" height="480" alt="Screenshot 2026-09-14 014550" src="https://github.com/user-attachments/assets/db028291-cdee-44ec-bc79-2b4bc0738d85" />

---

# 5. Component Communication

The following table summarizes the communication between the major application components.

| **Source**           | **Destination**                     | **Purpose**                        |
| -------------------- | ----------------------------------- | ---------------------------------- |
| User                 | React Frontend                      | User interaction                   |
| React Frontend       | NGINX                               | API requests                       |
| NGINX                | Employee API                        | Employee operations                |
| NGINX                | Salary API                          | Salary operations                  |
| NGINX                | Attendance API                      | Attendance operations              |
| Employee API         | ScyllaDB                            | Employee data                      |
| Salary API           | ScyllaDB                            | Salary data                        |
| Attendance API       | PostgreSQL                          | Attendance data                    |
| Application Services | Redis                               | Supporting in-memory operations    |
| Notification Service | Employee / Salary / Attendance APIs | Retrieve required application data |
| Notification Service | Email                               | Send notifications                 |

---

# 6. Conclusion

This document provides the overall architecture and application flow of the OT-Microservices application, showing how its major components communicate and interact.


---

# 7. Contact Information

| **Name**      | **Email**                                                                           |
| ------------- | ----------------------------------------------------------------------------------- |
| Anshul Kotiya | [anshul.kotiya.snaatak@mygurukulam.co](mailto:anshul.kotiya.snaatak@mygurukulam.co) |

---

# 8. References

| **Reference**                                                                  | **Description**                 |
| ------------------------------------------------------------------------------ | ------------------------------- |
| [OT-Microservices GitHub](https://github.com/OT-MICROSERVICES)                 | OT-Microservices organization   |
| [Frontend](https://github.com/OT-MICROSERVICES/frontend)                       | Frontend repository             |
| [Employee API](https://github.com/OT-MICROSERVICES/employee-api)               | Employee API repository         |
| [Salary API](https://github.com/OT-MICROSERVICES/salary-api)                   | Salary API repository           |
| [Attendance API](https://github.com/OT-MICROSERVICES/attendance-api)           | Attendance API repository       |
| [Notification Worker](https://github.com/OT-MICROSERVICES/notification-worker) | Notification Service repository |
