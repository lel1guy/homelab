# homelab

> Self-hosted infrastructure running on KAIDO-01 — Fedora Linux server with Docker Compose stacks, PostgreSQL, Tailscale mesh VPN, and automated operations.

Built and maintained by [@lel1guy](https://github.com/lel1guy) in Portugal.

---

## Overview

A production-grade single-server homelab powering AI agent operations, workflow automation, and personal knowledge management. The server runs 24/7 with Docker Compose orchestration, native PostgreSQL, and a Tailscale mesh VPN connecting devices across the local network.

All services are documented with their purpose, ports, and dependencies. The entire stack is recoverable from Docker Compose files and git-tracked configuration.

---

## Hardware

| Component | Spec |
|-----------|------|
| **Machine** | Toshiba Tecra Z40-C |
| **CPU** | Intel Core i5-6200U (2C/4T, 2.3 GHz) |
| **RAM** | 8 GB DDR4 |
| **Storage** | 256 GB SSD |
| **OS** | Fedora 44 (kernel 6.19.10-300) |

---

## Network

| Interface | Address | Purpose |
|-----------|---------|---------|
| LAN | 192.168.1.x/24 | Local network access |
| Tailscale | 100.x.x.x | Mesh VPN — encrypted peer-to-peer |

**Kodo Naming System:** The infrastructure follows a Japanese-themed naming convention (Kakurega Sector). `KAIDO` (街道, "main highway") is the compute category.

**Connected devices via Tailscale:**
- KAIDO-01 — this server (Linux)
- Mobile device (Android)
- Workstation (Windows)

---

## Running Services

### Docker Compose Stacks

| Stack | Services | Ports | Purpose |
|-------|----------|-------|---------|
| **n8n** | n8nio/n8n | 5678 | Workflow automation — connects APIs, processes data, sends notifications |
| **Honcho** | Honcho API, Honcho Deriver, Redis 8.2, pgvector (pg15) | 8000, 6379, 5433 | AI memory layer with vector embeddings for persistent agent context |
| **Open Notebook** | Open Notebook, SurrealDB v2 | 8502, 5055, 8001 | AI research notebook for document analysis and agent collaboration |

All containers run with custom Docker bridge networks, health checks, restart policies, and resource limits defined in per-stack `docker-compose.yml` files.

### Native Services

| Service | Details |
|---------|---------|
| **PostgreSQL 18** | 7 databases including FCC certification projects and application data |
| **Docker 29.5** | With Docker Compose plugin and containerd runtime |
| **Tailscale** | Mesh VPN — encrypted peer-to-peer connections, ACL-based access control |
| **firewalld** | Dynamic firewall — per-service port management, zone-based policies |
| **Hermes Agent** | AI agent with 21 automated cron pipelines — see [lel1guy/hermes-homelab](https://github.com/lel1guy/hermes-homelab) |

---

## Architecture

```
+-------------------------------------------------------+
|                     KAIDO-01                           |
|                    Fedora 44                           |
|                                                       |
|  +----------+  +------------+  +--------------------+ |
|  |   n8n    |  |   Honcho   |  |  Open Notebook     | |
|  |  :5678   |  |  :8000     |  |  :8502 / :5055     | |
|  |          |  |  +Redis    |  |  +SurrealDB        | |
|  |          |  |  +pgvector |  |  :8001             | |
|  +----------+  +------------+  +--------------------+ |
|                                                       |
|  +--------------------------------------------------+ |
|  |           Native Services                         | |
|  |  PostgreSQL 18 | Docker 29.5 | Hermes Agent      | |
|  |  SSH | firewalld | cron | SMART monitoring       | |
|  +--------------------------------------------------+ |
|                                                       |
|  +--------------------------------------------------+ |
|  |           Tailscale Mesh VPN                      | |
|  |  KAIDO-01 <----> Mobile <----> Workstation       | |
|  +--------------------------------------------------+ |
+-------------------------------------------------------+
```

---

## Automation

The Hermes Agent runs 21 scheduled cron jobs covering:

| Category | Jobs | Examples |
|----------|------|---------|
| **Knowledge Management** | 5 | Session export, inbox processing, wiki ingest, task rebuilding, stale checks |
| **Notifications** | 4 | Morning briefing, stale item reminders, daily tech quiz, RSS feeds |
| **Content Generation** | 2 | Daily study podcast (edge-tts), weekly vault stats |
| **System Health** | 3 | System status snapshots, Syncthing events, uptime monitoring |
| **Backup & Sync** | 2 | Vault backup, logbook generation |
| **Study Tracking** | 2 | Study plan reminders, weekly progress review |
| **Session Processing** | 3 | Nightly note processing, daily knowledge extraction, daily note |

All automation scripts are Python and Bash, version-tracked, and configurable via environment variables.

---

## Repositories on Device

| Repo | Purpose |
|------|---------|
| `vault` | Obsidian knowledge vault — 350+ notes |
| `honcho` | AI memory server with pgvector |
| `hermes-webui` | Web interface for Hermes Agent |

---

## Why This Matters

This isn't a tutorial-following project. Every service here was chosen, deployed, and is actively maintained to solve real problems — knowledge management, workflow automation, AI agent operations. The infrastructure has evolved organically over a year of daily use: configuring Docker networks, troubleshooting PostgreSQL, tuning firewall rules, debugging container health checks, and recovering from failures.

It demonstrates practical Linux system administration, container orchestration, and automation skills that translate directly to IT support, NOC, and junior DevOps roles.
