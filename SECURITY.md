# ELSHWORK Security

## Overview

Security is a core architectural principle of ELSHWORK.

The platform is designed around several non-negotiable rules:

- the backend is the security boundary;
- private repositories remain private;
- client-side permissions are never trusted;
- organization membership does not automatically grant repository write access;
- platform administrators do not automatically gain access to private repository content;
- critical trust, moderation, administration and financial actions must be auditable;
- external infrastructure must never be simulated as if it were really connected.

---

# Security Philosophy

ELSHWORK follows the principle of least privilege.

A user, service, team, administrator or integration should receive only the permissions required to perform its intended action.

Access should be explicit.

It should not be inferred from unrelated roles or platform membership.

---

# Backend as Security Boundary

The frontend may hide or disable controls for usability.

But this is not security enforcement.

Every sensitive backend operation must independently validate authorization.

Examples include:

- repository read;
- repository write;
- repository deletion;
- branch modification;
- Pull Request merge;
- organization grants;
- Marketplace publishing;
- Research verification;
- moderation restrictions;
- administrative operations;
- payment transitions;
- private asset access.

A modified frontend or direct API request must never bypass these checks.

---

# Private Repositories

A private repository is private by default.

A user may access it only when they have a valid permission path.

Possible permission sources may include:

- ownership;
- direct collaboration;
- team repository grant;
- organization-level explicit grant;
- approved temporary emergency access.

A private repository must not become readable simply because a user:

- belongs to the same organization;
- is a moderator;
- is a support administrator;
- is a platform administrator;
- knows the repository identifier.

---

# Platform Administrator Privacy

An ELSHWORK administrator does not automatically receive access to private repository contents.

This is an explicit platform invariant.

```text
Platform Admin
≠
Automatic Private Repository Access
```

Administrative capabilities should be scoped to the relevant platform domain.

Examples:

```text
MARKET_ADMIN
→ Marketplace administration

SECURITY_ADMIN
→ Security administration

READ_ONLY_AUDITOR
→ Audit visibility
```

These roles do not automatically imply private source-code access.

---

# Emergency Private Access

Exceptional access to private content may be required in rare operational or security scenarios.

Such access must require:

- an explicit permission;
- a documented reason;
- step-up authentication;
- a limited validity period;
- an immutable audit event.

Example:

```text
Emergency Access

Reason:
Security incident investigation

Approved scope:
Repository X

Expires:
30 minutes

Audit:
Recorded
```

Emergency access should never silently become permanent access.

---

# Repository Permissions

Repository permissions follow least privilege.

Typical role hierarchy:

```text
READ
WRITE
MAINTAIN
ADMIN
OWNER
```

A lower role must never implicitly gain the capabilities of a higher role.

For example:

```text
READ
→ clone/read

READ
→ push
DENIED
```

---

# Organization Membership

Organization membership alone must not grant repository write access.

Correct behavior:

```text
Organization Member
+
No Repository Grant

→ Repository WRITE
DENIED
```

Write permission should come from an explicit repository or team grant.

---

# Ownership

Repository ownership is a critical permission boundary.

Only an authorized owner-level actor should be able to:

- delete a repository;
- transfer ownership;
- perform owner-only governance actions.

Ownership transfers should be:

- authenticated;
- authorized;
- validated;
- audited.

---

# Repository Paths

Repository file operations must prevent path traversal.

Safe repository paths may include arbitrary nested folders.

Examples:

```text
src/components/Button.tsx
research/results/data.csv
Документы/исследование/result.json
```

Unsafe paths must be rejected.

Examples:

```text
../secret
../../etc/passwd
.git/config
/absolute/path
C:\system
```

The backend must normalize and validate paths before using them.

---

# `.git` Protection

User file APIs must not allow writing through normal repository file endpoints into internal `.git` paths.

Examples:

```text
.git/config
.git/objects
.git/hooks
```

must be rejected.

Git internals should only be managed through trusted Git infrastructure.

---

# Git Smart HTTP

Git Smart HTTP endpoints must require repository authorization.

The service name must be explicitly mapped to supported Git operations.

Only approved operations such as:

```text
git-upload-pack
git-receive-pack
```

should be invoked.

Unknown service names must be rejected.

Git protocol headers must be validated before forwarding.

---

# Pull Request Security

Pull Request merge decisions are enforced by backend policy.

Merge requirements may include:

- required approvals;
- CODEOWNERS;
- Forge status;
- security policy;
- stale approval handling;
- repository permissions.

The frontend must not be able to bypass merge gates.

---

# Forge Security

Forge executes user-defined automation and therefore requires strong isolation.

User workloads should not have unrestricted access to:

- the ELSHWORK API host;
- production database credentials;
- Redis credentials;
- unrelated repositories;
- platform secrets;
- host filesystem;
- other runner workloads.

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

Runner isolation should become stronger as public usage grows.

---

# Forge Secrets

Secrets used by Forge should:

- be stored separately from source code;
- be exposed only to authorized runs;
- not appear in normal logs;
- be scoped where possible;
- be revocable.

Secret values should not be returned by ordinary API responses.

---

# Security Scanning

ELSHWORK may provide repository security scanning for:

- secrets;
- SAST;
- dependencies;
- containers;
- Infrastructure as Code.

External vulnerability sources must be represented honestly.

If a real CVE feed is not configured:

```text
CVE Provider:
NOT_CONFIGURED
```

must be reported instead of pretending live vulnerability intelligence exists.

---

# Finding Lifecycle

Security findings may move through states such as:

```text
OPEN
FIXED
DISMISSED
```

A finding should only be marked fixed when the relevant source condition has actually changed.

The platform should preserve enough history to understand why the finding state changed.

---

# Authentication

Authentication should support strong server-side verification.

Security-sensitive authentication features may include:

- secure password hashing;
- JWT or equivalent session tokens;
- refresh/session control;
- MFA;
- TOTP;
- session revocation;
- login restrictions;
- rate limiting.

Authentication secrets must never be embedded in frontend code.

---

# Session Revocation

When a security or moderation restriction requires session invalidation, active sessions should be revoked.

Examples:

```text
LOGIN_BLOCK
SECURITY_RESTRICTION
```

The system must not rely solely on hiding the UI.

---

# MFA and Step-Up Authentication

Sensitive actions may require stronger authentication than normal login.

Examples:

- emergency private access;
- critical admin operations;
- security settings;
- payout changes;
- ownership transfer.

Step-up authentication should be short-lived and scoped to the sensitive operation.

---

# Moderation Restrictions

Moderation may apply temporary or permanent restrictions.

Examples:

```text
LOGIN_BLOCK
TIMEOUT
BAN
```

Restrictions must be enforced by backend guards where applicable.

A user must not bypass an active restriction using direct API calls.

---

# Appeals

Eligible users should be able to appeal their own active restriction.

Appeal decisions must be:

- authorized;
- recorded;
- auditable.

If an appeal is accepted and the associated restriction is revoked, the original restriction history should remain visible in audit history.

---

# Administration

Administrative privileges are scoped.

Possible roles include:

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

A role should not receive unrelated permissions.

Example:

```text
FORGE_ADMIN
→ Forge operation
ALLOWED

FORGE_ADMIN
→ Finance operation
DENIED
```

---

# Immutable Audit

Critical actions should generate append-only audit records.

Examples:

- ownership transfer;
- permission changes;
- emergency access;
- admin role changes;
- moderation restrictions;
- appeal decisions;
- Marketplace security decisions;
- Research verification;
- payment state transitions;
- seller payout actions.

Audit events should record:

- actor;
- action;
- target;
- timestamp;
- reason where applicable;
- relevant metadata.

---

# Developer Identity Security

ELSHWORK professional reputation must not be directly controlled by users.

There must be no public client API equivalent to:

```text
giveMeScore(+100)
```

or:

```text
grantAchievement()
```

Reputation is derived from trusted backend events.

Possible contribution sources include:

- commits;
- merged Pull Requests;
- reviews;
- accepted answers;
- releases;
- packages;
- security contributions;
- Research;
- independent reproductions.

---

# Anti-Gaming

Developer Identity should include protection against manipulation.

Examples:

- duplicate verified events must not double-count;
- self-verification must be denied;
- self-reproduction must be denied;
- demo contributions must not affect real reputation;
- contribution weight must not come from the client;
- client timestamps must not become authoritative;
- repeated evidence must not generate unlimited reputation.

---

# Research Trust Security

Research verification must be revision-bound.

A reproduction of Revision 1 must not verify Revision 2.

Expected results may become locked once qualifying independent reproduction begins.

After lock:

```text
Expected Result modification
→ DENIED
```

Changing the claim requires a new Research Revision.

---

# Independent Reproduction

The original author must not be able to count their own reproduction as independent verification.

Backend invariant:

```text
revision.authorId == reproducerId
→ DENIED
```

The same applies to prohibited self-verification.

---

# Demo Research Isolation

Demo/sample Research must not affect real trust systems.

```text
isDemo = true
```

must imply:

```text
ELSH Score delta = 0
Domain Reputation delta = 0
Achievements = none
Rankings unaffected
```

This must be enforced by the backend.

---

# Marketplace Security

Marketplace products may contain code, integrations or permissions that affect users.

Publishing must therefore be controlled.

Production invariant:

```text
Seller = VERIFIED
AND
Security Review = PASSED
```

Only then may a product move from:

```text
DRAFT
```

to:

```text
PUBLISHED
```

The backend must enforce this rule.

---

# Marketplace Permission Scopes

Products should explicitly declare requested scopes.

Examples may include:

- repository read;
- repository write;
- organization access;
- webhook access;
- profile access.

Requested permissions should be visible before installation.

A product must not silently receive more access than declared.

---

# OAuth Applications

OAuth applications should use explicit scopes.

Authorization should show:

- application identity;
- requested scopes;
- target account or organization;
- relevant security information.

Tokens should be revocable.

---

# Payments

ELSHWORK payment systems must rely on real providers.

The platform must not simulate:

- successful payments;
- KYC;
- payouts;
- card processing.

Payment confirmation must come from the payment provider.

Frontend success alone is not proof of payment.

---

# Payment Webhooks

Payment webhooks should be:

- authenticated or otherwise verified according to provider requirements;
- idempotent;
- checked against expected payment identifiers;
- checked for amount and currency consistency;
- audited.

Repeated provider events must not grant a product twice or duplicate seller credit.

---

# Payment Data

ELSHWORK should not store raw card details.

Financial records may include:

- internal payment ID;
- provider;
- provider payment ID;
- amount;
- currency;
- status;
- order reference;
- seller reference;
- timestamps.

Sensitive payment data should remain with licensed providers.

---

# AI Security

AI integration must not bypass platform permissions.

Correct flow:

```text
AI Request
↓
Authentication
↓
Permission Check
↓
Context Selection
↓
AI Provider
```

The frontend should not directly connect to internal model infrastructure such as a private Ollama endpoint.

---

# Repository-Aware AI

AI context must be limited to repository content the requesting user can access.

A user without access to a private repository must not retrieve its code through AI prompts.

AI systems should not become a permission side channel.

---

# External AI Providers

When external providers are used, ELSHWORK should clearly define:

- what information is sent;
- under which permissions;
- provider status;
- relevant privacy constraints.

Provider availability should be represented honestly.

---

# Search Security

Search must apply access filtering.

A global search engine must never return private repository/code data to unauthorized users.

This remains true even if a dedicated external search cluster contains the indexed document.

Authorization must be applied before results become visible.

---

# Notifications

Notification channels may include:

```text
IN_APP
EMAIL
PUSH
WEBHOOK
```

If an external delivery provider is unavailable:

```text
WAITING_ADAPTER
```

should be used.

The system must not report fake successful delivery.

---

# Object Storage Security

Object storage keys must be validated.

Private objects must not become publicly readable simply because they exist in object storage.

Access may require:

- authenticated application delivery;
- signed URLs;
- scoped permissions.

The exact delivery method may depend on the configured provider.

---

# Avatar and Public Assets

Public assets such as avatars may require cross-origin delivery between web and API domains.

Only assets intentionally classified as public should receive public delivery headers.

Private files must not inherit the same public access policy automatically.

---

# Secrets Management

Secrets must never be committed to the public repository.

Examples:

- database passwords;
- JWT secrets;
- payment API keys;
- payment webhook secrets;
- AI provider keys;
- object storage credentials;
- SMTP credentials;
- private SSH keys.

Configuration should use environment variables or a dedicated secret manager.

---

# Production Credentials

Production credentials must not be included in:

- source code;
- public documentation;
- screenshots;
- example configuration;
- test fixtures.

Example files should use placeholders.

---

# Logging

Logs should provide operational visibility without exposing secrets.

Avoid logging:

- passwords;
- raw authorization tokens;
- private keys;
- payment secrets;
- sensitive personal data;
- repository secrets.

Structured request logs may include request IDs for tracing.

---

# Rate Limiting

Abuse-sensitive endpoints should be rate-limited.

Examples:

- login;
- registration;
- password recovery;
- AI requests;
- messaging;
- calls;
- reports;
- payment creation;
- token creation.

Rate limits should be appropriate to the threat model.

---

# Voice and Video

Future voice/video communication should require explicit browser/user permission for:

- microphone;
- camera;
- screen sharing.

No media device should activate without explicit consent.

Call systems should include:

- blocking;
- caller permission settings;
- spam controls;
- reporting;
- moderation integration.

---

# Dependency Security

Dependencies should be reviewed and updated carefully.

Major upgrades should not be applied blindly simply to eliminate an audit warning if they may destabilize production.

Security updates should consider:

- runtime exposure;
- exploitability;
- compatibility;
- existing mitigation;
- test coverage.

---

# Responsible Disclosure

If you discover a security issue in ELSHWORK, please do not publicly disclose exploit details before maintainers have had a reasonable opportunity to investigate and fix the issue.

Preferred reporting details:

- affected component;
- vulnerability description;
- reproduction steps;
- security impact;
- proof of concept if appropriate;
- suggested mitigation if known.

Do not include unnecessary sensitive user data.

---

# Security Contact

Until a dedicated security reporting channel is published, security contact details should be provided through the official ELSHWORK website:

https://www.elshwork.com

A dedicated security email or reporting portal may be added later.

---

# Out of Scope for Testing

Security researchers should not:

- access other users' private data beyond the minimum needed to prove an issue;
- destroy or alter unrelated data;
- perform denial-of-service attacks;
- use social engineering;
- compromise third-party systems;
- publish secrets or personal information.

A formal vulnerability disclosure policy may expand these rules later.

---

# No Fake Production Integrations

A central ELSHWORK principle is production honesty.

If a service is not connected:

```text
Payment Provider
→ NOT_CONFIGURED

Email Provider
→ WAITING_ADAPTER

AI Provider
→ UNAVAILABLE

External Search
→ NOT_CONFIGURED
```

The platform must not present simulated integration state as real production success.

---

# Security Development Process

Security-sensitive development should include:

```text
Design
↓
Threat Analysis
↓
Implementation
↓
Executable Tests
↓
Security Contracts
↓
Integration Tests
↓
Live E2E
↓
Review
```

Source-level audits are useful, but they do not replace executable security tests.

---

# Quality Requirements

Security changes must preserve platform quality gates.

Current baseline requirements include:

```text
Lines >= 90%
Branches >= 80%
Functions >= 80%
```

Security work should increase test coverage rather than weaken thresholds.

---

# Core Security Invariants

The most important rules can be summarized as:

```text
Backend is the security boundary
```

```text
Private means private
```

```text
Organization membership != repository write access
```

```text
Platform admin != automatic private repository access
```

```text
Client != authority for reputation
```

```text
Frontend success != payment confirmation
```

```text
Demo activity != real reputation
```

```text
Unavailable external integration != success
```

---

# ELSHWORK

Security is not a separate feature.

It is a property of every platform domain.

https://www.elshwork.com
