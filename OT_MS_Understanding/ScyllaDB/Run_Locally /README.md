# ScyllaDB Setup on AWS EC2

<img width="496" height="171" alt="image" src="https://github.com/user-attachments/assets/05bbc244-fb23-49bd-9bce-952d59d06528" />

---

# Author Table

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ---------- | -------------- | ----------- | ------------------- | ------------------ | --------------- | --------------- | --------------- |
| Shivam Tomar | 12-09-2026 | v1.0 | Shivam Tomar | 20-09-2026 | Ishaan | Anuj Jain | Piyush Upadhyay |

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [ScyllaDB Architecture](#3-scylladb-architecture)
4. [AWS EC2 Configuration](#4-aws-ec2-configuration)
5. [Install ScyllaDB](#5-install-scylladb)
6. [Run ScyllaDB Setup](#6-run-scylladb-setup)
7. [Start and Verify ScyllaDB](#7-start-and-verify-scylladb)
   * [Service Status](#service-status)
   * [Node Status](#node-status)
   * [CQL Connectivity](#cql-connectivity)
8. [Conclusion](#8-conclusion)
9. [Contact Information](#9-contact-information)
10. [References](#10-references)

---

# 1. Introduction

This document details the single-node deployment and verification of ScyllaDB Open Source (v2026.3.0) on an AWS EC2 instance running Ubuntu 24.04 LTS. It covers the installation using the official ScyllaDB web installer, system setup using `scylla_setup`, service execution, and CQL connectivity validation.

---

# 2. Prerequisites

| **Requirement** | **Prerequisite** |
| --- | --- |
| Architecture | 64-bit Linux on x86_64 or AArch64 |
| CPU | Modern Intel/AMD or supported AWS Graviton processors |
| Memory | Minimum 4 GB for production workloads |
| Storage | SSD storage is recommended |
| Network | High-speed network recommended for high-performance workloads |
| Operating System | Supported 64-bit Linux distribution such as Ubuntu, Debian, CentOS, or RHEL |
| Access | Root or `sudo` privileges |
| Network Ports | Required ScyllaDB communication ports must be accessible |

---

# 3. ScyllaDB Architecture

ScyllaDB uses a peer-to-peer, shared-nothing distributed architecture where all nodes are equal peers. Each node operates on a shard-per-core design, with dedicated CPU cores and local SSTable storage, delivering high throughput and low-latency database operations.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/8c1f43d3-0694-436c-94e0-cd45d8863b4c" />


---

# 4. AWS EC2 Configuration

| **Parameter** | **Specification** |
| ------------- | ----------------- |
| Cloud Provider | Amazon Web Services (AWS) |
| Region | `ap-south-1` (Mumbai) |
| Instance Type | `c7i-flex.large` |
| Compute & Memory | 2 vCPUs, ~4 GiB RAM (3.7 GiB usable) |
| Storage | 30 GB gp3 EBS Volume |
| Operating System | Ubuntu Server 24.04 LTS |
| Architecture | x86_64 |
| Network / Security Group | Port 22 (SSH) restricted to admin IP; Port 9042 (CQL) local only |

---

# 5. Install ScyllaDB

Install the official ScyllaDB repository and package suite using the web installer:

```bash
curl -sSf get.scylladb.com/server | sudo bash
```

<img width="771" height="73" alt="install-1" src="https://github.com/user-attachments/assets/155f2e31-36e8-4e7d-9689-b56a24793fe9" />

Verify the installed binary version:

```bash
scylla --version
```

<img width="772" height="65" alt="install done" src="https://github.com/user-attachments/assets/eaa62867-40f1-4193-9745-3d7d83c47f66" />

---

# 6. Run ScyllaDB Setup

ScyllaDB provides the `scylla_setup` utility to optimize host kernel settings and hardware configurations for the database.

```bash
sudo scylla_setup
```

<img width="773" height="131" alt="scylladb-initial-setup" src="https://github.com/user-attachments/assets/c1fe52d9-596e-4e39-8eb0-e410449c1c99" />

### Interactive configuration selected:

| **Configuration / Prompt** | **Selection** | **Purpose** |
| -------------------------- | ------------- | ----------- |
| Automatically start ScyllaDB service | Yes | Enables ScyllaDB to start automatically with the system |
| Check for newer ScyllaDB version | No | Version is already selected for this POC |
| Configure NTP | Yes | Keeps system time synchronized |
| Configure RAID/XFS | No | Not required for this single-instance POC |
| Configure coredumps | No | Not required for this POC |
| Configure system-wide ScyllaDB settings | No | Default configuration is sufficient for this POC |
| Configure I/O tuning / IOTune | No | I/O tuning was configured separately using `scylla_io_setup` |
| Configure CPU performance governor | No | Not required for this POC |
| Configure fstrim | No | Not required for this POC |
| Configure ScyllaDB-only mode | No | The EC2 instance is not dedicated exclusively to ScyllaDB |
| Configure remote rsyslog | No | Remote logging is not required for this POC |
| Configure large-node LimitNOFILES | No | Not required for this small POC instance |

---

# 7. Start and Verify ScyllaDB

Start the ScyllaDB service daemon and verify its active status and cluster state:

```bash
sudo systemctl start scylla-server
sudo systemctl status scylla-server --no-pager -l
```

<img width="778" height="365" alt="scylladb-running" src="https://github.com/user-attachments/assets/eae4fcbf-49ac-46da-bda4-363011f3158d" />

### Node Status

Check the operational status of the single-node cluster using `nodetool`:

```bash
nodetool status
```

<img width="773" height="191" alt="nodetool status" src="https://github.com/user-attachments/assets/fa5d3579-8f61-4343-83a9-61ac2182eab8" />

*Status Explanation: `UN` indicates Up and Normal, confirming the single ScyllaDB node is healthy and operational.*

### CQL Connectivity

Connect to the database instance locally using the CQL shell:

```bash
cqlsh
```

<img width="771" height="95" alt="image" src="https://github.com/user-attachments/assets/3a47b48d-5f16-4abc-81ce-06e07c623576" />

List existing database keyspaces:

```sql
DESCRIBE KEYSPACES;
```

<img width="773" height="112" alt="image" src="https://github.com/user-attachments/assets/05b214a8-185f-4382-8aca-8e5eda59577c" />

---

# 8. Conclusion

ScyllaDB 2026.3.0 was successfully deployed on a single AWS EC2 instance. The service is active and serving requests, the node status is Up/Normal (`UN`), and local CQL access was verified on port 9042.

---

# 9. Contact Information

| **Name** | **Email** |
| -------- | --------- |
| Shivam Tomar | [shivam.tomar.snaatak@mygurukulam.co](mailto:shivam.tomar.snaatak@mygurukulam.co) |

---

# 10. References

| **Resource / Standard** | **Official Reference Link** |
| ----------------------- | --------------------------- |
| ScyllaDB Official Documentation | [docs.scylladb.com](https://docs.scylladb.com/) |
| ScyllaDB Installation Guide (Ubuntu) | [docs.scylladb.com/manual/install/ubuntu](https://docs.scylladb.com/manual/install/ubuntu/) |
| ScyllaDB System Configuration (`scylla_setup`) | [docs.scylladb.com/manual/system-configuration/scylla-setup](https://docs.scylladb.com/manual/system-configuration/scylla-setup/) |
