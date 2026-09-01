# ELSHWORK Roadmap

## Purpose

This roadmap describes the strategic development direction of ELSHWORK.

It is not a promise that every item will be delivered on a fixed date.

Priorities may change based on:

- product validation;
- security requirements;
- infrastructure constraints;
- user feedback;
- production readiness;
- regulatory requirements;
- ecosystem demand.

The guiding principle is simple:

> Build the strongest trust, collaboration and developer identity loop first.

---

# Current Foundation

ELSHWORK already includes a substantial platform foundation:

- repositories;
- Git-compatible workflows;
- Pull Requests;
- Code Review;
- Forge CI/CD;
- releases;
- packages;
- security scanning;
- organizations;
- teams;
- permissions;
- notifications;
- marketplace foundations;
- research;
- Developer Identity;
- moderation;
- search;
- administration;
- object storage abstraction;
- production infrastructure foundations.

The next stages focus on making these systems deeper, more connected and more useful to real users.

---

# Phase 1 — Platform Stabilization

## Goal

Make the existing ELSHWORK platform predictable, safe and production-ready.

### Priorities

- complete QA across all major pages;
- eliminate broken navigation and stale UI state;
- ensure CRUD updates without manual refresh;
- fix avatar delivery and fallback behavior;
- finalize repository file path handling;
- support arbitrary safe nested folders;
- improve file rendering;
- complete responsive behavior;
- improve empty/loading/error/success states;
- remove native browser dialogs;
- harden permission boundaries;
- maintain coverage and executable test requirements.

### Quality requirements

- TypeScript checks pass;
- production build passes;
- Nest bootstrap passes;
- executable tests pass;
- coverage remains above required thresholds;
- live E2E passes;
- security boundaries remain backend-enforced.

---

# Phase 2 — Localization Completion

## Goal

Make ELSHWORK usable by an international audience.

### Languages

- English;
- Russian;
- German;
- Spanish;
- French;
- Arabic;
- Simplified Chinese.

### Requirements

- remove remaining hardcoded UI strings;
- grammar review for RU and EN;
- complete translation coverage;
- proper fallback behavior;
- Arabic RTL support;
- responsive RTL layouts;
- Chinese typography and layout validation;
- localized loading/error/empty states;
- localized modals and confirmations;
- localized Research and administration surfaces.

Localization should be treated as a platform-level capability, not a cosmetic layer.

---

# Phase 3 — Research Trust & Reproducibility

## Priority

This is one of the highest-priority strategic directions of ELSHWORK.

## Goal

Turn Research into a system where results can be connected to reproducible evidence.

### Core workflow

```text
Publish
↓
Research Revision
↓
Reproducibility Passport
↓
Expected Result
↓
Independent Reproduction
↓
Verification
↓
Verified Contribution
↓
Developer Identity
```

### Major capabilities

- immutable Research revisions;
- revision-bound reproduction;
- expected-result locking;
- Reproducibility Passport;
- automatic technical metadata capture;
- human-required methodology fields;
- READY / NOT READY verification readiness;
- independent reproduction lifecycle;
- evidence references;
- verification history;
- conflict-of-interest handling;
- Expert Review separation;
- negative reproduction support;
- demo-data isolation;
- verified reputation events;
- Research search filters;
- notifications;
- privacy filtering.

### Core trust rule

Verification of one revision must never automatically verify a later revision.

Example:

```text
Revision 1
2 verified reproductions

Revision 2
0 verified reproductions
UNVERIFIED
```

### Product validation

Before expanding the system further:

- internal cold-start walkthrough;
- independent cold-start walkthrough;
- fix critical UX blockers;
- pilot with 10–20 external users.

Success is measured by completed workflows, not registration counts.

---

# Phase 4 — Developer Identity Deepening

## Goal

Build professional identity from verified contribution rather than popularity alone.

### Areas

- ELSH Score;
- Domain Reputation;
- Verified Expertise;
- achievements;
- contribution history;
- skill levels;
- research contributions;
- independent reproductions;
- review quality;
- package/release contributions;
- security contributions.

### Principles

- no client-issued reputation;
- no client-issued achievements;
- no client-defined score weight;
- duplicate verified events must not double-count;
- demo activity must not affect real scores;
- scoring factors should be explainable;
- anti-gaming rules remain backend-controlled.

### Long-term direction

A developer profile should gradually become a verifiable professional record.

---

# Phase 5 — AI Integration

## Goal

Turn the existing ELSHWORK AI area into a real, provider-independent AI layer.

### First provider

Ollama may be used as an initial self-hosted adapter for development and early production testing.

### Architecture

```text
ELSHWORK Web
↓
ELSHWORK API
↓
AI Service
↓
AI Provider Adapter
↓
Model
```

### Possible adapters

- Ollama;
- OpenAI;
- Anthropic;
- future providers.

### Initial capabilities

- code assistance;
- repository-aware explanations;
- PR summaries;
- issue assistance;
- documentation generation;
- research assistance;
- security finding explanation.

### Security

AI must never bypass repository permissions.

Repository context must be selected server-side after authorization.

AI access should be:

- permission-aware;
- auditable;
- rate-limited;
- provider-transparent.

---

# Phase 6 — Marketplace Completion

## Goal

Develop Marketplace into a trusted ecosystem for developer products.

### Areas

- seller profiles;
- seller verification;
- product versions;
- licenses;
- permission scopes;
- installation scopes;
- OAuth Apps;
- verified installs;
- verified reviews;
- security review;
- suspend/archive lifecycle;
- audit history.

### Production invariant

A product must not become publicly published unless:

```text
Seller = VERIFIED
AND
Security Review = PASSED
```

This must be enforced by the backend.

---

# Phase 7 — Payments

## First provider

YooKassa.

## Goal

Introduce real payments without building fake financial infrastructure.

### Phase 7.1 — Payments Core

- PaymentProvider abstraction;
- YooKassa adapter;
- sandbox/test integration;
- payment creation;
- provider confirmation;
- webhooks;
- idempotency;
- refunds;
- financial audit;
- Marketplace orders;
- donations.

### Phase 7.2 — Marketplace settlements

- seller onboarding;
- external KYC;
- split payments;
- ELSHWORK platform commission;
- seller payouts;
- refund handling;
- settlement status.

### Core rule

ELSHWORK must never treat frontend success as payment confirmation.

Only verified provider state can produce:

```text
PAID
```

### Card security

ELSHWORK should not store raw card information.

---

# Phase 8 — Global Payments

## Goal

Support international buyers and sellers.

### Possible providers

- Stripe Connect;
- PayPal;
- Adyen;
- future regional providers.

### Architecture

```text
Payments Core
↓
Payment Router
├── YooKassa
├── Stripe
├── PayPal
├── Adyen
└── Future adapters
```

### Routing factors

- buyer country;
- seller country;
- currency;
- payment method;
- provider availability;
- compliance requirements.

### Multi-currency

Support should be designed for currencies such as:

- RUB;
- USD;
- EUR;
- GBP;
- JPY;
- CNY;
- others.

Amounts must not use floating-point arithmetic.

---

# Phase 9 — Donations

## Goal

Allow users to financially support developers, researchers and projects.

### Possible flows

```text
Support ELSHWORK
```

and later:

```text
Support Developer
Support Researcher
Support Project
```

### Requirements

- provider-backed payments;
- recipient identity;
- currency;
- provider status;
- refund state;
- financial audit.

Creator payouts require real payment-provider support and KYC.

No fake internal wallet system should be introduced initially.

---

# Phase 10 — Communication

## Goal

Expand ELSHWORK communication beyond text messaging.

### Phase 10.1 — Direct calls

- 1-to-1 voice;
- 1-to-1 video;
- microphone controls;
- camera controls;
- screen sharing;
- incoming call state;
- missed calls;
- device selection;
- reconnect handling.

### Technology

WebRTC.

### Infrastructure

- signaling through ELSHWORK backend;
- STUN;
- TURN;
- explicit permissions;
- abuse protection.

### Phase 10.2 — Group communication

- group calls;
- team rooms;
- repository rooms;
- research discussion rooms.

For larger calls, ELSHWORK should use an SFU architecture rather than unrestricted peer-to-peer meshes.

Possible implementations may include:

- LiveKit;
- mediasoup;
- Janus;
- other adapters.

---

# Phase 11 — Repository Context Communication

## Goal

Make communication part of development context.

Examples:

```text
Pull Request
[Start Call]
```

```text
Research Revision
[Discuss]
```

```text
Issue
[Start Voice Room]
```

Call metadata can reference the related object.

This avoids separating discussion from the work being discussed.

---

# Phase 12 — Organizations & Governance

## Goal

Strengthen organization-level controls.

### Areas

- organization dashboard;
- policies;
- repository defaults;
- quotas;
- audit;
- membership governance;
- owner hierarchy;
- team hierarchy UI;
- repository grants;
- organization-owned resources;
- security settings.

### Principle

Organization membership alone must never automatically grant repository write access.

---

# Phase 13 — Administration & Trust

## Goal

Provide production-grade platform governance without destroying user privacy.

### Administration areas

- users;
- organizations;
- repositories;
- communities;
- moderation;
- security;
- Forge;
- runners;
- packages;
- Marketplace;
- sellers;
- economy;
- cloud;
- AI;
- audit;
- feature flags;
- quotas;
- system health.

### Admin privacy principle

Platform administrators do not automatically gain access to private repository content.

Emergency private access requires:

- specific permission;
- reason;
- step-up authentication;
- limited duration;
- immutable audit event.

---

# Phase 14 — Search Infrastructure

## Goal

Scale global search without tying the platform to a single search engine.

### Development

PostgreSQL may continue to support local and early deployments.

### Production adapters

- OpenSearch;
- Meilisearch;
- other dedicated indexes.

### Search must support

- repositories;
- code;
- files;
- commits;
- issues;
- Pull Requests;
- users;
- organizations;
- teams;
- communities;
- packages;
- Marketplace;
- research;
- Q&A.

### Security

Private repository/code results must never be returned to unauthorized users.

---

# Phase 15 — ELSH CLI

## Goal

Create a first-party developer interface for ELSHWORK.

Possible future usage:

```bash
elsh clone user/project
elsh status
elsh push
elsh research
elsh publish
```

The CLI should complement existing Git compatibility rather than unnecessarily break compatibility with existing developer tools.

---

# Phase 16 — `.elsh` Persistent Digital Object

## Status

Experimental.

## Goal

Explore a new type of digital object rather than simply copying an existing file format.

The long-term concept is a Persistent Digital Object containing:

- identity;
- provenance;
- history;
- relationships;
- intent;
- permissions;
- integrity;
- trust metadata;
- payloads.

Possible future examples:

```text
research.elsh
experiment.elsh
software.elsh
dataset.elsh
agent.elsh
```

### Existing experimental work

An early `.elsh` proof-of-concept and viewer have already been explored.

### Future work

- ELSH Specification;
- stable binary layout;
- versioning;
- parser SDK;
- viewer;
- validator;
- MIME type;
- reference implementation;
- integration with ELSHWORK.

The format must remain independent enough that third-party tools can eventually understand it.

---

# Phase 17 — Desktop Integration

## Goal

Connect local developer environments with ELSHWORK.

Possible capabilities:

- publish local projects;
- repository synchronization;
- IDE integration;
- terminal integration;
- local file access with explicit permission;
- action logs;
- granular access control.

Desktop integration should never assume unrestricted access to the user's machine.

---

# Phase 18 — Production Infrastructure

## Goal

Operate ELSHWORK reliably as a public service.

### Initial infrastructure

- VPS;
- Docker;
- reverse proxy;
- HTTPS;
- PostgreSQL;
- Redis;
- object storage;
- backups;
- monitoring.

### Domain structure

Possible structure:

```text
www.elshwork.com
api.elshwork.com
assets.elshwork.com
status.elshwork.com
```

### Deployment flow

```text
Tests
↓
Quality Gates
↓
Production Build
↓
Deployment
↓
Health Check
↓
Release
```

Failed health checks should prevent broken releases from replacing healthy deployments.

---

# Phase 19 — Forge Runner Isolation

## Goal

Never execute arbitrary user CI workloads with unrestricted access to the main application infrastructure.

### Architecture

```text
ELSHWORK Core
↓
Forge Queue
↓
Isolated Runner
↓
Container / Sandbox
```

As usage grows, runners should move to separate infrastructure.

---

# Phase 20 — Object Storage Scaling

## Goal

Move user-generated assets away from application-local storage as the platform grows.

Possible uses:

- avatars;
- package binaries;
- release assets;
- Research artifacts;
- reproduction evidence;
- attachments.

The existing storage abstraction should remain provider-independent.

---

# Phase 21 — External Infrastructure Adapters

ELSHWORK should remain honest about features that depend on external infrastructure.

Examples:

- payment providers;
- KYC;
- payouts;
- AI providers;
- external search clusters;
- S3 infrastructure;
- cloud providers;
- production email;
- push notifications;
- CVE feeds;
- TURN infrastructure.

Unavailable external adapters should report states such as:

```text
NOT_CONFIGURED
WAITING_ADAPTER
UNAVAILABLE
```

They must never report fake production success.

---

# Product Validation Principle

ELSHWORK should avoid endless architecture expansion without user validation.

Before significantly expanding a new strategic workflow:

1. build the smallest complete version;
2. test it internally;
3. test it with someone unfamiliar with the design;
4. observe real friction;
5. simplify;
6. then expand.

This principle is especially important for Research Trust & Reproducibility.

---

# Long-Term Product Loop

```text
Build
↓
Collaborate
↓
Publish
↓
Verify
↓
Earn Reputation
↓
Discover
↓
Build Again
```

Research loop:

```text
Publish
↓
Declare
↓
Reproduce
↓
Verify
↓
Build Trust
```

---

# Strategic Focus

ELSHWORK should not compete by having the largest number of features.

Its strongest direction is the connection between:

```text
Code
+
Research
+
Evidence
+
Independent Verification
+
Professional Identity
```

The ecosystem should grow around this connection rather than dilute it.

---

# ELSHWORK

**Build. Share. Research. Verify. Ship.**

https://www.elshwork.com
