<div align="center">

# 🧠 DevBlueprint

### *The Complete Full-Stack Project Blueprint & Documentation Kit*

A production-ready project documentation template + architecture guide for modern web applications.
Stop starting from scratch — start with a solid foundation.

<br/>

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Node.js](https://img.shields.io/badge/Node.js-20.x_LTS-green?logo=node.js)](https://nodejs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue?logo=typescript)](https://typescriptlang.org)
[![Next.js](https://img.shields.io/badge/Next.js-14-black?logo=next.js)](https://nextjs.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue?logo=postgresql)](https://postgresql.org)
[![Redis](https://img.shields.io/badge/Redis-7.x-red?logo=redis)](https://redis.io)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue?logo=docker)](./06_DEPLOYMENT.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](./CONTRIBUTING.md)

<br/>

[📋 PRD](#-documentation) · [🏗️ Architecture](#️-tech-stack) · [🚀 Quick Start](#-quick-start) · [📁 Project Structure](#-project-structure) · [🤝 Contributing](#-contributing)

<br/>

---

</div>

## ✨ What is DevBlueprint?

**DevBlueprint** is a comprehensive documentation framework and project starter kit designed to eliminate the chaos of starting a new software project. It gives your team a structured, professional brain — covering everything from product requirements to deployment, security, and beyond.

> 💡 **The Problem:** Most projects fail not because of bad code, but because of unclear requirements, missing documentation, and no process. DevBlueprint solves this before a single line of code is written.

### Why DevBlueprint?

| Without DevBlueprint | With DevBlueprint |
|---------------------|-------------------|
| ❌ No clear requirements | ✅ Detailed PRD with user stories |
| ❌ Architecture decided on the fly | ✅ Documented system design upfront |
| ❌ API designed inconsistently | ✅ Standardized REST API patterns |
| ❌ Security added as afterthought | ✅ Security baked in from day 1 |
| ❌ "It works on my machine" | ✅ Docker + CI/CD pipeline ready |
| ❌ No testing strategy | ✅ Complete test pyramid defined |
| ❌ Team has no contribution guide | ✅ Professional CONTRIBUTING.md |

---

## 📋 Documentation

> All documentation lives in the root directory. Each file is a detailed, ready-to-use template.

| # | Document | Description | Status |
|---|----------|-------------|--------|
| 00 | [📋 PRD.md](./00_PRD.md) | Product Requirements — goals, features, user stories, KPIs | ✅ Ready |
| 01 | [🏗️ ARCHITECTURE.md](./01_ARCHITECTURE.md) | System design, tech stack, data flow, ADRs | ✅ Ready |
| 02 | [🗄️ DATABASE.md](./02_DATABASE.md) | Schema design, Prisma models, indexes, migrations, seeding | ✅ Ready |
| 03 | [🔌 API.md](./03_API.md) | REST API endpoints, auth, request/response formats, error codes | ✅ Ready |
| 04 | [🎨 UI_UX.md](./04_UI_UX.md) | Design system, color palette, typography, components, user flows | ✅ Ready |
| 05 | [✅ TESTING.md](./05_TESTING.md) | Unit, integration & E2E testing strategy with code examples | ✅ Ready |
| 06 | [🚀 DEPLOYMENT.md](./06_DEPLOYMENT.md) | Docker, Nginx, CI/CD pipeline, monitoring, rollback | ✅ Ready |
| 07 | [🔒 SECURITY.md](./07_SECURITY.md) | OWASP Top 10, JWT strategy, rate limiting, incident response | ✅ Ready |
| 08 | [📝 TODO.md](./08_TODO.md) | Phase-wise task tracker with bug board | ✅ Ready |
| 09 | [📝 CHANGELOG.md](./09_CHANGELOG.md) | Version history following Keep a Changelog | ✅ Ready |
| — | [🤝 CONTRIBUTING.md](./CONTRIBUTING.md) | Branch naming, commit conventions, PR process | ✅ Ready |
| — | [⚙️ .env.example](./.env.example) | All environment variables documented with descriptions | ✅ Ready |

---

## 🏗️ Tech Stack

This blueprint is designed around a proven, modern full-stack:

<table>
  <tr>
    <th>Layer</th>
    <th>Technology</th>
    <th>Purpose</th>
  </tr>
  <tr>
    <td rowspan="5"><b>🖥️ Frontend</b></td>
    <td>Next.js 14 (App Router)</td>
    <td>React framework with SSR/SSG</td>
  </tr>
  <tr>
    <td>TypeScript 5.x</td>
    <td>Type safety across the stack</td>
  </tr>
  <tr>
    <td>Tailwind CSS + shadcn/ui</td>
    <td>Styling + accessible components</td>
  </tr>
  <tr>
    <td>TanStack Query v5</td>
    <td>Server state, caching, sync</td>
  </tr>
  <tr>
    <td>Zustand</td>
    <td>Lightweight client state</td>
  </tr>
  <tr>
    <td rowspan="4"><b>⚙️ Backend</b></td>
    <td>Node.js 20 LTS + Express</td>
    <td>API server runtime</td>
  </tr>
  <tr>
    <td>TypeScript 5.x</td>
    <td>End-to-end type consistency</td>
  </tr>
  <tr>
    <td>Prisma ORM</td>
    <td>Type-safe database access</td>
  </tr>
  <tr>
    <td>Zod</td>
    <td>Runtime input validation</td>
  </tr>
  <tr>
    <td rowspan="2"><b>🗄️ Database</b></td>
    <td>PostgreSQL 15</td>
    <td>Primary relational database</td>
  </tr>
  <tr>
    <td>Redis 7</td>
    <td>Cache, sessions, rate limiting</td>
  </tr>
  <tr>
    <td rowspan="4"><b>☁️ DevOps</b></td>
    <td>Docker + Compose</td>
    <td>Containerized environments</td>
  </tr>
  <tr>
    <td>Nginx</td>
    <td>Reverse proxy + SSL</td>
  </tr>
  <tr>
    <td>GitHub Actions</td>
    <td>CI/CD pipeline</td>
  </tr>
  <tr>
    <td>Sentry</td>
    <td>Error monitoring</td>
  </tr>
</table>

---

## 🚀 Quick Start

### Prerequisites

```bash
node --version   # >= 20.x
npm --version    # >= 10.x
docker --version # >= 24.x
```

### 1. Clone & Install

```bash
# Clone the repository
git clone https://github.com/[your-org]/[project-name].git
cd [project-name]

# Install dependencies
npm install
```

### 2. Environment Setup

```bash
# Copy environment variables
cp .env.example .env

# Open .env and fill in required values:
# → JWT_SECRET (generate: openssl rand -hex 64)
# → JWT_REFRESH_SECRET (generate: openssl rand -hex 64)
# → DATABASE_URL (or use Docker defaults below)
```

### 3. Start Services

```bash
# Start PostgreSQL + Redis via Docker
docker compose up -d postgres redis

# Run database migrations
npx prisma migrate dev

# Seed with sample data
npx prisma db seed
```

### 4. Start Development

```bash
# Start backend (port 8000)
npm run dev:backend

# Start frontend (port 3000) — in new terminal
npm run dev:frontend

# OR start everything at once
npm run dev
```

### 5. Verify Setup

```bash
# Health check
curl http://localhost:8000/health
# → { "status": "healthy", "services": { "database": "up", "redis": "up" } }

# Open app
open http://localhost:3000
```

---

## 📁 Project Structure

```
[project-name]/
│
├── 📋 00_PRD.md              # Product Requirements Document
├── 🏗️ 01_ARCHITECTURE.md     # System architecture & tech decisions
├── 🗄️ 02_DATABASE.md         # Database design & schema
├── 🔌 03_API.md              # REST API documentation
├── 🎨 04_UI_UX.md            # UI/UX design guide & components
├── ✅ 05_TESTING.md          # Testing strategy & examples
├── 🚀 06_DEPLOYMENT.md       # Deployment guide & CI/CD
├── 🔒 07_SECURITY.md         # Security guidelines & checklist
├── 📝 08_TODO.md             # Task tracker & roadmap
├── 📝 09_CHANGELOG.md        # Version history
│
├── 📂 frontend/              # Next.js application
│   ├── src/
│   │   ├── app/              # Next.js App Router pages
│   │   ├── components/       # UI components
│   │   ├── hooks/            # Custom React hooks
│   │   ├── lib/              # Utilities, API client
│   │   ├── store/            # Zustand state stores
│   │   └── types/            # TypeScript types
│   └── public/               # Static assets
│
├── 📂 backend/               # Node.js + Express API
│   ├── src/
│   │   ├── routes/           # API route definitions
│   │   ├── controllers/      # Route handlers
│   │   ├── services/         # Business logic
│   │   ├── middleware/       # Auth, validation, rate limit
│   │   ├── validators/       # Zod schemas
│   │   ├── config/           # Database, Redis, env config
│   │   └── utils/            # Logger, helpers
│   └── prisma/
│       ├── schema.prisma     # Database schema
│       ├── migrations/       # Migration history
│       └── seed.ts           # Seed data
│
├── 📂 tests/                 # All test files
│   ├── unit/                 # Unit tests (Vitest)
│   ├── integration/          # API tests (Supertest)
│   ├── e2e/                  # E2E tests (Playwright)
│   └── fixtures/             # Test data factories
│
├── 📂 docker/                # Docker configs
│   ├── nginx.conf            # Nginx reverse proxy config
│   └── Dockerfile.*          # Dockerfiles
│
├── 📄 docker-compose.yml     # Local dev services
├── 📄 docker-compose.prod.yml # Production services
├── 📄 .env.example           # Environment variables template
├── 📄 .gitignore             # Git ignore rules
├── 📄 package.json           # Root scripts & workspace config
├── 📄 CONTRIBUTING.md        # How to contribute
├── 📄 LICENSE                # MIT License
└── 📄 README.md              # This file
```

---

## 🔑 Key Features

```
🔐 Authentication    JWT access tokens + HttpOnly refresh cookies
👥 Authorization    Role-based access control (USER, ADMIN, SUPER_ADMIN)
🛡️ Security         OWASP Top 10, Helmet.js, rate limiting, input validation
🗄️ Database         PostgreSQL + Prisma ORM with type-safe queries
⚡ Caching          Redis for sessions, rate limiting, hot data
📧 Email            Pluggable email service (Resend / SendGrid / SMTP)
📁 File Upload      S3/R2/Cloudinary with MIME validation
✅ Testing          Unit + Integration + E2E test setup with examples
🚀 CI/CD           GitHub Actions pipeline (test → build → deploy)
🐳 Docker           Full containerization with docker-compose
📊 Monitoring       Sentry error tracking + health check endpoint
📝 Logging          Winston structured logging
🔄 Migrations       Prisma migrations with rollback strategy
```

---

## 🧪 Running Tests

```bash
# All tests
npm run test

# Unit tests only
npm run test:unit

# Integration tests
npm run test:integration

# E2E tests (Playwright)
npm run test:e2e

# Tests with coverage report
npm run test:coverage

# Tests in watch mode (dev)
npm run test:watch
```

**Coverage targets:** Lines: 80%+ | Functions: 80%+ | Branches: 75%+

---

## 🚢 Deployment

For full deployment documentation, see [🚀 06_DEPLOYMENT.md](./06_DEPLOYMENT.md).

### Quick Deploy (Docker)

```bash
# Build images
docker compose -f docker-compose.prod.yml build

# Deploy
docker compose -f docker-compose.prod.yml up -d

# Run migrations in production
docker compose exec backend npx prisma migrate deploy
```

### CI/CD Pipeline

Every push to `main` automatically:
1. ✅ Runs all tests
2. 🔨 Builds Docker image
3. 🚀 Deploys to production server

See [.github/workflows/deploy.yml](./.github/workflows/deploy.yml) for details.

---

## 🤝 Contributing

We welcome contributions! Please read our [CONTRIBUTING.md](./CONTRIBUTING.md) first.

```bash
# Quick contribution flow
git checkout -b feat/your-feature
# ... make changes ...
git commit -m "feat(scope): description"
git push origin feat/your-feature
# Open Pull Request on GitHub
```

**Branch naming:** `feat/`, `fix/`, `docs/`, `chore/`, `test/`
**Commit format:** [Conventional Commits](https://conventionalcommits.org)

---

## 📜 Changelog

See [09_CHANGELOG.md](./09_CHANGELOG.md) for the full version history.

**Latest:** `v0.1.0` — Initial release 🎉

---

## 🔒 Security

Found a security vulnerability? **Please do NOT open a public issue.**

Email us directly: [security@[yourproject].com]

For our full security policy and guidelines, see [07_SECURITY.md](./07_SECURITY.md).

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](./LICENSE) file for details.

```
MIT License — Free to use, modify, and distribute.
Attribution appreciated but not required.
```

---

## 🙏 Acknowledgements

Built with love using these amazing open-source projects:

[Next.js](https://nextjs.org) · [Node.js](https://nodejs.org) · [PostgreSQL](https://postgresql.org) · [Prisma](https://prisma.io) · [Redis](https://redis.io) · [shadcn/ui](https://ui.shadcn.com) · [Tailwind CSS](https://tailwindcss.com) · [Vitest](https://vitest.dev) · [Playwright](https://playwright.dev) · [Docker](https://docker.com)

---

<div align="center">

**Made with ❤️ by [[Your Name / Org]](https://github.com/[your-handle])**

⭐ **Star this repo if it helped you!** ⭐

[![GitHub stars](https://img.shields.io/github/stars/[org]/[repo]?style=social)](https://github.com/[org]/[repo])

</div>
