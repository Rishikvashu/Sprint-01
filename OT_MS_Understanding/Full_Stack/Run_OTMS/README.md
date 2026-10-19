<div align="center">

<h1>OT-Microservices</h1>
<h3>Run with all components (Documentation)</h3>

<br/>

<p>
  <img src="https://img.shields.io/badge/Go-EmployeeAPI-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Python-AttendanceAPI-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Java-SalaryAPI-orange?style=for-the-badge" />
</p>

<p>
  <img src="https://img.shields.io/badge/Flask-NotificationAPI-lightgrey?style=for-the-badge" />
  <img src="https://img.shields.io/badge/ScyllaDB-Database-blueviolet?style=for-the-badge" />
  <img src="https://img.shields.io/badge/PostgreSQL-AttendanceDB-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/NGINX-Frontend-red?style=for-the-badge" />

</p>

</div>

---

| Author       | Created on | Version | Last updated by | Last edited on | Pre Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer  |
| ------------ | ---------- | ------- | --------------- | -------------- | ------------ | ----------- | ----------- | ------------ |
| Maqbool Alam | 07/09/2026 | 1.0     | Maqbool Alam    | 07/09/2026    | Team         | Rajnish | Pritam/Komal | Abhishek/Manish Nautiyal |

---

## Table of Contents

<details>
<summary>Table of Contents</summary>

* [Introduction](#introduction)
* [Architecture Overview](#architecture-overview)

<details>
<summary>Setup for Execution</summary>

* [Setup Logs](#setup-logs)
* [Initial Cleanup (Ports)](#initial-cleanup-ports)
* [Start Infrastructure](#start-infrastructure)
* [Employee API (Go)](#employee-api-go)
* [Attendance API (Python)](#attendance-api-python)
* [Salary API (Java)](#salary-api-java)
* [Notification API (Python)](#notification-api-python)
* [Frontend (React via NGINX)](#frontend-react-via-nginx)

</details>

<details>
<summary>Verification</summary>

* [Frontend UI](#frontend-ui)
* [Frontend Logs](#frontend-logs)
* [Functional API Tests](#functional-api-tests)
* [Swagger Validation](#swagger-validation)
* [Backend Logs](#backend-logs)
* [Notification Test](#notification-test)

</details>

* [Stop Services](#stop-services)
* [FAQs](#faqs)
* [Repository](#repository)
* [Contact Information](#contact-information)
* [References](#references)

</details>

---

## Introduction

OT-Microservices is a polyglot microservices-based Employee Management System where each service is independently deployed, owns its own database, and communicates via REST APIs. NGINX acts as a reverse proxy and serves as the single entry point for frontend and backend services.



---

## Architecture Overview

><img width="1467" height="816" alt="image" src="https://github.com/user-attachments/assets/0478129a-5159-4aeb-8c02-0af5459c639e" />

---

## Setup for Execution

### Setup Logs

```bash
mkdir -p ~/logs
```

### Initial Cleanup (Ports)

```bash
fuser -k 8080/tcp 2>/dev/null
fuser -k 8081/tcp 2>/dev/null
fuser -k 8082/tcp 2>/dev/null
fuser -k 5000/tcp 2>/dev/null
```

### Start Infrastructure

```bash
sudo systemctl start scylla-server
sudo systemctl start postgresql
sudo systemctl start redis
sudo systemctl start nginx
```

---

### Employee API (Go)

```bash
cd ~/OT-Micro/employee-api
nohup go run main.go > ~/logs/employee-api.log 2>&1 &
```

<img width="1837" height="815" alt="image" src="https://github.com/user-attachments/assets/ea56742b-3fc2-4a6e-9c57-36982f845f6f" />


### Attendance API (Python)

```bash
cd ~/OT-Micro/attendance-api
nohup poetry run gunicorn app:app -b 0.0.0.0:8081 > ~/logs/attendance.log 2>&1 &
```

<img width="1837" height="575" alt="image" src="https://github.com/user-attachments/assets/d6cb3986-058f-475b-869a-f8f93567f2da" />


### Salary API (Java)

```bash
cd ~/OT-Micro/salary-api
nohup ./mvnw spring-boot:run > ~/logs/salary.log 2>&1 &
```

<img width="1837" height="784" alt="image" src="https://github.com/user-attachments/assets/3f04af53-4aca-43b9-af98-9d9655384075" />


### Notification API (Python)

```bash
cd ~/OT-Micro/notification-worker
pip3 install -r requirements.txt
nohup python3 notification_api.py --mode api > ~/logs/notification.log 2>&1 &
```

<img width="1837" height="256" alt="image" src="https://github.com/user-attachments/assets/94acc825-72d1-4c26-a4ef-f89b8894e741" />


### Frontend (React via NGINX)

```bash
cd ~/OT-Micro/frontend
npm install
npm run build
sudo systemctl restart nginx
```

<img width="1617" height="738" alt="image" src="https://github.com/user-attachments/assets/2d7ec178-d87a-469e-8908-89f652e28cff" />


---

## Verification

### Frontend UI

```
http://localhost/

```

### Frontend Logs

```bash
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
```

### Functional API Tests

```bash
curl http://localhost:8080/api/v1/employee/search/all | jq
curl http://localhost:8081/api/v1/attendance/search | jq
curl http://localhost:8082/api/v1/salary/search/all | jq
```

### Swagger Validation

```bash
curl -I http://localhost:8080/swagger/index.html
curl -I http://localhost:8081/apidocs/
curl -I http://localhost:8082/swagger-ui/index.html
```

### Backend Logs

```bash
tail -f ~/logs/employee-api.log
tail -f ~/logs/attendance.log
tail -f ~/logs/salary.log
tail -f ~/logs/notification.log
```

### Notification Test

```bash
curl -X POST http://localhost:5000/notification/send \
-H "Content-Type: application/json" \
-d '{"id":"1","name":"Maqbool alam","email":"maqbul.alam.snaatak@mygurukulam.co"}'
```

---

## Stop Services

```bash
fuser -k 8080/tcp
fuser -k 8081/tcp
fuser -k 8082/tcp
fuser -k 5000/tcp
sudo systemctl stop nginx
```

---

## FAQs


**1. Why are services not starting?**

> Ports may be occupied or dependent services (DB/Redis) are not running.

**2. Why am I getting 502 Bad Gateway from NGINX?**

> Backend service is down. Verify using curl on respective ports (8080, 8081, 8082).

**3. Why APIs are returning empty data?**

> Database might not be initialized or migrations not executed.

**4. Why notification service is failing?**

> Missing Python dependencies (e.g., emails module). Run `pip3 install -r requirements.txt`.


**5. Why frontend is not loading properly?**

> React build not generated or NGINX not restarted. Run `npm run build` and restart NGINX.


---

## Repository

[https://github.com/opstree/OT-Microservices](https://github.com/opstree/OT-Microservices)

---

## Contact Information

| Name         | Email                                                                             |
| ------------ | --------------------------------------------------------------------------------- |
| Maqbool Alam | [maqbul.alam.snaatak@mygurukulam.co](mailto:maqbul.alam.snaatak@mygurukulam.co) |

---

## References

| Resource   | Link                                                     |
| ---------- | -------------------------------------------------------- |
| Git Docs   | [https://git-scm.com/docs](https://git-scm.com/docs)     |
| NGINX Docs | [https://nginx.org/en/docs/](https://nginx.org/en/docs/) |
| FrontEnd Documentation | [Link](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-72-MAQBOOL/Documentation/OT_MS_Understanding/Frontend/DOC/README.md) |
| Employee Documentation | [Link](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-68-ANSHUL/Documentation/OT_MS_Understanding/Employee/DOC/README.md) |
| Attendance Documentation | [Link](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-66-VASHISHTHA-PRAKASH/Documentation/OT_MS_Understanding/Attendance/DOC/README.md) |
| Salary Documentation | [Link](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-70-ARJUN/Documentation/OT_MS_Understanding/Salary/DOC/README.md) |
| Notification Documentation | [Link](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-64-SARTHAK/Documentation/OT_MS_Understanding/Notification/DOC/README.md) |
