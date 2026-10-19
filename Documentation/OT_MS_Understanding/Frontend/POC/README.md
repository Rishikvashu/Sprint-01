# POC: Frontend 


<p align="center">
<img width="325" height="325" alt="frontend-logo" src="https://github.com/user-attachments/assets/30f75174-4bdd-4de4-aa38-f8631b61070b" />
</p>


| Author       | Created on | Version | Last updated by | Last edited on | Pre Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer  |
| ------------ | ---------- | ------- | --------------- | -------------- | ------------ | ----------- | ----------- | ------------ |
| Mansoor Alam Alam| 07/09/2026 | 1.0     | Mansoor Alam   | 10/09/2026     | Team         | Rajnish | Pritam/Komal | Abhishek/Manish Nautiyal |
| Mansoor Alam Alam| 07/09/2026 | 1.1    | Mansoor Alam   | 17/09/2026     | Team         | Rajnish | Pritam/Komal | Abhishek/Manish Nautiyal |

---

<details>
<summary>Table of Contents</summary>

1. [Introduction](#introduction)
2. [Detailed Frontend Documentation](#detailed-frontend-documentation)
3. [Purpose](#purpose)
4. [System Requirements](#system-requirements)
5. [Prerequisites](#prerequisites)
6. [Ports Required](#ports-required)
7. [Installation and Setup](#installation-and-setup)
   - [1. Update Package Index](#1-update-package-index)
   - [2. Install Node.js and npm](#2-install-nodejs-and-npm)
   - [3. Verify Node.js and npm Installation](#3-verify-nodejs-and-npm-installation)
   - [4. Install Nginx](#4-install-nginx)
   - [5. Clone the Frontend Repository](#5-clone-the-frontend-repository)
   - [6. Install Project Dependencies](#6-install-project-dependencies)
   - [7. Start the Frontend Application](#7-start-the-frontend-application)
   - [8. Configure Nginx as a Reverse Proxy](#8-configure-nginx-as-a-reverse-proxy)
   - [9. Enable the Nginx Configuration](#9-enable-the-nginx-configuration)
   - [10. Validate Nginx Configuration](#10-validate-nginx-configuration)
   - [11. Reload & Check Status Nginx](#11-reload--check-status-nginx)
   - [12. Validate Frontend Through Nginx](#12-validate-frontend-through-nginx)
   - [13. Access Frontend Through Public IP](#13-access-frontend-through-public-ip)
   - [14. Configure Frontend as a System Service](#14-configure-frontend-as-a-system-service)
   - [15. Reload and Start the Frontend Service](#15-reload-and-start-the-frontend-service)
8. [Conclusion](#conclusion)
9. [Contact](#contact)
10. [References](#references)

</details>

---

## Introduction

This document provides a Proof of Concept (PoC) for the manual setup and execution of the Frontend application of the OT-Microservices project.

The frontend is developed using React.js and provides a user interface for interacting with the application’s backend microservices. This document covers the required system setup, dependencies, application configuration, build process, and execution of the frontend application on a Linux-based system.

## Detailed Frontend Documentation

For detailed information about the frontend application, its architecture, components, configuration, and other frontend-related details, refer to the following documentation:

[Frontend Documentation](<https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-72-MAQBOOL/Documentation/OT_MS_Understanding/Frontend/DOC/README.md>)

## Purpose

The primary goal of this PoC is to demonstrate the setup and execution of the ReactJS-based frontend within the OT-Microservices ecosystem. This frontend serves as the visual interface through which users interact with underlying microservices such as Employee, Attendance, and Salary microservices.

## System Requirements

| Component        | Minimum Requirement           |
|------------------|-------------------------------|
| OS               | Ubuntu or other Linux-based   |
| Disk Space       | 8 GB                         |
| RAM              | 2 GB                          |
| Processor        | Single-core                     |
| Instance Type    | t2.small                      |

---

## Prerequisites

| Tool        | Description |
|--------------|-------------|
| **Node.js**  | JavaScript runtime required to install dependencies, build, and run the React.js application.|
| **NPM**      | Node Package Manager used to install and manage project dependencies.|
| **Nginx** | Used to serve the frontend application to users. |
---

## Ports Required

| Port | Used By     | Description                                      |
|------|-------------|--------------------------------------------------|
| 22   | SSH         | Used for secure remote login and VM access.     |
| 80 | Nginx | Used to serve the frontend application over HTTP. |

---

## Installation and Setup

Follow the steps below to manually set up the frontend application on a Linux-based system.

### 1. Update Package Index

Update the system package index before installing the required dependencies.

```bash
sudo apt update
```

### 2. Install Node.js and npm

Node.js is required to run the React.js frontend application, while npm is used to install and manage the project dependencies.

Install Node.js and npm using the following command:

```bash
sudo apt install nodejs npm -y
```

<img width="1351" height="460" alt="node npm install" src="https://github.com/user-attachments/assets/7d4be6e8-511e-41a4-a642-be814646246e" />


### 3. Verify Node.js and npm Installation

Verify that Node.js and npm have been installed successfully by checking their installed versions.

Check the Node.js version:

```bash
node --version
npm --version
```

<img width="905" height="94" alt="node npm version" src="https://github.com/user-attachments/assets/e0bf6efe-e07f-4792-b383-421d2ef48703" />


### 4. Install Nginx

```bash
sudo apt install nginx -y
sudo systemctl status nginx
```


<img width="1260" height="309" alt="nginx status" src="https://github.com/user-attachments/assets/bf5d0226-ffb3-4ef5-a3bf-260e6ee7db4b" />


### 5. Clone the Frontend Repository

Clone the OT-Microservices frontend repository using Git:

```bash
git clone https://github.com/OT-MICROSERVICES/frontend.git
cd frontend
```

<img width="890" height="210" alt="cd frontend" src="https://github.com/user-attachments/assets/d6f37063-5859-4ee1-8135-ab6ad53c24ea" />


### 6. Install Project Dependencies

Install the dependencies required by the frontend application:

```bash
npm install
```

<img width="1363" height="601" alt="npm install" src="https://github.com/user-attachments/assets/49ad9546-f723-4546-b30c-1bf20b49bb4e" />


### 7. Start the Frontend Application

Start the React.js frontend application using NPM:

```bash
export NODE_OPTIONS=--openssl-legacy-provider
npm start
http://<PUBLIC-IP>:3000
```

<img width="1359" height="723" alt="frontend browser port 3000" src="https://github.com/user-attachments/assets/e732f513-dbd3-4b38-821d-2168210613f2" />


### 8. Configure Nginx as a Reverse Proxy

Configure Nginx to forward incoming HTTP requests on port `80` to the React.js application running on port `3000`.

Create a new Nginx configuration file:

```bash
sudo nano /etc/nginx/sites-available/frontend

server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://127.0.0.1:3000;

        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

<img width="1249" height="297" alt="add nginx configuration" src="https://github.com/user-attachments/assets/b710d4d8-ab4a-4bcc-81f9-a7aebc3c5e20" />

### 9. Enable the Nginx Configuration

Create a symbolic link to enable the frontend Nginx configuration:

```bash
sudo ln -s /etc/nginx/sites-available/frontend /etc/nginx/sites-enabled/frontend
sudo rm /etc/nginx/sites-enabled/default
```
<img width="919" height="69" alt="symbolic link" src="https://github.com/user-attachments/assets/cffde6b9-e7b9-48aa-aeb0-921105cc0cc2" />


### 10. Validate Nginx Configuration

Test the Nginx configuration before reloading the Nginx service:

```bash
sudo nginx -t
```

<img width="885" height="77" alt="validate nginx" src="https://github.com/user-attachments/assets/b771213d-2321-45c3-9afd-9d9c7cf2cdec" />


### 11. Reload & Check Status Nginx

Reload Nginx to apply the updated frontend configuration:

```bash
sudo systemctl reload nginx
sudo systemctl status nginx
```
<img width="1265" height="399" alt="relode nginx" src="https://github.com/user-attachments/assets/27d8a85b-615b-4625-a242-e6fade9558d7" />

### 12. Validate Frontend Through Nginx

Verify that Nginx is listening on port `80` and forwarding requests to the React.js application.

Run the following commands:

```bash
sudo ss -lntp | grep ':80'
curl http://127.0.0.1
```

<img width="1363" height="447" alt="validate nginx port and curl" src="https://github.com/user-attachments/assets/b414ccb8-c6d6-4ec1-a5ca-f6f50ca67565" />

### 13. Access Frontend Through Public IP

Open the following URL in a web browser:

```text
http://<PUBLIC-IP>
```

<img width="1361" height="723" alt="frontend browser through nginx" src="https://github.com/user-attachments/assets/0cf559b3-9e5d-43d4-b52b-855abb875da4" />

### 14. Configure Frontend as a System Service

Create a systemd service file for the frontend application:

```bash
sudo nano /etc/systemd/system/frontend.service
```
<img width="1161" height="319" alt="systemd service code" src="https://github.com/user-attachments/assets/0c44321c-c112-4008-9b29-0006f02a6971" />



### 15. Reload and Start the Frontend Service

Reload the systemd daemon to apply the new service configuration:

```bash
sudo systemctl daemon-reload
sudo systemctl enable frontend
sudo systemctl start frontend
sudo systemctl status frontend
```

<img width="1015" height="476" alt="status systemd service" src="https://github.com/user-attachments/assets/89c8dbc6-2397-4a3f-a907-6e3ac36a087e" />

---


## Conclusion

Through this PoC, we explored the complete manual setup and deployment process of the OT-Microservices frontend application on a Linux-based server.

We explored the required system setup, Node.js and NPM installation, project dependency management, frontend application execution, and Nginx configuration.

We also configured the frontend application as a systemd service for automatic restart and startup after a system reboot. Nginx was configured as a reverse proxy to forward requests from port `80` to the React.js application running on port `3000`.

Finally, we validated the complete setup using command-line checks and browser access through the server's public IP address.

This PoC provides an understanding of how the frontend application can be manually configured, managed, and accessed on a Linux-based server without using Docker or Kubernetes.

---
## Contact

| Name            | Email Address                                 |
|-----------------|-----------------------------------------------|
| Mansoor Alam | mansoor.alam.snaatak@mygurukulam.co        |

---
## References

| **Title**                              | **Link**                                                                                        |
|----------------------------------------|-------------------------------------------------------------------------------------------------|
| Frontend Detaild Documentation                  | [Visit](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-72-MAQBOOL/Documentation/OT_MS_Understanding/Frontend/DOC/README.md) |
| Node.js Official Docs                  | [Visit](https://nodejs.org/en/download/package-manager) |
| NPM Documentation               | [Visit](https://docs.npmjs.com/)                                 |
| React Official Documentation               | [Visit](https://reactjs.org/docs/getting-started.html)                                 |

---
