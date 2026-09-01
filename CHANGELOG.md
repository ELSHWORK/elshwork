# ELSHWORK Changelog

All notable public changes to ELSHWORK will be documented in this file.

The format is inspired by Keep a Changelog.

ELSHWORK is under active development, so some features described here may still be evolving before full production release.

---

# Unreleased

## Research Trust & Reproducibility

Planned and in active development:

- immutable Research revisions;
- revision-bound independent reproduction;
- Reproducibility Passport;
- Expected Result locking;
- verification readiness;
- independent reproduction lifecycle;
- evidence references;
- verification history;
- conflict-of-interest handling;
- verified reproduction contributions;
- Developer Identity integration;
- Research cold-start testing;
- external pilot workflow.

## Localization

Ongoing localization work includes:

- English;
- Russian;
- German;
- Spanish;
- French;
- Arabic;
- Simplified Chinese.

Arabic RTL support and additional localization QA are being developed.

## AI

AI provider abstraction is being prepared for:

- local Ollama integration;
- future external AI providers;
- repository-aware assistance;
- Research assistance;
- PR summaries;
- security explanations.

## Payments

Payment infrastructure is planned around a provider abstraction.

Initial direction:

- YooKassa;
- Marketplace payments;
- donations;
- webhooks;
- refunds;
- provider-confirmed payment states.

Future international adapters may include additional global providers.

---

# 7.0.0 RC6 — Ecosystem & Identity Completion

## Developer Identity

Expanded Developer Identity infrastructure with support for:

- ELSH Score;
- Domain Reputation;
- Verified Expertise;
- achievements;
- skill levels;
- contribution history;
- verified contribution events;
- reputation recalculation;
- anti-gaming policies.

Reputation is derived from trusted backend events rather than client-controlled score changes.

---

## Research

Expanded Research infrastructure with:

- revisions;
- persistent citation identity;
- citation export;
- expert review requests;
- review acceptance and decline;
- review deadlines;
- conflict-of-interest handling;
- reproduction infrastructure;
- verification history;
- Research ↔ Repository relationships;
- Research ↔ Author relationships;
- Research ↔ Reviewer relationships;
- Research ↔ Contribution relationships.

Citation export supports:

- plain text;
- Markdown;
- BibTeX.

---

## Marketplace

Expanded Marketplace infrastructure with:

- seller profiles;
- seller verification;
- product versions;
- permission scopes;
- installation scopes;
- licenses;
- verified installs;
- verified reviews;
- security review lifecycle;
- suspend/archive behavior;
- Marketplace audit.

Production publishing policy:

```text
Seller = VERIFIED
AND
Security Review = PASSED
```

Only products that satisfy both conditions may become publicly published.

---

## Organizations

Organization governance was expanded with:

- dashboard support;
- organization policies;
- repository policy defaults;
- quotas;
- organization audit;
- membership governance;
- owner hierarchy;
- team hierarchy;
- repository grants;
- organization-owned resources;
- security settings.

Organization membership alone does not automatically grant repository write access.

---

## Administration Console

Added dedicated Administration infrastructure with scoped roles.

Supported administrative domains include:

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

Scoped roles include:

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

Ordinary platform administration does not automatically grant access to private repository content.

---

## Moderation & Trust

Expanded moderation workflows with:

- reports;
- moderation queue;
- moderator assignment;
- restrictions;
- temporary restrictions;
- bans;
- timeouts;
- appeals;
- appeal review;
- moderator notes;
- evidence references;
- abuse history;
- seller/product moderation;
- security incident controls.

Active eligible restrictions can be appealed by the affected user.

---

## Notifications

Expanded notification delivery infrastructure with:

- unified inbox;
- read/unread state;
- mentions;
- Issue notifications;
- Pull Request notifications;
- review requests;
- Forge notifications;
- Security notifications;
- Marketplace notifications;
- Organization notifications;
- Moderation notifications;
- Research notifications.

Supported delivery channels:

```text
IN_APP
EMAIL
PUSH
WEBHOOK
```

If an external delivery provider is not configured:

```text
WAITING_ADAPTER
```

is used instead of fake successful delivery.

---

## Search

Expanded global search across:

- repositories;
- files;
- code;
- commits;
- issues;
- Pull Requests;
- users;
- organizations;
- teams;
- communities;
- packages;
- Marketplace;
- Research;
- Q&A.

Search architecture supports provider abstraction.

Development deployments may use PostgreSQL.

Future production adapters may include:

- OpenSearch;
- Meilisearch;
- other dedicated search engines.

Private repository and code results remain permission-filtered.

---

## Quality

RC6 introduced additional executable validation for:

- Identity scoring;
- anti-gaming;
- Verified Expertise;
- achievements;
- citation export;
- expert reviews;
- Marketplace publishing policy;
- organization policies;
- scoped admin RBAC;
- private repository admin privacy;
- moderation restrictions;
- appeals;
- notification retries;
- external adapter waiting states;
- private search filtering.

---

# 7.0.0 RC5.1 — Production Workflow Completion

## Pull Requests & Code Review

Completed advanced Pull Request workflows including:

- batch reviews;
- dismissed reviews;
- timeline;
- multi-line review threads;
- suggestions;
- stale approval handling;
- linked issue close keywords;
- CODEOWNERS;
- Forge merge gates;
- source branch handling.

---

## Forge

Expanded Forge with:

- DAG job dependencies;
- cycle detection;
- retries;
- cancellation;
- continue-on-error;
- dependency-aware execution;
- caching policies;
- runner cancellation awareness.

---

## Security

Added and expanded repository scanning for:

- secrets;
- SAST;
- dependencies;
- containers;
- Infrastructure as Code.

Finding reconciliation and external CVE adapter architecture were also introduced.

---

## Packages & Releases

Expanded artifact workflows with:

- object storage abstraction;
- S3-compatible storage;
- binary package uploads;
- SHA-256 checksums;
- immutable package versions;
- release Git tag synchronization;
- release assets;
- provenance;
- SBOM metadata.

---

## Organizations & Teams

Added:

- organization invitations;
- invitation acceptance/decline;
- nested teams;
- repository team grants;
- permission-aware grants;
- repository ownership transfer;
- owner hierarchy protection.

---

## Git

Completed major Git infrastructure including:

- Git Smart HTTP;
- standard clone;
- push;
- fetch;
- fsck validation;
- repository permission enforcement;
- safe path handling;
- arbitrary nested repository folders.

---

## Infrastructure

Production foundations include:

- liveness endpoint;
- readiness endpoint;
- metrics;
- structured logs;
- request IDs;
- backup infrastructure;
- restore infrastructure;
- production adapter documentation;
- cross-platform setup.

---

# 7.0.0 — Platform Foundation

Earlier development established the core ELSHWORK platform architecture.

Major areas introduced during the foundation stage include:

- NestJS API;
- React/Vite frontend;
- Prisma;
- PostgreSQL;
- Redis;
- Docker;
- repository domain;
- authentication;
- permissions;
- communities;
- profiles;
- organizations;
- Research;
- Marketplace;
- Forge;
- security;
- packages;
- releases;
- realtime infrastructure.

---

# Experimental Work

The following areas are exploratory and should not yet be considered stable standards.

## `.elsh`

An experimental Persistent Digital Object format has been explored.

Potential future capabilities include:

- identity;
- provenance;
- history;
- relationships;
- trust metadata;
- intent;
- integrity;
- payloads.

A formal specification has not yet been finalized.

---

## Voice & Video

Future communication infrastructure may include:

- direct voice calls;
- video calls;
- screen sharing;
- group communication;
- repository-context calls;
- Research discussion calls.

WebRTC-based architecture is being considered.

---

# External Infrastructure

Some ELSHWORK features require real external infrastructure before production use.

These may include:

- payment providers;
- KYC;
- seller payouts;
- production object storage;
- production email delivery;
- push providers;
- external AI models;
- hosted Forge runners;
- CVE feeds;
- public cloud credentials;
- dedicated search clusters;
- STUN/TURN infrastructure.

ELSHWORK does not treat unavailable external adapters as successful production integrations.

---

# Development Principles

ELSHWORK development follows several core rules:

- do not rewrite working systems without necessity;
- preserve backend security boundaries;
- keep private repositories private;
- do not reduce test coverage requirements to pass a build;
- add executable regression tests for important bugs;
- avoid fake production integrations;
- preserve audit history;
- build reputation from verified contribution;
- validate major workflows with real users.

---

# ELSHWORK

**Build. Share. Research. Verify. Ship.**

https://www.elshwork.com
