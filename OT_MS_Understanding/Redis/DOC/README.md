# Redis documentation

| **Author** | **Created On** | **Version** | **Last Updated By** | **Last Edited On** | **L0 Reviewer** | **L1 Reviewer** | **L2 Reviewer** |
| ------ | ---------- | ------- | ---------------- | -------------- | ---------- | ------- | ------ |
| Priyanshu | 10-09-2026 | 1.0 | Priyanshu | 10-09-2026 | Aryan Mishra | Aditya | <L2 Reviewer> |


---

## Introduction

Redis (REmote DIctionary Server) is an open-source, in-memory data structure store used as a database, cache, message broker, and streaming engine. Originally released in 2009, Redis is written in C and stores data as key-value pairs entirely in memory, giving it sub-millisecond read/write latency, with optional persistence to disk so data survives restarts.

---

## Purposes

Redis is typically introduced into an architecture to solve one or more of the following problems:

- **Caching layer** in front of a slower primary database (SQL/NoSQL), reducing latency and offloading read traffic.
- **Session store** for web applications, holding user session state outside the application server.
- **Real-time leaderboards / ranking systems**, using sorted sets for fast score-based ordering.
- **Rate limiting and counters**, using Redis's atomic increment operations.

---

## Key features

- **In-memory storage** delivering sub-millisecond latency for reads and writes.
- **Rich data structures**: strings, hashes, lists, sets, sorted sets, bitmaps, HyperLogLogs, streams, and geospatial indexes.
- **Persistence options**: RDB point-in-time snapshots, AOF (append-only file) command logging, or both combined.
- **Built-in replication** (primary–replica) and **Redis Sentinel** for automatic monitoring and failover.
- **Redis Cluster** for horizontal scaling and automatic data sharding across multiple nodes.
- **Pub/Sub messaging** for real-time event broadcast between clients.

---

## Getting Started

---

### Pre-requisites

Since March 2024, Redis has been tri-licensed; which license applies depends on how it's used:

| License Type | Description | Commercial Use | Open Source |
| ------------ | ------------ | --------------- | ------------ |
| AGPLv3 | GNU Affero General Public License v3 — the OSI-approved open source option, available since Redis 8.0 (May 2025) | Yes, subject to AGPL's network-use copyleft (source disclosure) obligations | Yes |
| RSALv2 | Redis Source Available License v2 — source-available; restricts offering Redis as a competing managed database service | Restricted | No |
| SSPLv1 | Server Side Public License v1 — source-available; requires open-sourcing the full surrounding service stack if Redis is offered as a service | Restricted | No |

---

### Software Overview

| Software | Version |
| -------- | ------- |
| Redis | 8.8.0 (latest stable release) |

---

### System Requirement

| Requirement | Minimum Recommendation |
| ----------------------- | ---------------------------- |
| Processor/Instance Type | 2 vCPU / t3.medium or equivalent (scale up with throughput) |
| RAM | 2 GB minimum; production sizing should hold the full working dataset in memory plus headroom for fork-based persistence (RDB/AOF rewrite) |
| Disk Space | 10 GB or higher; size storage according to RDB snapshot and/or AOF log retention requirements |
| OS Required | Linux (recommended for production); macOS supported for development and testing; no official native Windows support |

---

### Important Ports

---

| Ports | Description |
| ----- | ------------ |
| 6379 | Default Redis server port used by clients (`redis-cli`, application drivers) to connect. |
| 22 | Port 22 is used to establish an SSH connection to the host running Redis for administration. |

---

## Dependencies

### Run-time Dependency

Mention the Run-time dependency to installed and also specify the version

| Run-time Dependency | Version | Description |
| -------------------- | ------- | ------------ |
| systemd | Any recent release | Manages the `redis-server` service lifecycle (start/stop/enable) on most Linux distributions. |
| OpenSSL | Any suitable version | Required only if TLS is enabled for encrypted client-server connections. |

---

### Other Dependency

Mention the Other dependency to installed and also specify the version

| Other Dependency | Version | Description |
| ------------------ | ------- | ------------ |
| redis-cli | Bundled with Redis | Command-line client used to interact with a running Redis server. |
| redis-sentinel | Bundled with Redis | Sentinel binary used for high-availability monitoring and automatic failover. |

---

## How to Setup/Install Redis

---

### Step-by-step Installation Instruction

The steps below install Redis on Ubuntu from the official Redis APT repository (recommended, as distro repos often carry older versions):

```bash
# 1. Install prerequisites
sudo apt-get update
sudo apt-get install -y curl gnupg lsb-release

# 2. Add the official Redis APT repository and its signing key
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | \
  sudo tee /etc/apt/sources.list.d/redis.list

# 3. Install Redis
sudo apt-get update
sudo apt-get install -y redis

# 4. Start the service and enable it on boot
sudo systemctl enable --now redis-server

# 5. Verify it's running
redis-cli ping
```

Expected output:

```text
PONG
```

---

### Configuration

Configuration refers to the process of setting up and customizing a program or system to work according to specific requirements. Redis's main configuration file is `/etc/redis/redis.conf`. Common settings to review before production use:

```conf
# Bind to specific interfaces only (avoid 0.0.0.0 in production)
bind 127.0.0.1 -::1

# Require a password for client connections
requirepass <your-strong-password>

# Cap memory usage and define eviction behaviour once the cap is hit
maxmemory 2gb
maxmemory-policy allkeys-lru

# Enable AOF persistence in addition to RDB snapshots
appendonly yes
appendfsync everysec

# RDB snapshot schedule (save <seconds> <changes>)
save 900 1
save 300 10
save 60 10000
```

After editing, apply the changes:

```bash
sudo systemctl restart redis-server
```

---

## Maintenance

Follow these commands:

```bash
# For Update
sudo apt-get update

# To upgrade software version
sudo apt-get install --only-upgrade redis

# For restart
sudo systemctl restart redis-server
```

---

## Monitoring

After installation, ensure Redis is in a working state and keep an eye on it over time:

```bash
redis-cli ping                 # Should return PONG
redis-cli INFO                 # Server, memory, replication, and keyspace stats
redis-cli --latency            # Live latency sampling
redis-cli --bigkeys            # Scan for unusually large keys
redis-cli SLOWLOG GET 10       # Recent slow commands
sudo systemctl status redis-server
```

Log files are located at `/var/log/redis/redis-server.log` by default — check them first when investigating unexpected behaviour.

---

## Disaster Recovery

Redis's two persistence mechanisms are the foundation of disaster recovery:

- **RDB snapshots** (`dump.rdb`) — compact, point-in-time backups suitable for scheduled off-host backup.
- **AOF** (`appendonly.aof`) — a log of write operations, replayed on startup; with `appendfsync everysec` this bounds potential data loss to roughly one second.

---

## High Availability

- **Redis Sentinel** continuously monitors the primary and its replicas, and automatically promotes a replica to primary if the current primary becomes unreachable, while also acting as a service-discovery layer for clients. Run an odd number of Sentinel processes (3 or more) spread across separate hosts/AZs to maintain quorum during a failure.
- **Redis Cluster** automatically shards data across multiple primary nodes (each with its own replicas), providing both horizontal scaling and resilience without a single point of failure — if a primary shard fails, its replica is promoted automatically.
- For people that prefer not to operate HA themselves, managed alternatives include AWS ElastiCache for Redis / MemoryDB, Azure Cache for Redis, and Google Cloud Memorystore for Redis.

---

## Troubleshooting

Mention all the common issues that you encountered while performing setup of the Application.

| Issue | Cause | Solution |
| ----- | ----- | -------- |
| `Could not connect to Redis` / connection refused | Redis isn't running, or is bound to a different interface than expected | Check `sudo systemctl status redis-server`; verify `bind` and `port` in `redis.conf` |
| `(error) NOAUTH Authentication required` | `requirepass` is set but the client didn't authenticate | Connect with `redis-cli -a <password>`, or run `AUTH <password>` after connecting |
| Data lost after a restart | Persistence disabled or misconfigured (`save ""`, `appendonly no`) | Enable RDB `save` points and/or set `appendonly yes` in `redis.conf` |
| `MISCONF Redis is configured to save RDB snapshots, but...` | Redis can't currently write to disk (permissions or disk full) | Check disk space and file permissions on the configured `dir`/`dbfilename` path |

---

## FAQs

- **Is Redis free to use?**

  * Yes. Since Redis 8.0 (May 2025), Redis is available under the OSI-approved AGPLv3 license (in addition to the source-available RSALv2 and SSPLv1 options), so self-hosted use is free.

- **Can Redis persist data to disk?**

  * Yes, via RDB point-in-time snapshots, AOF command logging, or both combined for stronger durability.

- **Can I deploy Redis on all cloud platforms?**

  * Yes — it can be self-managed on any VM or container on any cloud, or consumed as a managed service (AWS ElastiCache/MemoryDB, Azure Cache for Redis, GCP Memorystore).

---

## Contact Information

| **Name** | **Team** | **Email** |
| -------- | --------- | -------- |
| Priyanshu | Kubeclan | [priyanshu.banwala.snaatak@mygurukulam.co](mailto:priyanshu.banwala.snaatak@mygurukulam.co) |

---

## References

| Links | Descriptions |
| ----- | ------------ |
| <https://redis.io/docs/latest/> | Official Redis documentation covering commands, data types, and operations. |
| <https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/install-redis-on-linux/> | Official guide for installing Redis on Linux distributions. |
| <https://redis.io/legal/licenses/> | Official page detailing Redis's current tri-license model (AGPLv3, RSALv2, SSPLv1). |
| <https://github.com/redis/redis> | Redis source code repository. |
