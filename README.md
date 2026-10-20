# PropertyOS

An open, multi-tenant property and facilities management platform.
One React + TypeScript UI, one API contract, and backends you can build in any language.

<!-- TODO: add badges once CI is set up -->
<!-- ![Build](https://github.com/YOUR-USERNAME/PropertyOS/actions/workflows/ci.yml/badge.svg) -->
<!-- ![License](https://img.shields.io/badge/license-MIT-blue) -->

---

## Table of Contents

- [About](#about)
- [Features](#features)
- [How It Works](#how-it-works)
- [Repository Structure](#repository-structure)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Adding a New Backend](#adding-a-new-backend)
- [Contributing](#contributing)
- [Roadmap](#roadmap)
- [License](#license)

---

## About

Small and mid-sized property managers often run on spreadsheets and chat groups: records are scattered, rent follow-up is manual, and maintenance requests get lost. PropertyOS puts properties, units, residents, leases, rent billing and maintenance in one system, with AI-assisted search and questions over your data.

PropertyOS is also a learn-by-building project:

- The **UI is fixed**: a single React + TypeScript app (`PropertyOS.web`), built and maintained by the project owner.
- The **backend is replaceable**: every backend implements the same API contract, so you can build it in .NET, NestJS, Express, Python, Java or anything else.
- The **database is your choice**: use the shared sandbox database or bring your own.

## Features

- Multi-tenant organizations with role-based access (Owner, Property Manager, Accountant, Technician, Resident)
- Properties, buildings, floors and units
- Residents and leases (renewal, termination, move-in/out, expiry alerts)
- Rent schedules, invoices, payments and late fees
- Maintenance requests, work orders, SLAs and preventive schedules
- Email and in-app notifications, scheduled reminders
- Dashboards and reports (occupancy, collection, maintenance)
- Search and AI: text analysis, natural-language questions over data, guided actions

## How It Works

```
 +----------------------+        +------------------------+        +------------------+
 |  PropertyOS.web      |  HTTPS |  Any backend that      |        |  Database        |
 |  React + TypeScript  +------->+  implements the        +------->+  Shared sandbox  |
 |  (one shared UI)     | bearer |  PropertyOS API        |        |  or your own     |
 +----------------------+ token  |  contract (/api/v1)    |        +------------------+
                                 +------------------------+
```

- The UI talks only to the API contract, never to a specific backend.
- Switch backend by changing one setting: the API base URL.
- The tenant is read from the sign-in token, never from client input.

## Repository Structure

```
PropertyOS/
|-- frontend/
|   `-- PropertyOS.web/              # React + TypeScript UI
|-- backend/
|   |-- PropertyOS.API.DotNet/       # Reference implementation (.NET)
|   |-- PropertyOS.API.NEST/         # NestJS implementation
|   `-- PropertyOS.API.<Stack>/      # Add yours (Express, Python, Java, ...)
`-- README.md
```

Naming convention for backends: `PropertyOS.API.<Stack>`

## Tech Stack

| Area | Technology |
|---|---|
| Frontend | React, TypeScript |
| API contract | OpenAPI 3.1, versioned at `/api/v1` |
| Reference backend | .NET, EF Core |
| Other backends | NestJS (and community backends) |
| Authentication | OpenID Connect bearer tokens (Entra ID in the reference deployment) |
| Data | Azure SQL, Cosmos DB, or your own database |
| Cloud (reference deployment) | Azure Always Free services: Static Web Apps, App Service F1, Functions, Key Vault, App Insights, AI Search, AI Language |

## Getting Started

### Prerequisites

- Git
- Node.js (LTS) and npm, for the frontend and NestJS backend
- .NET SDK, for the reference backend
- A database (see [Configuration](#configuration))

### 1. Clone the repository

```bash
git clone https://github.com/mayank3012/PropertyOS.git
cd PropertyOS
```

### 2. Run a backend

Pick one. Each backend has its own README with detailed steps.

**.NET (reference)**

```bash
cd backend/PropertyOS.API.DotNet
dotnet restore
dotnet run
```

**NestJS**

```bash
cd backend/PropertyOS.API.NEST
npm install
npm run start:dev
```

### 3. Run the frontend

```bash
cd frontend/PropertyOS.web
npm install
npm run dev
```

Open the URL printed in the terminal (commonly `http://localhost:5173`).

> TODO: add a single root-level build command (for example `./build.sh` or `make build`) that builds the frontend and the reference backend together, and document it here.

## Configuration

### Frontend

Copy the sample environment file and point it at the backend you are running:

```bash
cd frontend/PropertyOS.web
cp .env.example .env
```

| Variable | Purpose | Example |
|---|---|---|
| `VITE_API_BASE_URL` | Base URL of the backend API | `http://localhost:5000/api/v1` |


### Backend

Each backend reads its settings from environment variables or its own config file. Do not commit secrets.

| Setting | Purpose |
|---|---|
| Database connection | Shared sandbox database or your own |
| OIDC authority and audience | Token validation |
| Allowed origin (CORS) | The frontend URL |

### Database options

| Option | Notes |
|---|---|
| Shared sandbox | Hosted by the project owner. You get your own schema and login. No real data. Ask in an issue to get access. |
| Your own | Any engine you like (PostgreSQL, MySQL, SQL Server, SQLite, MongoDB, ...). Run it locally, for example with Docker. |

## Adding a New Backend

1. Create `backend/PropertyOS.API.<Stack>/`.
2. Implement the API contract, module by module (identity and tenancy first).
3. Include a README with setup, run and test instructions, and a `.env.example`.
4. Run the shared UI against your backend by setting `VITE_API_BASE_URL`.
5. Open a pull request.

Backends are compared by compatibility level:

| Level | Meaning |
|---|---|
| Core | Identity, tenancy, properties and units |
| Standard | Core plus residents, leases, billing and maintenance |
| Full | Standard plus notifications, reports and search |
| Advanced | Full plus the AI assistant |

## Contributing

You can join as a:

| Track | You work on |
|---|---|
| Frontend | `frontend/PropertyOS.web` |
| Backend | `backend/PropertyOS.API.*` |
| Ops / platform | CI/CD, infrastructure, monitoring |
| QA / test | API, end-to-end and tenant-isolation tests |
| AI / data | Search, text analysis, natural-language assistant |
| Docs / design | Docs, runbooks, UI design |

### Workflow

- `main` is the only long-lived branch. It is protected and changes arrive through pull requests.
- Create short-lived branches named with the work item key, for example `feature/POS-142-lease-renewal` or `fix/POS-210-late-fee-rounding`.
- Open a pull request into `main`. CI must pass and the code owner must approve.
- PRs are squash merged.
- Never commit secrets, real personal data or connection strings.

See `CONTRIBUTING.md` for details (TODO).

## Roadmap

- Sprint 0: repository, CI/CD, infrastructure as code, security baseline
- Sprints 1-2: identity, tenancy, roles, UI foundation
- Sprints 3-4: properties, units, residents, leases
- Sprints 5-6: billing, maintenance
- Sprint 7: notifications and background jobs
- Sprints 8-9: reports, search, AI assistant
- Sprint 10: observability, hardening, documentation
- After that: conformance test suite, Docker Compose, contributor guide, more community backends

## License

TODO: choose a licence (for example MIT) and add a `LICENSE` file.