#  Notification Documentation

<p align="center">
  <img src="https://cdn-icons-png.flaticon.com/512/3602/3602145.png" alt="Notification Service" width="90"/>
</p>

---


# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Sarthak Bhatnagar | 09-09-2026 | v1.0  | Sarthak Bhatnagar | 09-09-2026 | Ishaan | Anuj Jain | Piyush Upadhyay |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Purpose](#2-purpose)
3. [Prerequisites](#3-prerequisites)
4. [Notification API Installation](#4-notification-api-installation)
5. [Configuration](#5-configuration)
6. [Verify Notification Execution](#6-verify-notification-execution)
7. [Validation Checklist](#7-validation-checklist)
8. [Use Cases](#8-use-cases)
9. [Troubleshooting](#9-troubleshooting)
10. [Best Practices](#10-best-practices)
11. [Conclusion](#11-conclusion)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

# 1. Introduction

This document provides a concise **step-by-step setup** for the **Notification API (Notification Worker)**. 

---

# 2. Purpose

| **Requirement** | **Description** |
| :--- | :--- |
| **Install API** | Set up Python 3, virtual environment, and project packages. |
| **Configure** | Configure `config.yaml` and persistent `CONFIG_FILE`. |
| **Verify** | Validate execution in `external` mode. |

---

# 3. Prerequisites

| **Prerequisite** | **Details** |
| :--- | :--- |
| **Operating System** | Ubuntu 20.04 / 22.04 LTS or above (+) |
| **Python Runtime** | Python 3.9+ with `pip` and `venv` |
| **System Libraries** | `build-essential`, `libxml2-dev`, `libxslt1-dev` |


---

# 4. Notification API Installation

### Step 1: Update System Packages
```bash
sudo apt update
```
<img width="1188" height="666" alt="Screenshot 2026-09-09 at 2 09 49 AM" src="https://github.com/user-attachments/assets/7b1fe651-dabc-442d-b5fe-04f715926f0c" />


### Step 2: Install Dependencies
```bash
sudo apt install -y python3 python3-pip python3-venv build-essential libxml2-dev libxslt1-dev
```
<img width="1188" height="692" alt="Screenshot 2026-09-09 at 2 11 04 AM" src="https://github.com/user-attachments/assets/a2f4e92d-c48a-4217-a526-135625d244f8" />


### Step 3: Navigate to Directory
```bash
cd Snaatak/notification-worker
```
<img width="1188" height="119" alt="Screenshot 2026-09-09 at 2 12 07 AM" src="https://github.com/user-attachments/assets/9880b0e6-b903-48cf-a4e0-ffb733d3be57" />

### Step 4: Create Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate
```
<img width="1188" height="119" alt="Screenshot 2026-09-09 at 2 12 37 AM" src="https://github.com/user-attachments/assets/b6fb0d63-8c3b-4b92-967a-2a40b02eee6a" />


### Step 5: Install Python Packages
```bash
pip install -r requirements.txt
```
<img width="1188" height="597" alt="Screenshot 2026-09-09 at 2 13 16 AM" src="https://github.com/user-attachments/assets/9c515bc2-7785-4907-a733-cf7873739f6e" />


---

# 5. Configuration

### 5.1 config.yaml
Update `config.yaml` with your connection settings:
```yaml
---
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
<img width="1188" height="354" alt="Screenshot 2026-09-09 at 2 18 24 AM" src="https://github.com/user-attachments/assets/1fdadecf-bbb9-48ff-951a-e32fa5212383" />


### 5.2 CONFIG_FILE
Configure persistent environment variable:
```bash
export CONFIG_FILE=$(pwd)/config.yaml
echo "export CONFIG_FILE=$(pwd)/config.yaml" >> ~/.bashrc
```
<img width="1188" height="190" alt="Screenshot 2026-09-09 at 2 25 26 AM" src="https://github.com/user-attachments/assets/2549e415-ddac-4641-98f9-d18c474cdb13" />


---

# 6. Verify Notification Execution

Run the application in external mode to trigger email notifications:

```bash
python3 notification_api.py --mode external
```


---

# 7. Validation Checklist

| **Check** | **Command** | **Expected Result** |
| :--- | :--- | :--- |
| **Python Version** | `python3 --version` | Python 3.9+ |
| **CONFIG_FILE** | `echo $CONFIG_FILE` | Path to `config.yaml` |
| **Run Execution** | `python3 notification_api.py --mode external` | Sends email and exits 0 |

---

# 8. Use Cases

| **Use Case** | **Description** | **Example Command** |
| :--- | :--- | :--- |
| **Salary Slip Alerts** | Sends email notifications when salary slips are generated. | `python3 notification_api.py --mode external` |
| **Scheduled Worker** | Periodic background run to automatically dispatch emails. | `python3 notification_api.py --mode scheduled` |
| **On-Demand Trigger** | Manual run to notify all employees from Elasticsearch. | `python3 notification_api.py -m external` |

---

# 9. Troubleshooting


| **Issue**                           | **Cause**                                                      | **Solution**                                                                     |
| ----------------------------------- | -------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Configuration file cannot be parsed | `CONFIG_FILE` path is missing or incorrect.                    | Set the correct path: `export CONFIG_FILE=$(pwd)/config.yaml`                    |
| Elasticsearch query fails           | Elasticsearch is not running or the required index is missing. | Make sure Elasticsearch is running on port `9200` and the required index exists. |
| Email cannot be sent                | SMTP server or credentials are incorrect.                      | Check the SMTP host, port, TLS settings, and credentials in `config.yaml`.       |

---

# 10. Best Practices

| **Best Practice** | **Description** |
| :--- | :--- |
| **Containerized Runtime** | Run the worker inside Docker for identical environments across dev and production. |
| **Secret Management** | Inject sensitive credentials using environment variables instead of hardcoding in files. |
| **Process Auto-Restart** | Configure a systemd service or Docker restart policy so the worker auto-heals on failure. |
| **Log Rotation** | Enable log rotation (`logrotate`) to prevent application log files from filling up disk space. |




---

# 11. Conclusion

This setup  provides a standardized, reliable procedure to install, configure, and verify the **Notification API**. Following these steps ensures dependencies, environment variables, and execution modes run without issues.

---

# 12. Contact Information

| **Name** | **Email** |
| :--- | :--- |
| Sarthak Bhatnagar | [sarthak.bhatnagar.snaatak@mygurukulam.co](mailto:sarthak.bhatnagar.snaatak@mygurukulam.co) |

---

# 13. References

| **Topic** | **Link** |
| :--- | :--- |
| **OT-Microservices** | [github.com/OT-MICROSERVICES](https://github.com/OT-MICROSERVICES) |
| **Elasticsearch Python** | [elasticsearch-py.readthedocs.io](https://elasticsearch-py.readthedocs.io/) |
| **Schedule Library** | [schedule.readthedocs.io](https://schedule.readthedocs.io/) |
