<div align="center">

# POC - Setup and Run Redis for Microservices (Standalone & HA Replication)

<img width="482" height="175" alt="image" src="https://github.com/user-attachments/assets/928acf4a-a766-4bf0-bf77-a588935bb5b4" />


</div>

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Palak Upadhayay | 08-09-2026 | v1.0 | Palak Upadhayay | 18-09-2026 | Aryan Mishra | 
| Palak Upadhayay | 08-09-2026 | v1.1 | Palak Upadhayay | 19-09-2026 | Aryan Mishra | Aditya Raj Singh | - |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Objective](#2-objective)
3. [Prerequisites](#3-prerequisites)
4. [SCRUM-74 Reference Documentation](#4-scrum-74-reference-documentation)
5. [Project Setup](#5-project-setup)
6. [Implementation / Execution (POC)](#6-implementation--execution-poc)
7. [Validation](#7-validation)
8. [Observations](#8-observations)
9. [Troubleshooting](#9-troubleshooting)
10. [Best Practices](#10-best-practices)
11. [Conclusion](#11-conclusion)
12. [Contact Information](#12-contact-information)
13. [References](#13-references)

---

# 1. Introduction

Redis (Remote Dictionary Server) is an open-source, in-memory data structure store used as a database, cache, and message broker. This POC validates standalone deployment, in-memory caching with TTL auto-expiration, and local High Availability (HA) Master-Replica replication on OS .

---

# 2. Objective

* Install and verify standalone Redis server daemon on Ubuntu.
* Execute in-memory CRUD operations, key inspection, and TTL cache auto-expiration.
* Configure multi-instance Master-Replica architecture (Master: 6379, Replica: 6380) with direct configuration settings.
* Validate real-time asynchronous data replication and enforce read-only replica access for high availability.

---

# 3. Prerequisites

| **Prerequisite** | **Details** | **Verification Command** |
| :--- | :--- | :--- |
| **Operating System** | Linux (Ubuntu / RHEL / WSL2) | `cat /etc/os-release` |
| **Redis Server & CLI** | In-memory key-value database and CLI | `redis-server --version && redis-cli ping` |
| **SSH Client** | OpenSSH Client for repository authentication | `ssh -V` |
| **Permissions** | Root / Sudo privileges | `sudo -v` |

```bash
redis-server --version && redis-cli ping
```

---

## 4. Reference Documentation

For architectural design details and theoretical background on Redis microservices integration, refer to the Redis documentation link below:

* **GitHub Reference:** [SCRUM-74 Redis Architecture & Design Documentation](https://github.com/SnaatakKubeClan/Sprint-1/blob/SCRUM-74-PRIYANSHU/Documentation/OT_MS_Understanding/Redis/DOC/README.md)
---

# 5. Project Setup

## 5.1 Git Branch Setup
```bash
git checkout -b SCRUM-73-PALAK
```

## 5.2 Install & Verify Redis Service
```bash
sudo apt update -y && sudo apt install redis-server -y
sudo service redis-server start
sudo service redis-server status
```

<img width="710" height="530" alt="01_redis_status" src="https://github.com/user-attachments/assets/579af1e8-ce90-4cf7-b49c-df2949b34fee" />


## 5.3 Multi-Instance Configuration (HA Setup)

To configure the Replica instance, create a dedicated configuration file and append the required replica settings directly:

1. **Copy the master configuration file to create the replica configuration:**
   ```bash
   sudo cp /etc/redis/redis.conf /etc/redis/redis-replica.conf
   ```

2. **Create a dedicated data directory for the replica instance:**
   ```bash
   sudo mkdir -p /var/lib/redis-replica && sudo chown -R redis:redis /var/lib/redis-replica
   ```

3. **Open `/etc/redis/redis-replica.conf` using a text editor (e.g. `sudo nano /etc/redis/redis-replica.conf`) or append the replica parameters directly:**
   ```bash
   cat <<EOF | sudo tee -a /etc/redis/redis-replica.conf
   port 6380
   pidfile /var/run/redis/redis-server-replica.pid
   logfile /var/log/redis/redis-server-replica.log
   dir /var/lib/redis-replica
   replicaof 127.0.0.1 6379
   EOF
   ```

**Parameter Explanation Table:**

| **Parameter** | **Value** | **Purpose** |
| :--- | :--- | :--- |
| **port** | `6380` | Runs the replica instance on port 6380 to avoid collision with master (6379). |
| **pidfile** | `/var/run/redis/redis-server-replica.pid` | Process ID tracking file for the replica service. |
| **logfile** | `/var/log/redis/redis-server-replica.log` | Dedicated log file for replica execution history. |
| **dir** | `/var/lib/redis-replica` | Storage directory for replica data snapshots. |
| **replicaof** | `127.0.0.1 6379` | Configures this instance as a slave/replica of master running on port 6379. |

---

# 6. Implementation / Execution (POC)

## 6.1 Standalone CRUD & In-Memory Operations
```bash
redis-cli ping
redis-cli SET poc:user "Palak"
redis-cli GET poc:user
redis-cli SETEX otp:login 60 "852963"
redis-cli TTL otp:login
redis-cli DEL poc:user
```

<img width="522" height="241" alt="02_standalone_crud" src="https://github.com/user-attachments/assets/4fb035c1-50cb-4021-9fa0-b7463cf1a602" />


## 6.2 Start Replica Daemon (Port 6380)
```bash
sudo redis-server /etc/redis/redis-replica.conf --daemonize yes
```

## 6.3 Verify Dual Running Instances
```bash
ps aux | grep redis-server
```

<img width="690" height="106" alt="03_dual_instances_running" src="https://github.com/user-attachments/assets/c063e889-1222-4e64-bbee-393a502db64d" />


## 6.4 Inspect Master & Replica Replication Status
```bash
redis-cli -p 6379 INFO replication
redis-cli -p 6380 INFO replication
```

<img width="682" height="517" alt="04_replication_info" src="https://github.com/user-attachments/assets/dba847b3-f446-496d-a34c-038473b64092" />


---

# 7. Validation

## 7.1 Real-Time Data Synchronization & Read-Only Check
```bash
# Write to Master (6379)
redis-cli -p 6379 SET ha_check "Replication Working Perfectly"

# Read from Replica (6380)
redis-cli -p 6380 GET ha_check

# Verify Read-Only enforcement on Replica
redis-cli -p 6380 SET test_write "Fail"
```

<img width="719" height="327" alt="05_data_replication_proof" src="https://github.com/user-attachments/assets/9df9f537-f4ce-4479-bd50-399f318f4ea2" />


## 7.2 Validation Checklist

| **Check** | **Expected Result** | **Actual Result** |
| :--- | :--- | :--- |
| **Ping Health Check** | `PONG` response | PASS |
| **In-Memory Storage** | Value retrieved in microseconds | PASS |
| **TTL Cache Expiration** | Key expires automatically after countdown | PASS |
| **Key Eviction (`DEL`)** | Key deleted; subsequent `GET` returns `(nil)` | PASS |
| **Dual Daemon Execution** | Independent instances on ports 6379 and 6380 | PASS |
| **Master-Replica Link** | Master shows `connected_slaves:1`, Replica `up` | PASS |
| **Real-Time Data Sync** | Master write immediately readable on Replica | PASS |
| **Read-Only Enforcement** | Write on replica rejected with `READONLY` error | PASS |

---

# 8. Observations

* **Ultra-Low Latency:** In-memory key-value operations execute in sub-millisecond time.
* **Automated Expiry:** `TTL` eliminates manual cache eviction logic.
* **Seamless Async Replication:** Zero data lag between Master (6379) and Replica (6380).
* **Data Safety:** Replica strictly enforces read-only state, protecting against data skew.

---

# 9. Troubleshooting

| **Issue** | **Cause** | **Solution** |
| :--- | :--- | :--- |
| `Connection refused` | Redis daemon inactive. | Run `sudo service redis-server start`. |
| `READONLY error` | Writing directly to replica. | Direct write commands to Master (`port 6379`). |
| `Address already in use` | Port collision on binding. | Change port in config or terminate conflicting process. |
| `Cannot allocate memory` | Linux overcommit policy restricted. | Set `sysctl vm.overcommit_memory=1` in `/etc/sysctl.conf`. |

---

# 10. Best Practices

- **Enable Overcommit:** Set `vm.overcommit_memory = 1` in `/etc/sysctl.conf` to prevent background snapshot save failures.
- **Set Maxmemory Policy:** Configure `maxmemory` with `allkeys-lru` eviction policy to prevent Out-Of-Memory (OOM) crashes.
- **Protect with Password Authentication:** Use `requirepass` and `masterauth` in production to enforce strong password authentication.
- **Isolate Instance Resources:** Ensure separate `dir`, `pidfile`, and `logfile` paths for each Redis instance to avoid resource collision.
- **Hybrid Persistence:** Combine RDB snapshots with Append-Only File (AOF) logs for maximum disaster recovery and data durability.

---

# 11. Conclusion

This Proof of Concept (POC) successfully demonstrates standalone Redis deployment, in-memory caching with TTL auto-expiration, and multi-instance Master-Replica replication on Ubuntu .
By isolating configuration parameters and enforcing read-only access on the replica instance, the setup achieves zero-lag data synchronization and protects against data corruption.
Overall, the implementation provides a high-performance, resilient, and enterprise-ready Redis foundation for microservices caching and high availability.

---

# 12. Contact Information

| **Name** | **Email Address** |
| :--- | :--- |
| Palak Upadhyay | [palak.upadhyay.snaatak@mygurukulam.co](mailto:palak.upadhyay.snaatak@mygurukulam.co) |

---

# 13. References

| **Resource** | **Link** |
| :--- | :--- |
| Redis Documentation | https://redis.io/docs/ |
| Redis Replication Guide | https://redis.io/docs/management/replication/ |
| Redis Configuration Guide | https://redis.io/docs/management/config/ |
