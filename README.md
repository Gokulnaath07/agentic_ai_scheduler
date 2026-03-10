# Autonomous Agentic Orchestration & Scheduled RAG System
---
> **A high-reliability, stateful autonomous agent designed to solve and document 75 technical coding challenges daily. This system leverages Gemini 2.0 for reasoning and a distributed cloud architecture (Hugging Face + Supabase) to ensure 99.9% execution uptime.**
---
## 🚀 The Pipeline
> **The system executes a complex multi-stage workflow every morning at 08:00 AM EST:**

**Ingestion:** Fetches target problem metadata from Google Sheets.

**Reasoning:** Gemini 2.0 processes the logic, generates optimized solutions, and formats technical explanations.

**Persistence:** Programmatically generates a formatted Microsoft Word document and syncs it to a specific Google Drive directory.

**Handshake:** Triggers a Twilio API call to send a real-time SMS notification confirming the full-chain completion.

## 🛠️ Technical Architecture & Infrastructure
**1. Stateful Orchestration**
Unlike standard "stateless" automations, this system decouples Compute (Hugging Face Docker Space) from State (Supabase PostgreSQL). This ensures that execution logs, binary data, and encryption credentials persist even across container restarts or deployments.

**2. Performance Optimization**
Session-Based Pooling: Migrated from Transaction Mode to Session Mode (Port 5432) to resolve I/O latency bottlenecks during heavy LLM processing.

IPv4 Tunneling: Engineered a connection strategy to bridge IPv6-only database endpoints with IPv4-only container proxies.

Log Pruning: Implemented a 72-hour automated data retention policy to mitigate PostgreSQL index bloat and maintain query performance.

## 📦 Deployment & Environment
The project is deployed using a custom Debian-based Dockerfile optimized for n8n v2.x.
```
Prerequisites
Docker (Node:20-alpine / Debian Bullseye)

Python 3.10+ (Managed via internal venv)

PostgreSQL 15+ (Supabase recommended)
```
---

## Core Environment Variables
**Bash**
---
```
# Database Configuration
DB_TYPE=postgresdb
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_SSL_REJECT_UNAUTHORIZED=false

# System Configuration
GENERIC_TIMEZONE=America/New_York
N8N_PYTHON_BINARY=/home/node/.n8n/python_env/bin/python
EXECUTIONS_DATA_MAX_AGE=72
```
---
## 🧠 Key Challenges & Solutions
The "Silent Success" Bug: Discovered that n8n v2.x would mark nodes as "Success" even if the JSON payload was empty. Resolved by implementing "Always Output Data" guards and data-validation nodes before the Twilio handshake.

Python Task Runner Security: n8n v2.8+ requires a managed virtual environment for Python nodes. I engineered a custom Docker build that pre-configures a secure /home/node/.n8n/python_env during the image build phase.

## 🛠️ Tech Stack
```
Orchestration: n8n

AI/LLM: Gemini 2.0

Database: Supabase (PostgreSQL)

Infrastructure: Docker, Hugging Face Spaces

Communication: Twilio API, Google Workspace APIs

OS: Linux (Debian Administration)
```
> **Developed by Gokulnaath Govindaraj**
