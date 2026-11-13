<div align="center">
  <h1>🚀 ProposalFlow</h1>

  <p>
    <strong>Enterprise-grade multi-tenant B2B SaaS platform for commercial proposal management, financial margin governance, and auditable deal tracking.</strong>
  </p>

  <p>
    <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white" />
    <img alt="React 19" src="https://img.shields.io/badge/React_19-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" />
    <img alt="Hono" src="https://img.shields.io/badge/Hono-E36002?style=for-the-badge&logo=hono&logoColor=white" />
    <img alt="Prisma" src="https://img.shields.io/badge/Prisma-2D3748?style=for-the-badge&logo=prisma&logoColor=white" />
    <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" />
    <img alt="Tailwind CSS v4" src="https://img.shields.io/badge/Tailwind_CSS_v4-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white" />
  </p>

  <p align="center">
    <img src="https://img.shields.io/badge/Status-Active_Development-2ea44f?style=for-the-badge" alt="Project Status" />
    <img src="https://img.shields.io/badge/License-Proprietary-red?style=for-the-badge" alt="License" />
  </p>
</div>

---

## 📖 Overview

**ProposalFlow** is an enterprise-grade multi-tenant B2B SaaS platform engineered for commercial proposal workflows, financial margin governance, and auditable negotiation tracking. Unlike conventional CRMs designed primarily for top-of-funnel lead tracking, ProposalFlow focuses on the high-stakes closing phase: itemized pricing, discount margin ceilings, immutable version snapshots, and approval hierarchies.

Built with an emphasis on **strict row-level multi-tenancy, regulatory compliance, speed, and clean architecture**, it guarantees that every commercial quote aligns with corporate margin guidelines while maintaining a 100% auditable history of revisions and status transitions.

## ✨ Key Features

- **🏢 Row-Level Multi-Tenancy:** Strict tenant isolation across all domain models (`Client`, `ClientRepresentative`, `Proposal`, `ProposalVersion`, `Justification`) enforced via authenticated JWT middleware (`tenantId` injected and scoped on every query).
- **🛡️ Role-Based Access Control (RBAC) & SaaS Trial:** Automated role assignment upon onboarding (`MANAGER` for tenant founders, `SELLER` for team members), reporting hierarchy, and a 14-day SaaS trial license guard (`accessExpiresAt` returning `402 Payment Required` upon expiration).
- **👥 Client & Representative Governance:** Enforces domain integrity rules allowing 1 to 3 representatives per client, mandatory contact validation, and one-click communication triggers (WhatsApp via `wa.me`, direct telephone dialer via `tel:`, and email via `mailto:`).
- **📊 Priority-Sorted Kanban Pipeline:** High-density visual pipeline across six lifecycle stages (`DRAFT`, `SENT`, `NEGOTIATING`, `APPROVED`, `REJECTED`, `EXPIRED`), strictly ordered from `HIGH` to `LOW` priority.
- **🔒 Intentional Pipeline Safeguards:** Drag-and-drop is deliberately omitted to prevent accidental stage advancements; all pipeline shifts require structured modal interactions with mandatory audit justification logging.
- **💰 Itemized Financial & Margin Ceilings:** Multi-item proposal aggregation (`ProposalItem`) with unit values, quantities, units of measure, and discount percentages, automatically raising dynamic visual alerts whenever discounts exceed pre-set margin ceilings.
- **📜 Immutable Snapshots & Audit Trail:** Enforces mandatory justification for 100% of status, priority, or contract updates, paired with immutable version snapshots (`ProposalVersion` and `ProposalVersionItem`) and contract SLA tracking (`closedAt`, `contractExpiresAt`).
- **⚡ Deterministic Network Mocking:** Built-in Mock Service Worker (MSW v2/v3) request handlers for rapid, offline frontend development and isolated UI component testing.
- **🤖 Automated Testing Suite:** Comprehensive unit and integration coverage with Vitest for API and web components, alongside Playwright end-to-end (E2E) automated test flows.

## 🏗️ Architecture & Tech Stack

This project is structured as a **Monorepo** separating the REST API and the frontend workspace while standardizing linting, typing, and testing rules.

### 🧱 Monorepo Structure

```text
proposal-flow/
├── proposal-flow-api/          # Hono REST API, Prisma schema, and domain business logic
│   ├── prisma/                 # PostgreSQL schema and migration scripts
│   ├── src/
│   │   ├── controllers/        # Request handling and schema transformation
│   │   ├── middleware/         # Auth, Tenant isolation, and Trial guards
│   │   ├── routes/             # Hono route endpoints (Auth, Clients, Proposals)
│   │   ├── schemas/            # Zod validation schemas
│   │   └── services/           # Domain business rules and repositories
│   └── tests/                  # Vitest API and unit test suites
├── proposal-flow-web/          # React 19 + Vite + Tailwind CSS v4 frontend
│   ├── public/                 # Static assets, official logos, and MSW worker
│   ├── src/
│   │   ├── components/         # KanbanBoard, Modals, Forms, and Cards
│   │   ├── mocks/              # MSW network mock handlers and browser setup
│   │   └── pages/              # Login, Onboarding, TrialExpired, Dashboard
│   ├── e2e/                    # Playwright end-to-end specifications
│   └── tests/                  # Vitest component test suites
└── docs/
    ├── 1. PRD.md
    ├── 2. TRD.md
    ├── 3. Fluxo do App (App Flow).md
    ├── 4. Design UI UX (Design System).md
    ├── 5. Esquema Backend (Data Model).md
    ├── 6. Spec Driven Development (SDD).md
    ├── 7. Plano de Implementacao.md
    ├── MEMORY.md
    └── RULES.md
```

### 🛠️ Technology Stack

- **Frontend:** React 19, TypeScript, Vite, Tailwind CSS v4 (dynamic scale), TanStack Query v5, Axios, Lucide React.
- **Backend:** Node.js, Hono (`@hono/node-server`), TypeScript, Prisma ORM v7 (`@prisma/adapter-pg`), PostgreSQL, Zod, Argon2, JWT (`jsonwebtoken`).
- **Testing & Mocking:** Vitest, MSW (Mock Service Worker), `@testing-library/react`, Playwright.
- **Tooling & Governance:** ESLint 9, Prettier, strict coding rules (no-semi, single quotes, en-US commit messages).

## 🚀 Getting Started

### Prerequisites

- Node.js (v20 or higher)
- npm (v9 or higher)
- PostgreSQL instance running locally or via Docker

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/immaioli/proposal-flow.git
   cd proposal-flow
   ```

2. Install dependencies for all workspaces:
   ```bash
   npm install
   ```

### Running the Project

**1. Start the API (Backend):**
```bash
cd proposal-flow-api
npm install
npx prisma migrate dev
npm run dev
```
*The API will run on `http://localhost:3333`.*

**2. Start the Web App (Frontend):**
```bash
cd proposal-flow-web
npm install
npm run dev
```
*The UI will be available on `http://localhost:5173`.*

### Running Automated Tests

**Backend API Integration Tests:**
```bash
cd proposal-flow-api
npm test
```

**Frontend Component Tests (Vitest + MSW):**
```bash
cd proposal-flow-web
npm test
```

**End-to-End Tests (Playwright):**
```bash
cd proposal-flow-web
npx playwright test
```

### Available Scripts

| Workspace | Command | Description |
| :--- | :--- | :--- |
| **API** | `npm run dev` | Start Hono server with hot reload (`tsx watch`) |
| **API** | `npm run test` | Run API integration and schema tests with Vitest |
| **API** | `npm run build` | Compile TypeScript into `/dist` |
| **Web** | `npm run dev` | Launch Vite development server |
| **Web** | `npm run test` | Run frontend component and MSW mock tests |
| **Web** | `npm run build` | Type-check and create production bundle |
| **Web** | `npm run lint` | Run ESLint checks across JSX/TSX files |

## ⚙️ Environment Variables

Create `.env` files in `proposal-flow-api` and `proposal-flow-web` based on the templates below:

**Backend API (`proposal-flow-api/.env`):**
```env
# Database connection (PostgreSQL)
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/proposalflow?schema=public"

# Security & Authentication
JWT_SECRET="your-super-secure-jwt-secret-key"
PORT=3333
NODE_ENV=development
```

**Frontend Web (`proposal-flow-web/.env`):**
```env
VITE_API_URL="http://localhost:3333"
```

## 🧠 Architectural Decisions (ADR Summary)

* **Why Hono over Express or NestJS?** Hono provides an ultra-lightweight, high-performance HTTP foundation with web-standard APIs (`Request`/`Response`), native TypeScript ergonomics, and zero runtime bloat.
* **Why omit Drag-and-Drop in the Kanban?** Commercial proposals involve binding financial terms, margin calculations, and contractual SLAs. Drag-and-drop leads to accidental status transitions. Requiring a structured modal transition ensures every stage shift is deliberate and accompanied by a mandatory audit justification.
* **Why Mandatory Justifications on 100% of Transitions?** In B2B sales cycles, management loses visibility when deals stall or change priority without explanation. Requiring justifications creates an immutable audit timeline for every negotiation revision.
* **Why Row-Level Multi-Tenancy via JWT?** Rather than maintaining separate databases per tenant, every domain table includes a foreign key `tenantId`. Authenticated tokens inject the verified tenant ID into every query, preventing cross-tenant data leaks with minimal infrastructure overhead.
* **Why Itemized Line Items & Margin Alerts?** B2B proposals are rarely single lump sums. Tracking item-level quantities, units, and discount percentages allows the system to warn reps when a custom discount breaches configured margin thresholds before sending quotes to clients.

## 🤝 Contributing & Code Guidelines

1. Follow the rules defined in [`docs/RULES.md`](./docs/RULES.md).
2. Code formatting: single quotes (`'...'`), no semicolons (`no-semi`), explicit descriptive variable names.
3. Commit messages must be in **en-US** following **Conventional Commits**:
   - `feat(api): ...` or `feat(web): ...`
   - `fix(api): ...` or `fix(web): ...`
   - `refactor(...)`, `docs:`, `test(...)`, `chore:`

## 📜 License

Private & Proprietary. All rights reserved.
