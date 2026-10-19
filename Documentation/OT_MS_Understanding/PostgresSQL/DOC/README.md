# PostgreSQL  Documentation

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/2/29/Postgresql_elephant.svg" alt="PostgreSQL Logo" width="100"/>
</p>

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Sarthak Bhatnagar | 09-09-2026 | v1.0| Sarthak Bhatnagar | 11-09-2026 | Ishaan | Anuj Jain | Piyush Upadhyay |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Purposes](#2-purposes)
3. [Key Features](#3-key-features)
4. [Pre-requisites](#4-pre-requisites)
5. [PostgreSQL Architecture](#5-PostgreSQL-Architecture)
6. [How to Setup/Install PostgreSQL](#5-how-to-setupinstall-postgresql)
7. [Use Cases](#6-use-cases)
8. [Best Practices](#7-best-practices)
9. [FAQs](#8-faqs)
10. [Conclusion](#9-conclusion)
11. [Contact Information](#10-contact-information)
12. [References](#11-references)

---

# 1. Introduction

This document provides an overview of PostgreSQL, covering its  key features, and major use cases. It is designed to provide a structured understanding of PostgreSQL and its applications in modern software development.

**PostgreSQL** is a popular, open-source, and powerful relational database management system (RDBMS) used to store, manage, and retrieve application data efficiently.


---

# 2. Purposes


| Purpose                      | Description                                                          |
| ---------------------------- | -------------------------------------------------------------------- |
| **Application Data Storage** | Stores application data in a structured format.                      |
| **Data Retrieval**           | Allows applications to retrieve required data using SQL queries.     |
| **Transaction Processing**   | Handles multiple database operations reliably.                       |
| **Data Relationships**       | Manages relationships between different data tables.                 |
| **Application Integration**  | Provides database support for applications, APIs, and microservices. |
| **Data Protection**          | Helps protect application data through access control and backups.   |




---

# 3. Key Features

| Feature             | Description                                       |
| ------------------- | ------------------------------------------------- |
| **Open Source**     | Freely available with community support.          |
| **SQL Support**     | Supports SQL for database operations.             |
| **ACID Compliance** | Ensures reliable and consistent transactions.     |
| **Indexing**        | Enables faster data searching and retrieval.      |
| **Security**        | Provides authentication, roles, and permissions.  |
| **Extensibility**   | Supports extensions, custom types, and functions. |



# 4. Pre-requisites

### 4.1 Software Overview
| **Parameter** | **Details** |
| :--- | :--- |
| **Software** | PostgreSQL 14 / 15 / 16 LTS |
| **Default Port** | `5432` |
| **Default User** | `postgres` |
| **Database Name** | `attendance_db` |

---

### 4.2 System Requirements
| **Component** | **Minimum Requirement** |
| :--- | :--- |
| **CPU** | 2 Cores |
| **RAM** | 2 GB (4 GB recommended) |
| **Disk** | 20 GB free space |
| **OS** | Ubuntu 20.04 / 22.04 LTS / Debian / RHEL |

---

# 5.PostgreSQL Architecture


<img width="1100" height="800" alt="images-2" src="https://github.com/user-attachments/assets/01446359-a790-439d-b535-ee1fa7b24a99" />




# 6. How to Setup/Install PostgreSQL

### Step 1: Install PostgreSQL
```bash
sudo apt update && sudo apt install -y postgresql postgresql-contrib
```

### Step 2: Start & Enable Service
```bash
sudo systemctl enable --now postgresql
```

### Step 3: Verify Service Status
```bash
sudo systemctl status postgresql --no-pager
```

---

# 7. Use Cases

| Use Case                    | Description                                           |
| --------------------------- | ----------------------------------------------------- |
| **Employee Data**           | Stores employee-related application data.             |
| **Attendance Data**         | Stores employee attendance records.                   |
| **Microservices Database**  | Provides database support for microservices.          |
| **Transaction Processing**  | Handles application transactions reliably.            |
| **Application Development** | Used as a backend database for applications and APIs. |


---

# 8. Best Practices

| Best Practice        | Description                                              |
| -------------------- | -------------------------------------------------------- |
| **Regular Backups**  | Take regular backups to protect data.                    |
| **Secure Access**    | Use strong authentication and proper user permissions.   |
| **Use Indexes**      | Create indexes for frequently queried columns.           |
| **Monitor Database** | Monitor performance, storage, and database health.       |
| **Keep Updated**     | Regularly apply PostgreSQL updates and security patches. |

---

# 9. FAQs

**Q: Which service uses PostgreSQL?**  
A: The **Attendance API** uses PostgreSQL as its primary database.

**Q: What is the default database and port?**  
A: Database: `attendance_db`, Port: `5432`, User: `postgres`.

**Q: How are database migrations managed?**  
A: Schema migrations are handled using **Liquibase**.

---

# 10. Conclusion

**PostgreSQL** provides a fast, reliable, and ACID-compliant relational database for the **OT-Microservices Attendance API**, ensuring high concurrency, data integrity, and easy maintenance.

---

# 11. Contact Information

| **Name** | **Email** |
| :--- | :--- |
| Sarthak Bhatnagar | [sarthak.bhatnagar.snaatak@mygurukulam.co](mailto:sarthak.bhatnagar.snaatak@mygurukulam.co) |

---

# 12. References

| **Topic** | **Link** |
| :--- | :--- |
| **PostgreSQL Docs** | [postgresql.org/docs](https://www.postgresql.org/docs/) |
| **OT-Microservices** | [github.com/OT-MICROSERVICES](https://github.com/OT-MICROSERVICES) |
| **Attendance API** | [github.com/OT-MICROSERVICES/attendance-api](https://github.com/OT-MICROSERVICES/attendance-api) |
