# Notification Setup & Run for POC

<p align="center">
  <img src="https://cdn-icons-png.flaticon.com/512/3602/3602145.png" alt="Notification Service" width="90"/>
</p>

---


# Author Table

| Author | Created On | Version | L0 Reviewer | L1 Reviewer | L2 Reviewer     |
| :----- | :--------- | :------ | :---------- | :---------- | :-------------- |
| Rani   | 12-09-2026 | 1.0     | Ishaan      | Anuj Jain   | Piyush Upadhyay |

---

## Table of Contents

* [Introduction](#introduction)
* [Prerequisites](#prerequisites)
* [Installation Steps](#installation-steps)
* [Application Setup](#application-setup)
* [Verification](#verification)
* [Jira Ticket Rough Notes](#jira-ticket-rough-notes)
* [Troubleshooting](#troubleshooting)
* [FAQs](#faqs)
* [Contact Information](#contact-information)
* [References](#references)

</details>

---

## Introduction

Notification Worker is used as the asynchronous email notification service for the OT Microservices suite.

---

## Prerequisites

* Ubuntu 22.04+ Cloud VM or WSL2
* Python 3.8+ & pip
* Docker (for MailHog mock SMTP server & Elasticsearch)
* Elasticsearch 7.8+ (running on port 9200)
* Open Ports: `22` (SSH), `9200` (ES), `1025` (SMTP), `8025` (MailHog Web UI)

---

## Installation Steps

### Step 1 — Update System

```bash
sudo apt update && sudo apt upgrade -y
```
<img width="1335" height="442" alt="installation-step-1 png" src="https://github.com/user-attachments/assets/76e12acc-4a28-48d5-9ed6-d88e1a9343a3" />


### Step 2 — Install System Dependencies

```bash
sudo apt install -y python3 python3-pip python3-venv build-essential libxml2-dev libxslt1-dev git docker.io
```

<img width="1085" height="592" alt="Docker status" src="https://github.com/user-attachments/assets/421028e5-7ffd-4a16-b01c-3d47f68f40bc" />


### Step 3 — Start MailHog (Mock SMTP Server)

```bash
sudo docker run -d -p 1025:1025 -p 8025:8025 --name mailhog mailhog/mailhog
```
<img width="1906" height="580" alt="mailhog-services" src="https://github.com/user-attachments/assets/17a5e6b4-e28e-4150-8fe6-788d5f8b37c6" />


### Step 4 — Verify MailHog Service

```bash
sudo docker ps | grep mailhog
```

<img width="1327" height="222" alt="verify-mailhog-service" src="https://github.com/user-attachments/assets/82f46a2a-ca62-42ca-8778-74b8f23a57c9" />

## Application Setup

### Step 1 — Clone Repository

```bash
git clone https://github.com/OT-MICROSERVICES/notification-worker.git
cd notification-worker
```

### Step 2 — Install Python Requirements

```bash
pip3 install -r requirements.txt --break-system-packages
```
<img width="1920" height="486" alt="install-python-Req" src="https://github.com/user-attachments/assets/0ece440d-3831-443d-a87a-0da79c2c6d9c" />

### Step 3 — Configure `config.yaml`

```bash
cat <<EOF > config.yaml
---
smtp:
  from: "admin@otms.com"
  username: "test"
  password: "test"
  smtp_server: "127.0.0.1"
  smtp_port: "1025"

elasticsearch:
  username: "elastic"
  password: "changeme"
  host: "127.0.0.1"
  port: 9200
EOF
```

### Step 4 — Create Elasticsearch Index

```bash
curl -X PUT "http://127.0.0.1:9200/employee-management" -H 'Content-Type: application/json'
```

Expected output:
```json
{"acknowledged": true, "shards_acknowledged": true, "index": "employee-management"}
```

### Step 5 — Seed Test Employee Data

```bash
curl -X POST "http://127.0.0.1:9200/employee-management/_doc/101" \
     -H 'Content-Type: application/json' \
     -d '{
       "name": "Rahul Kumar",
       "email_id": "rahul.kumar@otms.com",
       "department": "DevOps",
       "status": "Active"
     }'
```
---
### Step 6 — Run Notification API

export CONFIG_FILE="$(pwd)/config.yaml"
python3 notification_api.py --mode external

## Verification

### Step 1 — Verify Ingested Records

```bash
curl -X GET "http://127.0.0.1:9200/employee-management/_search?pretty"
```
<img width="1900" height="670" alt="varify" src="https://github.com/user-attachments/assets/9fc8f851-3f8e-41a1-92bb-e1b1d9174291" />


### Step 2 — Verify Delivered Email in MailHog Web UI

Open in browser:
```
http://localhost:8025
```
<img width="875" height="676" alt="png" src="https://github.com/user-attachments/assets/91d5d281-4132-4a91-86fe-66aa0f779c3a" />


---

## Jira Ticket Rough Notes

> Rough notes documented for Jira ticket reference and reviewer sign-off:

```text
================================================================================
JIRA TICKET ROUGH NOTES - POC DEMO
Ticket: OT MS Understanding | Notification | Setup and run the App for POC
Author: Rani Kumari
Date: 12/09/2026
================================================================================

1. Cloud Environment:
   - OS: Ubuntu 22.04 LTS (AWS EC2 / GCP Compute Engine)
   - Ports: 22 (SSH), 9200 (ES), 1025 (SMTP), 8025 (MailHog UI)

2. Commands Run:
   - docker run -d -p 1025:1025 -p 8025:8025 --name mailhog mailhog/mailhog
   - pip3 install -r requirements.txt
   - curl -X PUT "http://127.0.0.1:9200/employee-management"
   - curl -X POST "http://127.0.0.1:9200/employee-management/_doc/101" -d '{"name":"Rahul Kumar","email_id":"rahul.kumar@otms.com"}'
   - export CONFIG_FILE="$(pwd)/config.yaml"
   - python3 notification_api.py --mode external

3. Verification Status:
   [x] Elasticsearch service running and test data query returns 200 OK
   [x] MailHog mock SMTP container active on ports 1025 & 8025
   [x] Notification worker runs successfully without exceptions
   [x] Salary Slip email verified in MailHog Web dashboard
================================================================================
```

---

## Troubleshooting

### KeyError: 'email_id'

* **Cause:** Elasticsearch documents do not contain the `email_id` field.
* **Fix:** Verify documents have `"email_id"` key using `curl -X GET "http://127.0.0.1:9200/employee-management/_search"`.

### SMTP Connection Refused

```bash
docker ps | grep mailhog
```

* Verify MailHog container is running.
* Verify port `1025` is accessible.

### Elasticsearch Connection Refused

```bash
curl -I http://127.0.0.1:9200
```

* Verify Elasticsearch service is up and bound to `127.0.0.1`.

### Config File Error

```bash
export CONFIG_FILE="$(pwd)/config.yaml"
```

* Ensure `CONFIG_FILE` environment variable points to a valid file.

---

## FAQs

**Q1. Why does Notification Worker use Elasticsearch instead of PostgreSQL?**

> In the OT Microservices architecture, PostgreSQL is reserved for Attendance data, while employee master records are indexed in Elasticsearch for fast search queries.

**Q2. Which service is used to test emails?**

> MailHog is used to capture and view test emails.


**Q3. How to verify email delivery without a real SMTP server?**

> Use MailHog running in Docker. It catches all outgoing emails on port `1025` and displays them in a web UI on port `8025`.

---

## Contact Information

| Name         | Email                                                                             |
| ------------ | --------------------------------------------------------------------------------- |
| Rani Kumari | [rani.kumari.snaatak@mygurukulam.co](mailto:rani.kumari.snaatak@mygurukulam.co) |

---

## References

| **Resource** | **Link** |
|---|---|
| OT Microservices Repo | [OT Microservices GitHub](https://github.com/OT-MICROSERVICES) |
| MailHog Documentation | [MailHog Documentation](https://github.com/mailhog/MailHog) |
