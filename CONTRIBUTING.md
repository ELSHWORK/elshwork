# Contributing to ELSHWORK

Thank you for your interest in contributing to ELSHWORK.

ELSHWORK is being built as a developer ecosystem focused on code, collaboration, research, reproducibility, verified contribution and professional identity.

We welcome thoughtful contributions that improve the platform, documentation, architecture, security, usability and ecosystem direction.

---

## Ways to contribute

You can contribute by:

- reporting bugs;
- suggesting improvements;
- improving documentation;
- proposing architecture changes;
- submitting Pull Requests;
- reviewing existing issues;
- improving tests;
- reporting security concerns through the proper channel;
- contributing to Research & Reproducibility design;
- improving localization;
- proposing ecosystem integrations.

---

## Before opening an issue

Please check whether a similar issue already exists.

When reporting a bug, include:

- a clear title;
- affected area;
- steps to reproduce;
- expected behavior;
- actual behavior;
- screenshots or logs when useful;
- browser/OS/runtime information when relevant.

Good bug report example:

```text
Area:
Repositories

Issue:
Nested repository path fails to create

Steps:
1. Open repository
2. Click Add file
3. Enter folder/subfolder
4. Enter test.txt
5. Commit

Expected:
folder/subfolder/test.txt is created

Actual:
Request fails
```

---

## Feature requests

Before proposing a new feature, consider whether it strengthens one or more of the core ELSHWORK directions:

- developer productivity;
- collaboration;
- evidence;
- trust;
- professional identity;
- research reproducibility;
- distribution;
- security.

ELSHWORK should not become a collection of unrelated features.

A useful feature request should explain:

- the problem;
- who experiences it;
- why existing functionality is insufficient;
- the proposed behavior;
- security/privacy implications;
- how it fits the ecosystem.

---

## Strategic focus

One of the strongest ELSHWORK directions is the connection between:

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

Contributions that improve this connection are especially valuable.

---

## Repository philosophy

ELSHWORK is developed incrementally.

Please avoid large rewrites unless there is a clear architectural reason.

Preferred approach:

```text
Understand existing behavior
↓
Identify the smallest correct change
↓
Add or update executable tests
↓
Preserve compatibility
↓
Run quality gates
```

Do not replace working systems simply because another implementation appears cleaner.

---

## Security boundaries

The backend is the security boundary.

Pull Requests must never move permission logic into the frontend.

Examples of rules that must remain backend-enforced:

- private repository access;
- repository write permission;
- repository deletion;
- organization grants;
- administrative scope;
- Marketplace publishing;
- Research verification;
- moderation restrictions;
- reputation events;
- payment state.

Frontend checks may improve UX, but are not authorization.

---

## Private repositories

A platform administrator must not automatically gain access to private repository contents.

Organization membership must not automatically grant repository write permission.

Do not weaken these invariants.

---

## Developer Identity

Developer reputation must originate from trusted backend events.

Do not introduce APIs that allow a client to directly set:

- ELSH Score;
- reputation;
- expertise;
- skill level;
- achievement;
- contribution weight.

Trusted events may include:

- commits;
- merged Pull Requests;
- reviews;
- accepted answers;
- releases;
- packages;
- Research;
- independent reproductions;
- security contributions.

---

## Research & Reproducibility

Research verification must remain revision-bound.

A reproduction of Revision 1 must not automatically verify Revision 2.

Expected Result locking and reproduction history must remain backend-controlled.

Do not collapse:

```text
Expert Review
```

and:

```text
Independent Reproduction
```

into one generic verification state.

They answer different questions.

---

## Negative reproduction results

A properly executed reproduction may be verified even if the original result was not confirmed.

Example:

```text
Verification:
VERIFIED

Outcome:
NOT_CONFIRMED
```

Contributions must not reward only positive outcomes.

Honest negative evidence is valuable.

---

## Demo data

Demo/sample content must never influence real reputation or rankings.

Do not create shortcuts where demo events can generate:

- ELSH Score;
- Domain Reputation;
- Verified Expertise;
- achievements.

Demo isolation must remain backend-enforced.

---

## Marketplace

Marketplace publishing must preserve the production rule:

```text
Seller = VERIFIED
AND
Security Review = PASSED
```

Only then may a product become publicly published.

Do not move this rule to frontend-only logic.

---

## External integrations

ELSHWORK uses adapter-based external infrastructure.

Examples include:

- AI;
- payments;
- object storage;
- email;
- push;
- search;
- CVE feeds;
- cloud providers.

Do not implement fake production integrations.

Correct behavior:

```text
Provider missing
→ NOT_CONFIGURED
```

or:

```text
Delivery provider missing
→ WAITING_ADAPTER
```

Never:

```text
Provider missing
→ SUCCESS
```

---

## Payments

Payment state must come from a real provider.

Do not treat frontend redirects as payment confirmation.

Never commit:

- payment secrets;
- API keys;
- webhook secrets;
- card data.

Financial integrations should be implemented through provider adapters.

---

## AI

AI integrations must respect existing permissions.

Repository-aware AI flow should remain:

```text
Request
↓
Authentication
↓
Permission Check
↓
Context Selection
↓
AI Provider
```

AI must never become a side channel for private repository access.

---

## Code style

Match the existing codebase style.

Prefer:

- clear names;
- small focused changes;
- explicit logic;
- strong typing;
- reusable policy functions;
- testable business rules.

Avoid:

- unnecessary abstractions;
- hidden permission logic;
- duplicated domain rules;
- very large unrelated refactors;
- production secrets;
- unexplained magic values.

---

## TypeScript

New TypeScript code should:

- typecheck cleanly;
- avoid unnecessary `any`;
- preserve domain types;
- keep policy logic explicit;
- avoid unsafe type casts used only to silence errors.

If legacy compatibility requires broader types, document the reason.

---

## Testing

New behavior should include executable tests.

Source grep/audit checks alone are not sufficient.

Depending on the change, tests may include:

- unit tests;
- policy tests;
- integration tests;
- live E2E;
- frontend regression tests;
- security tests.

Bug fixes should ideally include a regression test that would have caught the original bug.

---

## Coverage

Do not reduce test thresholds to make a change pass.

Current minimum expectations:

```text
Lines >= 90%
Branches >= 80%
Functions >= 80%
```

If coverage falls, improve the tests.

---

## Live E2E

Security-sensitive or workflow-sensitive changes should be tested through real workflows where practical.

Examples:

```text
User A
→ creates contribution
→ verified backend event
→ reputation recalculated
```

```text
Private repository
→ unauthorized User B searches
→ repository/code not returned
```

```text
Research Revision 1
→ independently reproduced

Research Revision 2
→ created

Revision 2
→ remains unverified
```

---

## Database changes

When modifying Prisma models:

- reuse existing domain models where possible;
- avoid creating duplicate concepts;
- preserve migrations/data compatibility;
- do not silently generate trusted historical data;
- do not mark old records as verified without evidence.

Existing Research data should not become reproduced merely because a new reproduction model is added.

---

## API design

Sensitive API endpoints should explicitly validate:

- authentication;
- authorization;
- target ownership;
- relevant state transition;
- idempotency where needed.

Do not trust client-provided:

- role;
- score weight;
- verification status;
- authoritative timestamp;
- payment status;
- ownership.

---

## State machines

For lifecycle-heavy domains, prefer explicit transitions.

Examples:

```text
DRAFT
↓
SUBMITTED
↓
UNDER_REVIEW
↓
VERIFIED
```

Transitions should reject invalid jumps.

Example:

```text
DRAFT
→ VERIFIED
DENIED
```

unless the domain explicitly allows it.

---

## Audit events

Trust-critical operations should create audit/history records.

Examples:

- role changes;
- permission grants;
- ownership transfer;
- Marketplace moderation;
- Research verification;
- expected-result locking;
- emergency private access;
- financial transitions.

Do not overwrite history for convenience.

---

## UI contributions

UI changes should preserve the existing ELSHWORK visual language.

Requirements:

- responsive behavior;
- mobile width support;
- no overflow;
- loading state;
- empty state;
- error state;
- success state;
- accessible close/cancel actions;
- no dependency on native browser dialogs where shared ELSHWORK components exist.

---

## Localization

User-facing strings should use the shared i18n system.

Do not introduce new hardcoded RU/EN branching such as:

```text
isRu ? "..." : "..."
```

when the shared localization layer should be used.

When changing layouts, consider:

- long translated strings;
- Arabic RTL;
- Chinese typography;
- mobile layouts.

---

## Accessibility

Where practical:

- use semantic elements;
- provide labels;
- ensure keyboard navigation;
- support Escape for appropriate dialogs;
- avoid information conveyed only through color;
- keep text readable at common zoom levels.

---

## Pull Requests

A good Pull Request should:

- address one coherent problem;
- explain the reason for the change;
- describe important design decisions;
- mention relevant tests;
- document security implications;
- avoid unrelated formatting churn.

Suggested description:

```text
## Problem

Describe the issue.

## Solution

Describe the change.

## Security

Explain any permission/privacy impact.

## Tests

List executable tests added or updated.

## Compatibility

Describe migration or compatibility considerations.
```

---

## Review expectations

Reviewers should consider:

- correctness;
- security;
- permission boundaries;
- tests;
- backward compatibility;
- UX;
- performance;
- data migration;
- auditability.

A change that works visually but weakens backend authorization should not be accepted.

---

## Documentation

Significant architectural changes should update relevant documentation.

Possible files include:

```text
README.md
VISION.md
ROADMAP.md
ARCHITECTURE.md
REPRODUCIBILITY.md
SECURITY.md
```

Documentation should describe actual behavior or clearly mark future plans as such.

---

## Experimental features

Experimental work should be clearly marked.

Examples:

- `.elsh` Persistent Digital Object;
- new AI adapters;
- early payment integrations;
- prototype Research trust features.

Do not present experimental behavior as stable production functionality.

---

## `.elsh`

The `.elsh` format is currently experimental.

Contributions around it should avoid prematurely locking the format before the specification matures.

Future work may include:

- format specification;
- parser;
- validator;
- viewer;
- SDK;
- versioning;
- MIME type.

---

## Responsible security research

Potential vulnerabilities should be reported according to `SECURITY.md`.

Do not publish active exploit details before maintainers have had reasonable time to investigate.

---

## Project handoff

Internal release archives use continuity files such as:

```text
PROJECT_HANDOFF.md
PROJECT_STATE.json
```

These are intended to make development continuation explicit across work sessions.

When producing internal release archives, update them with:

- current version;
- completed work;
- tests;
- known issues;
- external adapters;
- next priority.

---

## What we value

We value contributions that are:

- technically correct;
- secure;
- honest;
- testable;
- understandable;
- maintainable;
- useful to developers.

The goal is not to maximize feature count.

The goal is to build a coherent ecosystem developers can trust.

---

## Code of Conduct

Contributors are expected to follow the ELSHWORK Code of Conduct.

See:

```text
CODE_OF_CONDUCT.md
```

---

## Questions

For general project information:

https://www.elshwork.com

Issues and Discussions may be used for public technical discussion as the repository community grows.

---

# ELSHWORK

**Build. Share. Research. Verify. Ship.**
