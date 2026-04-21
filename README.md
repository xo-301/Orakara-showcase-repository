# Orakara-showcase-repository
Technical architecture overview of Orakara — a B2B manufacturing marketplace with a deterministic negotiation engine, escrow tranche system, and failure analytics pipeline. Built on Next.js, React Native, and PostgreSQL.
# Orakara — Technical Architecture Overview

**Orakara** is a B2B manufacturing marketplace built for emerging markets, starting with Bangladesh. It replaces fragmented, relationship-dependent procurement with a structured digital negotiation layer — letting buyers and sellers converge on price, specifications, payment terms, and contracts through a rules-governed, multi-stage deal engine.

> This repository contains technical architecture documentation and anonymised code excerpts. The production codebase is private as the product is in active development.

---

## What It Does

Orakara addresses a specific failure mode in B2B manufacturing procurement: deals collapse not because parties are too far apart, but because the *process* breaks down — misunderstood specs, ambiguous payment terms, undocumented agreements, and no mechanism for parties to credibly commit.

The platform gives both sides:

- A **structured negotiation engine** that walks them through Specs → Sample → Price & Payment → Contract in a defined sequence
- **Deterministic state transitions** — neither party can skip a stage, act out of turn, or make irreversible moves by accident
- A **timer system** with holiday awareness, extension requests, and admin override capability
- An **escrow tranche system** for platform-managed payment releases tied to proof of delivery milestones
- A **failure analytics pipeline** that captures why deals collapse, at what stage, and with what price gap — powering both internal learning and institutional research export

---

## Tech Stack

| Layer | Technologies |
|---|---|
| **Web platform** | Next.js 16, React 19, Tailwind CSS 4, TypeScript 5, Zod 3 |
| **Mobile** | React Native 0.81, Expo 54, Expo Router 6, TanStack Query 5 |
| **Database / ORM** | PostgreSQL via Prisma ORM 6 (50+ models, 40+ enums) |
| **Auth** | JWT (access + refresh), bcrypt, Google OAuth, Firebase Admin SDK |
| **Storage** | Cloudflare R2 via S3-compatible SDK + presigned URLs |
| **Email** | Resend |
| **Push notifications** | Firebase Cloud Messaging via React Native Firebase |
| **Deployment** | Vercel (web + 3 cron jobs) |
| **Data processing** | Apache Arrow, Parquet, ExcelJS, pdf-lib |
| **Testing** | Jest 30, ts-jest, Testing Library, 50% coverage threshold |

---

## Repository Structure (Production)

```
orakara-platform/          ← Next.js web app + API
├── prisma/schema.prisma   ← Full PostgreSQL schema
├── src/app/api/           ← ~80 API routes
├── src/lib/               ← Core engine (negotiation, cron, exports)
│   ├── negotiationOrchestrator.ts
│   ├── dealFlow.ts
│   ├── failureTracking.ts
│   └── cron/
└── src/middleware/        ← JWT auth middleware

orakara-mobile/            ← Expo React Native app
├── app/                   ← File-based routing (Expo Router)
│   ├── (auth)/
│   ├── buyer/
│   ├── dealRoom/
│   └── rfq/
└── components/
```

---

## Core Documentation

| Document | What it covers |
|---|---|
| [SYSTEM_DESIGN EXAMPLES.md](docs/SYSTEM_DESIGN_EXAMPLES.md) | Architecture decisions, key tradeoffs, design philosophy |
| [NEGOTIATION_ENGINE.md](docs/NEGOTIATION_ENGINE.md) | State machine, turn tracking, timer logic |
| [PAYMENT_ARCHITECTURE.md](docs/PAYMENT_ARCHITECTURE.md) | Escrow, milestones, proof-gated releases, dispute flow |
| [ANALYTICS_PIPELINE.md](docs/ANALYTICS_PIPELINE.md) | Failure detection, F1 scoring, research export |

---

## Build Status

| Area | Status |
|---|---|
| Negotiation state machine (4 stages) | ✅ Complete |
| Timer system (pause, resume, extend, holiday-aware) | ✅ Complete |
| Per-spec turn-based negotiation with final-offer grim trigger | ✅ Complete |
| Price negotiation (price, payment mode, quantity, lead time, tranches) | ✅ Complete |
| Escrow tranche negotiation | ✅ Complete |
| Failure analytics + F1 pipeline | ✅ Complete |
| Admin panel (users, products, timers, disputes, exports) | ✅ Complete |
| RFQ system (bids, pausing, contract lock) | ✅ Complete |
| Auth (JWT, Google OAuth, session management) | ✅ Complete |
| File upload (R2 presigned URLs) | ✅ Complete |
| Sample stage (request + approval) | 🔄 ~80% — courier API integration pending |
| Notifications dispatch | 🔄 ~40% — hooks exist, dispatch partially wired |
| Milestone proof submission + payment release routes | 🔄 In progress |
| In-app chat and calling | 🔲 Models designed, routes not built |
| TIN / trade registration verification | 🔲 Scaffold only |
| Feature flag enforcement | 🔲 Model exists, gate-check not implemented |
