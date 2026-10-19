# Frontend

### Detailed Documentation

<p>
  <img src="https://img.shields.io/badge/React-Frontend-red?style=for-the-badge" />
</p>

</div>

---

| Author       | Created on | Version | Last updated by | Last edited on | Pre Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer  |
| ------------ | ---------- | ------- | --------------- | -------------- | ------------ | ----------- | ----------- | ------------ |
| Maqbool Alam | 08/09/2026 | 1.1     | Maqbool Alam    | 16/09/2026     | Team         | Rajnish     | Pritam/Komal | Abhishek/Manish Nautiyal |

---
## Table of Contents

<details>
<summary>Table of Contents</summary>

- [Introduction](#introduction)
- [Pre-requisites](#pre-requisites)
- [System Requirements](#system-requirements)

<details>
<summary>Dependencies</summary>

- [Build time Dependency](#build-time-dependency)
- [Run time Dependency](#run-time-dependency)
- [Other Dependency](#other-dependency)

</details>

- [Important Ports](#important-ports)
- [Others](#others)
- [Architecture](#architecture)
- [Dataflow Diagram](#dataflow-diagram)

<details>
<summary>Step-by-step installation of Frontend</summary>

- [Step1: Installation of software Dependencies](#step1-installation-of-software-dependencies)
- [Step2: Build/Artifact Generation](#step2-buildartifact-generation)
- [Step3: Application Deployment](#step3-application-deployment)

</details>

- [Monitoring](#monitoring)
- [Logging](#logging)
- [Disaster Recovery](#disaster-recovery)
- [High Availability](#high-availability)
- [Troubleshooting](#troubleshooting)
- [FAQs](#faqs)
- [Contact Information](#contact-information)
- [References](#references)

</details>

## Introduction

Frontend is the ReactJS-based web UI of the **OT-Microservices** Employee Management System. It gives employees and admins a single web interface to create and search employee, attendance and salary records, instead of calling the backend REST APIs directly. It is stateless, cross-platform, and only needs a JavaScript runtime to build, plus a static file server (`serve`, or NGINX) to run.

The frontend does not own any database. It is directly dependent on three backend microservices, and indirectly tied to a fourth:

| Service | Role | Called directly by frontend? |
|---------|------|-------------------------------|
| [Employee API](https://github.com/OT-MICROSERVICES/employee-api) (Go) | Employee records | Yes |
| [Attendance API](https://github.com/OT-MICROSERVICES/attendance-api) (Python) | Attendance records | Yes |
| [Salary API](https://github.com/OT-MICROSERVICES/salary-api) (Java) | Salary records | Yes |
| [Notification Worker](https://github.com/OT-MICROSERVICES/notification-worker) (Python) | Emails employees on a schedule using the data above | No — indirect, downstream dependency |

---

## Pre-requisites

Before setting up the frontend, ensure the following are available and reachable.

| Dependency | Version | Purpose |
|------------|---------|----------|
| Node.js | 16.15.1 | Build the React application |
| npm | bundled with Node 16.15.1 | Install packages, run the build |
| NGINX | latest | Serve the production build |
| Employee API | latest | Must be reachable for employee pages to work |
| Attendance API | latest | Must be reachable for attendance pages to work |
| Salary API | latest | Must be reachable for salary pages to work |

---

## System Requirements

The frontend is a lightweight static React build with no heavy compute requirements.

| Hardware Specifications | Minimum Recommendation |
| ------------------------ | ----------------------- |
| Processor                | dual-core               |
| RAM                      | 2GB                     |
| Disk                     | 8GB                     |
| OS                       | Ubuntu (22.04) or any OS with Node.js support |

---

## Dependencies

### Build time Dependency

| Name | Version | Description |
|------|---------|--------------|
| Node.js | 16.15.1 | Required to install packages and build the React app |
| npm | bundled with Node 16.15.1 | Package manager used to install dependencies and run the build |

### Run time Dependency

| Name | Version | Description |
|------|---------|--------------|
| serve (or NGINX) | latest | Serves the production `build/` output |
| Employee API | latest | Backend service for employee records |
| Attendance API | latest | Backend service for attendance records |
| Salary API | latest | Backend service for salary records |

### Other Dependency

| Name | Version | Description |
|------|---------|--------------|
| Notification Worker | latest | Not called directly by the frontend; reads the same data on a schedule to send emails |

---

## Important Ports

| Inbound Traffic | Description |
|------------------|--------------|
| 3000 (or 80 via NGINX) | Frontend web UI |

| Outbound Traffic | Description |
|-------------------|--------------|
| 8080 | Employee API |
| 8081 | Attendance API |
| 8082 | Salary API |

---

## Others

### Stop Service

```
fuser -k 3000/tcp
```

### Repository

<https://github.com/OT-MICROSERVICES/frontend>

---

## Architecture

The browser loads the React app from the frontend server and makes REST calls to the three backend APIs. The Notification Worker is not called by the browser; it separately reads the same data on a schedule to send emails.

---

## Dataflow Diagram

<img width="1709" height="564" alt="image" src="https://github.com/user-attachments/assets/f69303d1-7053-466f-8498-30549a3358fc" />

---

## Step-by-step installation of Frontend

### Step1: Installation of software Dependencies

#### Build Dependency

```
cd ~/OT-Micro/frontend
npm install
```

#### Run time Dependency

The frontend needs the three backend APIs reachable before it is useful. From the machine running the frontend:

```
curl -I http://localhost:8080/api/v1/employee/health
curl -I http://localhost:8081/api/v1/attendance/health
curl -I http://localhost:8082/actuator/health
```

If any of these fail, start/fix that service first — see that service's own repository for its setup steps.

#### Other Dependency

Not applicable for this application.

### Step2: Build/Artifact Generation

```
mkdir -p ~/logs
fuser -k 3000/tcp 2>/dev/null

cd ~/OT-Micro/frontend
npm run build
```

### Step3: Application Deployment

```
nohup npx serve -s build -l 3000 > ~/logs/frontend.log 2>&1 &
```

Ensure the application deployed is in a working state:

```
http://localhost:3000/
```

Open the UI and confirm employee, attendance and salary pages load data. Equivalent API-level check:

```
curl http://localhost:8080/api/v1/employee/search/all | jq
curl http://localhost:8081/api/v1/attendance/search | jq
curl http://localhost:8082/api/v1/salary/search | jq
```

---

## Monitoring

Basic functional and reachability checks for the frontend:

| Check | Command / URL | Expected Result |
|-------|-----------------|-------------------|
| Frontend UI reachable | `http://localhost:3000/` | Page loads without errors |
| Employee data loads | `curl http://localhost:8080/api/v1/employee/search/all` | Returns employee records |
| Attendance data loads | `curl http://localhost:8081/api/v1/attendance/search` | Returns attendance records |
| Salary data loads | `curl http://localhost:8082/api/v1/salary/search` | Returns salary records |

---

## Logging

```
tail -f ~/logs/frontend.log
```

---

## Troubleshooting

| Issue | Possible Cause | Resolution |
|-------|------------------|-------------|
| Blank page after deployment | Backend APIs unreachable | Verify Employee/Attendance/Salary API URLs from [Run time Dependency](#run-time-dependency) |
| `npm install` fails | Node.js version mismatch | Use Node.js `16.15.1` as pinned in the Dockerfile |
| Port already in use | Another process on 3000 | Run the port cleanup step under [Step2: Build/Artifact Generation](#step2-buildartifact-generation) or map to a different port |
| Data not showing on a specific page | That page's backend service is down or unmigrated | Check that service's own health endpoint and logs |

---

## FAQs

**1. Why is the frontend showing a blank page?**
> One or more of the Employee/Attendance/Salary APIs is unreachable. Check each with `curl` against its health endpoint.

**2. Why does `npm run build` fail?**
> Usually a Node.js version mismatch — use `16.15.1` as pinned in the Dockerfile.

**3. Does the frontend talk to the Notification Worker?**
> No. Notification Worker is a separate scheduled job that later reads the data the frontend creates; the frontend never calls it directly.

**4. Can I run the frontend without Docker?**
> Yes — `npm install && npm run build`, then serve the `build/` folder with `serve` or any static file server.

---

## Contact Information

| Name | Email address |
|------|-----------------|
| Maqbool Alam | maqbul.alam.snaatak@mygurukulam.co |

---

## References

| Resource | Link |
|----------|------|
| Frontend repository | <https://github.com/OT-MICROSERVICES/frontend> |
| Employee API | <https://github.com/OT-MICROSERVICES/employee-api> |
| Attendance API | <https://github.com/OT-MICROSERVICES/attendance-api> |
| Salary API | <https://github.com/OT-MICROSERVICES/salary-api> |
| Notification Worker | <https://github.com/OT-MICROSERVICES/notification-worker> |
| Frontend POC | [Link](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUN-71-MANSOOR/Documentation/OT_MS_Understanding/Frontend/POC/README.md) |
| Employee Documentation | [Link](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-68-ANSHUL/Documentation/OT_MS_Understanding/Employee/DOC/README.md) |
| Attendance Documentation | [Link](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-66-VASHISHTHA-PRAKASH/Documentation/OT_MS_Understanding/Attendance/DOC/README.md) |
| Salary Documentation | [Link](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-70-ARJUN/Documentation/OT_MS_Understanding/Salary/DOC/README.md) |
| Notification Documentation | [Link](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-64-SARTHAK/Documentation/OT_MS_Understanding/Notification/DOC/README.md) |
