# 🚀 Deployment Guide — WeightGuard

> **Project:** WeightGuard — Steganographic Malware Detection in AI Model Weights
> **Hosting:** Docker / Render / Railway / GCP Cloud Run
> **CI/CD:** GitHub Actions
> **Last Updated:** 2026-09-23
> **Author:** Nishant Maurya (Niss54)

---

## 📑 Table of Contents

1. [Environments](#1-environments)
2. [Prerequisites](#2-prerequisites)
3. [Quick Start — Local](#3-quick-start--local)
4. [Docker Setup](#4-docker-setup)
5. [Environment Variables](#5-environment-variables)
6. [API Server](#6-api-server)
7. [Cloud Deployment](#7-cloud-deployment)
   - [Render (Recommended)](#render-recommended)
   - [Railway](#railway)
   - [GCP Cloud Run](#gcp-cloud-run)
8. [CI/CD Pipeline](#8-cicd-pipeline)
9. [Monitoring & Health](#9-monitoring--health)
10. [Rollback Strategy](#10-rollback-strategy)
11. [Production Checklist](#11-production-checklist)
12. [Related Documents](#12-related-documents)

---

## 🌍 1. Environments

| Environment | URL | Purpose | Branch |
|---|---|---|---|
| **Development** | `http://localhost:8000` | Local dev + testing | `feature/*` |
| **Staging** | `https://weightguard-staging.onrender.com` | Pre-demo validation | `develop` |
| **Production** | `https://weightguard.onrender.com` | Live demo for judges | `main` |

---

## 📋 2. Prerequisites

```bash
# Verify you have the right versions
python --version    # >= 3.11
pip --version       # >= 23.x
docker --version    # >= 24.x (for containerized deployment)
git --version       # >= 2.x

# Optional — for cloud CLI deployment
render --version    # Render CLI
railway --version   # Railway CLI
gcloud --version    # Google Cloud SDK
```

---

## ⚡ 3. Quick Start — Local

### Step 1 — Clone & Install

```bash
git clone https://github.com/Niss54/WeightGuard.git
cd WeightGuard

# Create virtual environment
python -m venv .venv
source .venv/bin/activate      # macOS/Linux
# .venv\Scripts\activate       # Windows

# Install dependencies
pip install -r requirements.txt
```

### Step 2 — Configure Environment

```bash
cp .env.example .env

# Edit .env:
#   REPORTS_DIR=./reports
#   QUARANTINE_DIR=./quarantine
#   LOG_LEVEL=INFO
```

### Step 3 — Run WeightGuard

```bash
# Option A — CLI scan
python -m weightguard.cli scan ./models/test_model.pt --verbose

# Option B — API server
uvicorn weightguard.api:app --reload --host 0.0.0.0 --port 8000

# Option C — Docker (see Section 4)
docker compose up -d
```

### Step 4 — Verify

```bash
# Health check
curl http://localhost:8000/health
# → { "status": "ok", "version": "1.0.0", "agents": 6 }

# Quick scan via API
curl -X POST http://localhost:8000/scan \
  -F "file=@./models/test_model.pt"
```

---

## 🐳 4. Docker Setup

### Dockerfile

```dockerfile
# Dockerfile

FROM python:3.11-slim AS base

# System dependencies
RUN apt-get update && apt-get install -y \
    libgomp1 \                          
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Install Python dependencies (cached layer)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy source
COPY weightguard/ ./weightguard/
COPY weightguard.config.yml .
COPY .env.example .env

# Create output directories
RUN mkdir -p reports quarantine models

# Expose API port
EXPOSE 8000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=10s --retries=3 \
  CMD curl -f http://localhost:8000/health || exit 1

# Start API
CMD ["uvicorn", "weightguard.api:app", "--host", "0.0.0.0", "--port", "8000"]
```

### docker-compose.yml (Local Dev)

```yaml
# docker-compose.yml

version: '3.8'

services:
  weightguard:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8000:8000"
    env_file: .env
    volumes:
      - ./reports:/app/reports          # Persist scan reports
      - ./quarantine:/app/quarantine    # Persist quarantined files
      - ./models:/app/models            # Mount local test models
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  # Optional: Nginx reverse proxy (for demo)
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./docker/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - weightguard
    profiles: ["production"]
```

### Docker Commands

```bash
# Build image
docker compose build

# Start services
docker compose up -d

# View logs
docker compose logs -f weightguard

# Scan a file via running container
docker compose exec weightguard python -m weightguard.cli scan /app/models/test.pt

# Stop all
docker compose down

# Rebuild after code changes
docker compose up -d --build weightguard

# Clean up
docker compose down -v --remove-orphans
docker image prune -f
```

### Nginx Config (Demo Mode)

```nginx
# docker/nginx.conf

events {
  worker_connections 512;
}

http {
  # Rate limiting — protect scan endpoint
  limit_req_zone $binary_remote_addr zone=scan:10m rate=10r/m;

  gzip on;
  gzip_types application/json;

  server {
    listen 80;
    server_name _;

    # WeightGuard API
    location / {
      proxy_pass http://weightguard:8000;
      proxy_http_version 1.1;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

      # Security headers
      add_header X-Frame-Options DENY;
      add_header X-Content-Type-Options nosniff;
    }

    # Scan endpoint — rate limited
    location /scan {
      limit_req zone=scan burst=5 nodelay;
      proxy_pass http://weightguard:8000;
      proxy_set_header Host $host;
      proxy_read_timeout 120s;       # Scanning can take time for large models
    }

    # Static report downloads
    location /reports/ {
      proxy_pass http://weightguard:8000;
    }
  }
}
```

---

## 🔐 5. Environment Variables

```bash
# .env.example

# ─────────────────────────────────────────
# WeightGuard Configuration
# ─────────────────────────────────────────

# Directories
REPORTS_DIR=./reports           # JSON scan report output directory
QUARANTINE_DIR=./quarantine     # Quarantined model file directory

# Logging
LOG_LEVEL=INFO                  # DEBUG | INFO | WARNING | ERROR

# Config
CONFIG_PATH=./weightguard.config.yml

# ─────────────────────────────────────────
# API Server
# ─────────────────────────────────────────
HOST=0.0.0.0
PORT=8000
API_VERSION=v1
MAX_UPLOAD_SIZE_MB=500          # Max model file size per scan request

# ─────────────────────────────────────────
# Alerts (Optional)
# ─────────────────────────────────────────
ALERT_WEBHOOK=                  # Slack incoming webhook URL
ALERT_EMAIL=                    # Email to notify on malicious verdict

# ─────────────────────────────────────────
# Security
# ─────────────────────────────────────────
API_KEY=                        # Optional API key authentication (leave blank to disable)
ALLOWED_ORIGINS=*               # CORS origins (set to your domain in production)
```

### Secrets Management

```bash
# Generate a secure API key
python -c "import secrets; print(secrets.token_hex(32))"

# Never commit .env to git
echo ".env" >> .gitignore

# For cloud deployments, set env vars via dashboard
# Render: https://render.com → Service → Environment
# Railway: railway variables set KEY=VALUE
# GCP: gcloud run services update --set-env-vars KEY=VALUE
```

---

## 🌐 6. API Server

### Start in Development

```bash
# Auto-reload on file changes
uvicorn weightguard.api:app --reload --host 0.0.0.0 --port 8000 --log-level debug
```

### Start in Production

```bash
# Multiple workers for concurrent scan requests
uvicorn weightguard.api:app \
  --host 0.0.0.0 \
  --port 8000 \
  --workers 2 \
  --log-level info \
  --timeout-keep-alive 30
```

### API Endpoints

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| `GET` | `/health` | Health check + agent status | None |
| `POST` | `/scan` | Upload and scan a model file | Optional API key |
| `GET` | `/reports` | List all scan reports | Optional API key |
| `GET` | `/reports/{scan_id}` | Get single report by ID | Optional API key |
| `DELETE` | `/reports/{scan_id}` | Delete a report | API key required |

### Scan Request

```bash
# Scan a model file
curl -X POST https://weightguard.onrender.com/scan \
  -H "X-API-Key: your_api_key_here" \
  -F "file=@./model.pt" \
  -F "strict=false"
```

```json
// Response
{
  "scan_id": "a4f3b2c1-...",
  "file": "model.pt",
  "format": "pt",
  "verdict": "suspicious",
  "risk_score": 0.45,
  "risk_flags": [
    "[LSBAgent] Non-random LSB pattern (p=0.00012) in layer 'layer2.weight'",
    "[ClusterAgent] Statistical outlier layer detected: 'layer2.weight'"
  ],
  "layers_scanned": 8,
  "report_url": "/reports/a4f3b2c1-...",
  "quarantined": false,
  "started_at": "2026-09-23T10:00:00Z",
  "completed_at": "2026-09-23T10:00:03Z"
}
```

---

## ☁️ 7. Cloud Deployment

### Render (Recommended)

**Best for: hackathon demo — free tier, zero config**

```yaml
# render.yaml

services:
  - type: web
    name: weightguard
    runtime: python
    buildCommand: "pip install -r requirements.txt"
    startCommand: "uvicorn weightguard.api:app --host 0.0.0.0 --port $PORT"
    envVars:
      - key: PYTHON_VERSION
        value: 3.11.0
      - key: REPORTS_DIR
        value: /tmp/reports
      - key: QUARANTINE_DIR
        value: /tmp/quarantine
      - key: LOG_LEVEL
        value: INFO
    disk:
      name: reports-storage
      mountPath: /tmp/reports
      sizeGB: 1
```

```bash
# Deploy to Render via CLI
render deploy
# Or: connect GitHub repo at dashboard.render.com
# Set: Main Branch = main, Build Command = pip install -r requirements.txt
# Start Command = uvicorn weightguard.api:app --host 0.0.0.0 --port $PORT
```

---

### Railway

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login and deploy
railway login
railway init
railway up

# Set environment variables
railway variables set REPORTS_DIR=/tmp/reports
railway variables set QUARANTINE_DIR=/tmp/quarantine
railway variables set LOG_LEVEL=INFO

# Open deployed URL
railway open
```

---

### GCP Cloud Run

```bash
# Build and push image to Artifact Registry
gcloud builds submit --tag gcr.io/[PROJECT_ID]/weightguard

# Deploy to Cloud Run
gcloud run deploy weightguard \
  --image gcr.io/[PROJECT_ID]/weightguard \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --memory 2Gi \
  --cpu 2 \
  --timeout 120 \
  --set-env-vars REPORTS_DIR=/tmp/reports,QUARANTINE_DIR=/tmp/quarantine,LOG_LEVEL=INFO

# Get service URL
gcloud run services describe weightguard --region us-central1 --format="value(status.url)"
```

---

## 🔄 8. CI/CD Pipeline

```yaml
# .github/workflows/deploy.yml

name: CI/CD — WeightGuard

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    name: Run Tests
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up Python 3.11
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"
          cache: "pip"
      
      - name: Install dependencies
        run: pip install -r requirements.txt
      
      - name: Lint
        run: ruff check weightguard/ tests/
      
      - name: Type check
        run: mypy weightguard/
      
      - name: Run tests with coverage
        run: pytest tests/ --cov=weightguard --cov-report=xml -v
      
      - name: Upload coverage
        uses: codecov/codecov-action@v4
        with:
          file: ./coverage.xml

  docker:
    name: Build Docker Image
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Build image
        run: docker build -t weightguard:${{ github.sha }} .
      
      - name: Smoke test image
        run: |
          docker run -d --name wg_test -p 8000:8000 weightguard:${{ github.sha }}
          sleep 5
          curl -f http://localhost:8000/health
          docker stop wg_test

  deploy:
    name: Deploy to Production
    needs: [test, docker]
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Deploy to Render
        run: |
          curl -X POST "${{ secrets.RENDER_DEPLOY_HOOK_URL }}"
          echo "✅ Deploy triggered on Render"
```

### Required GitHub Secrets

```
RENDER_DEPLOY_HOOK_URL   → From Render dashboard → Service → Deploy Hooks
```

---

## 📊 9. Monitoring & Health

### Health Check Response

```json
// GET /health
{
  "status": "ok",
  "version": "1.0.0",
  "timestamp": "2026-09-23T10:00:00Z",
  "agents": {
    "IngestAgent": "enabled",
    "EntropyAgent": "enabled",
    "LSBAgent": "enabled",
    "ClusterAgent": "enabled",
    "ReportAgent": "enabled",
    "QuarantineAgent": "enabled"
  },
  "storage": {
    "reports_dir": "./reports",
    "quarantine_dir": "./quarantine",
    "reports_count": 14,
    "quarantined_count": 2
  }
}
```

### Logging

```python
# WeightGuard uses structured logging via Python's logging module
# All agent actions log: [AgentName] [scan=abc12345] message

# Log output example
[2026-09-23 10:00:00] INFO [IngestAgent] [scan=a4f3b2c1] Loaded 8 weight tensors from pt
[2026-09-23 10:00:01] INFO [EntropyAgent] [scan=a4f3b2c1] Entropy scan complete. 8 layers analyzed.
[2026-09-23 10:00:02] WARNING [LSBAgent] [scan=a4f3b2c1] Non-random LSB pattern (p=0.00012) in layer 'layer2.weight'
[2026-09-23 10:00:02] INFO [ReportAgent] [scan=a4f3b2c1] Report saved → ./reports/a4f3b2c1.json | Verdict: SUSPICIOUS
```

```bash
# View logs in Docker
docker compose logs -f weightguard

# Filter for warnings and above
docker compose logs weightguard 2>&1 | grep -E "WARNING|ERROR|CRITICAL"
```

---

## ↩️ 10. Rollback Strategy

```bash
# Option 1 — Render instant rollback
# Dashboard → Service → Deploys → Click "Rollback" on previous deploy

# Option 2 — Git revert (triggers new CI/CD deploy)
git revert HEAD
git push origin main

# Option 3 — Docker tag rollback
docker pull weightguard:previous-sha
docker compose down
docker compose up -d

# Option 4 — Emergency: pin to last known good commit
git checkout <last-good-sha>
git push origin main --force-with-lease
```

---

## ✅ 11. Production Checklist

### Pre-Demo (Day Before)

- [ ] All tests passing (`pytest tests/ -v`)
- [ ] Docker image builds without errors
- [ ] App deployed on cloud (Render / Railway)
- [ ] Health check endpoint returns `200 OK`
- [ ] Test scan with benign model → verdict: `clean`
- [ ] Test scan with synthetic malicious model → verdict: `suspicious` or `malicious`
- [ ] `DEMO_SCRIPT.md` reviewed and rehearsed
- [ ] Reports directory is writable
- [ ] Quarantine directory is writable
- [ ] API response time < 30s for a 50MB model

### Demo Day

- [ ] Cloud URL is accessible from demo machine
- [ ] Backup: local Docker running as fallback
- [ ] Test models ready: `clean_model.pt` and `malicious_lsb.pt`
- [ ] JSON report viewer open in browser
- [ ] Terminal showing live logs during scan
- [ ] DEMO_SCRIPT.md open on second screen

---

## 🔗 12. Related Documents

| Document | Purpose |
|---|---|
| [ACCEPTANCE_REPORT.md](./ACCEPTANCE_REPORT.md) | PCC 2026 competition submission tracking |
| [AGENTS.md](./AGENTS.md) | Agent pipeline architecture + code |
| [CLAUDE.md](./CLAUDE.md) | Claude AI project context file |
| [DEMO_SCRIPT.md](./DEMO_SCRIPT.md) | Live demo script for judges |
| [ADMIN_HANDBOOK.md](./ADMIN_HANDBOOK.md) | Operations + admin runbook |

---

**WeightGuard — Deployable, Demonstrable, and Demo-Ready 🚀**
