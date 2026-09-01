# ELSHWORK Architecture

## Overview

ELSHWORK is a modular developer ecosystem designed around several core principles:

- backend-enforced security boundaries;
- modular domains;
- provider abstraction;
- auditability;
- reproducibility;
- explicit permissions;
- production honesty;
- extensibility without rewriting the platform.

The current architecture is based on a monorepo and combines web, API, runner and CLI components.

---

# High-Level Architecture

```text
Users
  ↓
ELSHWORK Web
  ↓
Reverse Proxy / HTTPS
  ↓
ELSHWORK API
  ├── Authentication
  ├── Repositories
  ├── Pull Requests
  ├── Forge
  ├── Security
  ├── Research
  ├── Developer Identity
  ├── Marketplace
  ├── Organizations
  ├── Notifications
  ├── Search
  ├── Moderation
  ├── Administration
  ├── Payments
  └── AI
        ↓
Infrastructure Adapters
        ├── PostgreSQL
        ├── Redis
        ├── Object Storage
        ├── Search Providers
        ├── AI Providers
        ├── Payment Providers
        ├── Email / Push
        └── Cloud Providers
```

---

# Monorepo Structure

The platform is organized as a monorepo.

Main workspaces include:

```text
apps/
├── api
├── web
├── runner
└── cli
```

### API

NestJS backend responsible for:

- authentication;
- authorization;
- business logic;
- repository permissions;
- Research workflows;
- Marketplace policy;
- moderation;
- notifications;
- administration;
- audit;
- external adapters.

### Web

React / Vite frontend responsible for:

- user interface;
- navigation;
- responsive layouts;
- localized content;
- authenticated API interaction;
- realtime UI updates.

The frontend must not make security decisions that belong to the backend.

### Runner

Execution component for Forge workloads.

Responsible for:

- pipeline jobs;
- dependency handling;
- retries;
- cancellation;
- isolated execution;
- runtime state.

### CLI

Developer-facing command interface intended for local interaction with ELSHWORK.

---

# Core Infrastructure

Current infrastructure includes:

- PostgreSQL;
- Prisma ORM;
- Redis;
- Docker;
- Git Smart HTTP;
- WebSocket / realtime communication;
- object storage abstraction;
- CI/CD runner infrastructure.

---

# Backend as Security Boundary

One of the most important architectural rules:

> The frontend is never the authority for permissions.

The backend must validate:

- repository read access;
- repository write access;
- organization grants;
- admin scopes;
- moderation authority;
- Marketplace publishing rules;
- Research verification permissions;
- payment state;
- private resource visibility.

Client-side controls may improve UX, but they must never be considered security enforcement.

---

# Repository Access Model

Repository access follows least privilege.

Typical repository roles:

```text
READ
WRITE
MAINTAIN
ADMIN
OWNER
```

A user must not gain write access simply because they:

- belong to an organization;
- are a platform administrator;
- can view the repository;
- share a team without explicit grant.

Access can come from:

- ownership;
- direct collaboration;
- team repository grants;
- organization governance rules;
- explicit elevated permissions.

---

# Private Repository Invariant

A private repository remains private.

This applies even to ordinary platform administrators.

```text
Platform Admin
≠
Automatic Private Repository Access
```

Emergency access must require:

- explicit emergency permission;
- reason;
- step-up authentication;
- limited lifetime;
- immutable audit event.

---

# Authentication

Authentication is based on backend-issued credentials and guards.

The platform supports:

- JWT authentication;
- role-aware guards;
- repository access checks;
- scoped admin roles;
- moderation restrictions.

Restrictions such as active login blocks must be enforced by authentication guards, not only by UI.

---

# Scoped Administration

Administration is separated from normal user permissions.

Possible scoped roles include:

```text
SUPER_ADMIN
SECURITY_ADMIN
MODERATOR
SUPPORT_ADMIN
FORGE_ADMIN
MARKET_ADMIN
FINANCE_ADMIN
CLOUD_ADMIN
AI_ADMIN
READ_ONLY_AUDITOR
```

An administrator should only gain access to actions allowed by the relevant scope.

For example:

```text
MARKET_ADMIN
→ Marketplace moderation

MARKET_ADMIN
→ Security administration
DENIED
```

---

# Audit Architecture

Critical operations must produce audit events.

Examples:

- permission changes;
- admin actions;
- moderation decisions;
- emergency private access;
- Marketplace publishing;
- security review decisions;
- Research verification;
- payment state transitions;
- payout actions;
- organization ownership transfer.

Audit history must be append-only for trust-critical actions.

---

# Repositories

Repository architecture includes:

- Git-compatible storage;
- Git Smart HTTP;
- repository metadata;
- permissions;
- file browsing;
- file rendering;
- branches;
- commits;
- repository settings;
- collaboration.

Path handling must support arbitrary safe nested paths while rejecting:

- traversal;
- absolute paths;
- `.git` internals;
- unsafe control characters.

---

# Git Compatibility

ELSHWORK maintains compatibility with existing Git tooling.

The platform may provide ELSH-branded developer interfaces in the future, but compatibility should not be broken unnecessarily.

Example:

```bash
git clone https://www.elshwork.com/user/project.git
```

Future CLI:

```bash
elsh clone user/project
```

The user-facing command layer may evolve while the underlying transport remains compatible.

---

# Pull Requests & Review

Pull Request infrastructure supports workflows such as:

- code review;
- inline comments;
- multi-line review;
- suggestions;
- dismissing reviews;
- approval rules;
- merge gates;
- CODEOWNERS;
- Forge status requirements;
- source update handling.

The merge decision belongs to backend policy.

---

# Forge

Forge is the ELSHWORK execution and CI/CD layer.

Core concepts include:

- jobs;
- DAG dependencies;
- retries;
- cancellation;
- continue-on-error;
- resource limits;
- runner state;
- artifacts.

Example:

```text
Pipeline
├── lint
├── test
│   └── needs: lint
└── build
    └── needs: test
```

Dependency cycles must be rejected before execution.

---

# Runner Isolation

User-defined CI workloads should not execute with unrestricted access to the main ELSHWORK application infrastructure.

Preferred architecture:

```text
ELSHWORK Core
   ↓
Forge Queue
   ↓
Isolated Runner
   ↓
Container / Sandbox
```

As the platform scales, runners should move to dedicated infrastructure.

---

# Security Scanning

Repository security scanning may include:

- secret detection;
- SAST;
- dependency analysis;
- container analysis;
- IaC analysis;
- CVE integration.

External advisory feeds must be implemented as real adapters.

If a CVE source is not configured, ELSHWORK must not pretend that live vulnerability intelligence is available.

---

# Object Storage

ELSHWORK uses an object storage abstraction.

Example:

```text
ObjectStorageService
├── LocalObjectStorage
├── S3ObjectStorage
└── Future providers
```

Object storage may be used for:

- avatars;
- package binaries;
- release assets;
- Research evidence;
- reproduction artifacts;
- attachments.

Local storage is acceptable for development and early deployments.

Production can use S3-compatible infrastructure.

---

# Packages & Releases

Package and release architecture includes:

- package versions;
- binary uploads;
- checksums;
- immutable versions;
- release tags;
- release assets;
- provenance;
- SBOM metadata.

Artifact integrity must be calculated server-side.

---

# Research Architecture

Research is a strategic platform domain.

Core relationships:

```text
Research
├── Author
├── Revisions
├── Repository
├── Citations
├── Expert Reviews
├── Reproductions
└── Contributions
```

Research must support revision history and persistent citation identity.

---

# Reproducibility Architecture

The reproducibility trust model is revision-bound.

```text
Research
  ↓
Research Revision
  ↓
Reproducibility Passport
  ↓
Independent Reproduction
  ↓
Verification History
```

A reproduction must never verify an unspecified or future revision.

---

# Research Revision Invariant

Published revisions are treated as historical records.

A meaningful update should create a new revision instead of rewriting the old one.

This allows statements such as:

```text
Revision 1
2 verified reproductions

Revision 2
0 verified reproductions
```

without ambiguity.

---

# Expected Result Lock

An expected result may be editable before independent reproduction begins.

Once a qualifying independent reproduction starts, the expected result becomes locked for that revision.

```text
Before reproduction:
editable

After reproduction starts:
LOCKED
```

Changing the expected result requires a new revision.

This must be enforced by the backend.

---

# Reproducibility Passport

The passport should combine two kinds of data.

### Auto-captured

Examples:

- repository;
- commit;
- runtime;
- dependency metadata;
- environment;
- Forge run;
- artifacts;
- hashes;
- timestamps.

### Human-required

Examples:

- methodology;
- interpretation;
- limitations;
- expected result;
- why the result matters.

The system must not create a false appearance of completeness simply because technical metadata exists.

---

# Verification Readiness

Research readiness should be represented by meaningful state:

```text
READY
NOT_READY
```

If not ready, the UI should clearly show why.

Example:

```text
Not ready for independent verification

Missing:
- Methodology
- Expected result
```

---

# Independent Reproduction

Independent reproduction is a dedicated domain entity.

It is not:

- a comment;
- a generic review;
- a checkbox.

It contains:

- reproducer identity;
- target revision;
- environment;
- result;
- evidence;
- history;
- outcome.

The original author must not independently reproduce their own work.

---

# Reproduction State Machine

Example:

```text
DRAFT
↓
RUNNING
↓
SUBMITTED
↓
UNDER_REVIEW
├── VERIFIED
├── REJECTED
└── NEEDS_CHANGES
      ↓
    UPDATED
      ↓
UNDER_REVIEW
```

Each transition is backend-controlled and recorded.

---

# Verification vs Outcome

Verification quality and research outcome are separate concepts.

Example:

```text
Verification:
VERIFIED

Outcome:
NOT_CONFIRMED
```

This means the reproduction was performed and reviewed correctly, but the original result was not reproduced.

Negative results are valuable evidence.

---

# Expert Review

Expert Review and Independent Reproduction answer different questions.

Expert Review:

> Is the methodology or reasoning sound?

Independent Reproduction:

> Can an independent participant obtain the claimed result?

They should remain separate in both backend and UI.

---

# Developer Identity

Developer Identity transforms verified contribution events into professional reputation.

Possible sources:

- commits;
- merged Pull Requests;
- reviews;
- accepted answers;
- releases;
- packages;
- Research;
- independent reproductions;
- security contributions.

The client cannot issue these contributions directly.

---

# Verified Contribution Events

Trusted actions produce backend-generated events.

Example:

```text
Independent Reproduction
↓
Verified by eligible reviewer
↓
VerifiedContributionEvent
↓
Reputation recalculation
↓
Developer Identity
```

Events should be idempotent and resistant to replay.

---

# ELSH Score

ELSH Score is derived from trusted contribution events.

Users must never be able to specify:

- contribution weight;
- score delta;
- achievement eligibility;
- expertise level.

Scoring logic remains backend-controlled.

---

# Explainable Reputation

The exact anti-gaming formula may remain private.

However, the user should understand the factors influencing contribution weight.

Example:

```text
Contribution impact: HIGH

Factors:
✓ Independent reproduction
✓ Complete evidence
✓ Verified domain expertise
✓ No conflict of interest
```

This balances transparency and anti-gaming.

---

# Demo Data Isolation

Demo/sample content must be isolated from real professional reputation.

```text
isDemo = true
```

must imply:

```text
ELSH Score delta = 0
Reputation delta = 0
Achievements = none
Rankings = unaffected
```

This must be enforced by backend logic.

---

# Organizations

Organizations support:

- invitations;
- nested teams;
- grants;
- ownership;
- policies;
- quotas;
- governance.

Membership alone is not repository write permission.

---

# Marketplace

Marketplace architecture includes:

- seller identity;
- seller verification;
- product versions;
- licenses;
- permission scopes;
- installation scopes;
- security review;
- reviews;
- installs;
- moderation;
- audit.

A product must not be published unless:

```text
Seller = VERIFIED
AND
Security Review = PASSED
```

Backend enforcement is mandatory.

---

# Payments Architecture

Payments must use provider abstractions.

```text
Payments Core
   ↓
PaymentProvider
   ├── YooKassa
   ├── Stripe
   ├── PayPal
   ├── Adyen
   └── Future providers
```

The first production provider is expected to be YooKassa.

---

# Payment State

Payment status must come from trusted provider confirmation.

Frontend redirect success is not payment proof.

Example:

```text
CREATED
↓
PENDING
↓
PAID
```

Only a verified provider event may produce:

```text
PAID
```

Additional states include:

```text
FAILED
CANCELLED
REFUNDED
PARTIALLY_REFUNDED
CHARGEBACK
```

---

# Payment Security

ELSHWORK should not store raw payment card data.

Financial operations require:

- provider IDs;
- idempotency;
- webhook validation;
- amount checks;
- currency checks;
- immutable financial audit.

---

# Marketplace Payments

Marketplace payments may involve:

```text
Buyer
↓
Payment Provider
↓
Seller
+
ELSHWORK Commission
```

KYC and payouts must remain external-provider responsibilities.

They must never be simulated internally.

---

# Donations

Donation architecture should reuse Payments Core.

Possible targets:

- ELSHWORK;
- developer;
- researcher;
- project.

Creator payouts require real provider support and compliance.

---

# Notifications

Notifications support multiple channels:

```text
IN_APP
EMAIL
PUSH
WEBHOOK
```

Delivery is persisted.

Possible states:

```text
QUEUED
WAITING_ADAPTER
DELIVERED
FAILED
```

If an external provider is not connected:

```text
WAITING_ADAPTER
```

must be used instead of fake success.

---

# Search

Search uses an abstraction layer.

Development backend may use PostgreSQL.

Production may use:

- OpenSearch;
- Meilisearch;
- other dedicated index engines.

Search must enforce access filtering before returning private data.

---

# Search Privacy

A global search result must never reveal:

- private repository names;
- private code;
- private files;
- restricted Research evidence;

without authorization.

Search indexing does not override permissions.

---

# Moderation

Trust and moderation workflows include:

- reports;
- queue;
- assignment;
- restrictions;
- temporary restrictions;
- bans;
- appeals;
- moderator notes;
- evidence references;
- abuse history;
- seller/product moderation.

Restrictions should affect authentication where required.

---

# Appeals

Users must be able to appeal their own active eligible restriction.

Appeal lifecycle should be backend-controlled.

An accepted appeal may revoke the corresponding restriction.

The history of both restriction and appeal must remain auditable.

---

# AI Architecture

AI should be provider-independent.

```text
ELSHWORK Web
↓
ELSHWORK API
↓
AI Service
↓
AI Provider
```

Possible adapters:

- Ollama;
- OpenAI;
- Anthropic;
- future providers.

Frontend must not talk directly to private AI infrastructure.

---

# Repository-Aware AI

If AI receives repository context:

```text
Request
↓
Authentication
↓
Repository Permission Check
↓
Context Builder
↓
AI Provider
```

The AI layer must never bypass repository access.

---

# Communication

Future communication may include:

- text messaging;
- voice calls;
- video calls;
- screen sharing;
- group rooms.

Voice/video should use WebRTC-based architecture.

---

# Call Architecture

For 1-to-1 calls:

```text
User A
↓
Signaling
↓
WebRTC
↓
User B
```

Production requires:

- STUN;
- TURN;
- explicit media permissions;
- abuse protection.

Group calls should use an SFU architecture.

---

# Internationalization

Localization is a platform-level concern.

Supported or planned languages include:

- English;
- Russian;
- German;
- Spanish;
- French;
- Arabic;
- Simplified Chinese.

Arabic requires proper RTL layout.

Localization must include:

- labels;
- errors;
- empty states;
- loading states;
- confirmations;
- administration;
- Research workflows.

---

# `.elsh` Architecture

The experimental `.elsh` format is intended to become a Persistent Digital Object format.

Conceptual structure:

```text
ELSH Object
├── Identity
├── Provenance
├── History
├── Relationships
├── Intent
├── Trust
├── Permissions
└── Payload
```

The format should eventually be documented through a separate public specification.

---

# Deployment Architecture

Possible initial production topology:

```text
Internet
↓
HTTPS / Reverse Proxy
├── Web
├── API
├── Git Smart HTTP
└── WebSocket

Private Network
├── PostgreSQL
├── Redis
├── Runner
└── Object Storage
```

Database and Redis should not be publicly exposed.

---

# Domain Structure

Possible domain layout:

```text
www.elshwork.com
api.elshwork.com
assets.elshwork.com
status.elshwork.com
```

This may evolve as infrastructure grows.

---

# Production Deployment

Preferred deployment flow:

```text
Source
↓
Typecheck
↓
Executable Tests
↓
Coverage
↓
Quality Gates
↓
Production Build
↓
Nest Bootstrap
↓
Live E2E
↓
Deployment
↓
Health Check
```

A release that fails a required gate should not be promoted.

---

# Quality Gates

ELSHWORK uses executable and source-level gates.

Important areas include:

- TypeScript;
- Prisma schema;
- Nest dependency graph;
- Developer UI;
- Git paths;
- Git Smart HTTP;
- engineering hardening;
- stability;
- production completion;
- RC6 capabilities;
- Research Trust;
- build;
- live E2E.

Source grep/audit checks are not a substitute for executable tests.

---

# Coverage

Coverage thresholds must not be weakened to pass a release.

Current minimum policy:

```text
Lines >= 90%
Branches >= 80%
Functions >= 80%
```

If coverage falls, tests should be improved.

---

# Adapter Philosophy

External infrastructure should always be represented honestly.

Examples:

```text
AI provider missing
→ NOT_CONFIGURED

Email provider missing
→ WAITING_ADAPTER

Search cluster missing
→ fallback or unavailable

Payment provider missing
→ payments disabled
```

The platform must never simulate production success for unavailable integrations.

---

# Architecture Principle

ELSHWORK is not intended to become a collection of tightly coupled features.

The architecture should allow major capabilities to evolve independently while remaining connected through trusted events and explicit interfaces.

---

# Core Connection

The most important architectural relationship is:

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

Repositories preserve source history.

Research describes the claim.

Reproducibility records attempts to reproduce it.

Verification records how trust was established.

Developer Identity turns verified contribution into professional history.

---

# ELSHWORK

**Build. Share. Research. Verify. Ship.**

https://www.elshwork.com
