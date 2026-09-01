# ELSHWORK Reproducibility

## Overview

One of the core ideas behind ELSHWORK is simple:

> Publishing a result is not the same as proving that the result can be reproduced.

ELSHWORK is being designed to connect research, source code, environment, execution evidence, independent reproduction and professional reputation into one verifiable workflow.

The purpose is not to create another decorative "verified" badge.

The purpose is to preserve enough evidence to understand:

- what exactly was claimed;
- which version was evaluated;
- which code was used;
- which environment was used;
- what result was expected;
- who independently reproduced it;
- what they actually observed;
- how verification was performed;
- how the verification history evolved over time.

---

# The Core Workflow

A simplified reproducibility flow looks like this:

```text
Research
↓
Revision
↓
Reproducibility Passport
↓
Expected Result
↓
Independent Reproduction
↓
Evidence
↓
Verification
↓
Verified Contribution
↓
Developer Identity
```

Each step represents a real state or relationship.

The result is not just a final status.

It is a history that can be inspected.

---

# Why Revision Locking Matters

Research changes.

Code changes.

Datasets change.

Dependencies change.

Methodology changes.

Expected results change.

Because of this, a reproduction must never verify "the research" in the abstract.

It must verify a specific revision.

Example:

```text
Research A

Revision 1
Expected result: 94.2%

Independent reproductions:
2 VERIFIED

Revision 2
Expected result: 96.4%

Independent reproductions:
0

Status:
NOT YET VERIFIED
```

The two successful reproductions of Revision 1 must not automatically become verification of Revision 2.

This is a fundamental trust invariant.

---

# Research Revision

A Research Revision represents a historical version of a research claim.

A revision may include:

- title;
- abstract;
- methodology;
- expected result;
- interpretation;
- limitations;
- repository;
- commit;
- branch or tag;
- dataset references;
- execution parameters;
- author;
- publication date.

Once a revision is published and used in a reproduction workflow, it should not be silently rewritten.

Meaningful changes should create a new revision.

---

# Expected Result Lock

Before independent reproduction begins, the author declares the expected result.

Example:

```text
Expected result:

Accuracy >= 94.2%
```

Before the first qualifying independent reproduction:

```text
Expected Result
EDITABLE
```

Once reproduction begins:

```text
Expected Result
LOCKED
```

The author must not be able to change it silently.

If the expected result needs to change:

```text
Create Revision 2
```

instead of rewriting Revision 1.

This protects the system from retrospective adjustment of the claim.

---

# Why Expected Result Lock Exists

Without locking, this situation would be possible:

```text
Author publishes:
Expected result = 94.2%

Independent reproducer gets:
91.0%

Author edits expected result:
91.0%
```

The final page might make the reproduction look successful even though the claim changed after the fact.

ELSHWORK should prevent this.

The historical record must remain visible.

---

# Reproducibility Passport

Each Research Revision can have a Reproducibility Passport.

The passport combines technical evidence captured by the platform with explanations provided by the author.

It should not become a large bureaucratic form.

The platform should automatically capture everything it can reliably determine.

---

# Auto-Captured Information

Examples of information that may be collected automatically:

- Research ID;
- Revision ID;
- author identity;
- repository;
- commit SHA;
- branch;
- tag;
- timestamps;
- package manifests;
- dependency lock files;
- runtime;
- dependency versions;
- execution environment;
- Forge run metadata;
- artifacts;
- output checksums;
- execution timestamps.

Possible sources include:

```text
package.json
package-lock.json
pnpm-lock.yaml
yarn.lock
requirements.txt
poetry.lock
pyproject.toml
Cargo.lock
go.mod
Dockerfile
Forge metadata
```

The system should use extensible extractors rather than assume one development ecosystem.

---

# Human-Required Information

Some information cannot be honestly inferred from code or metadata.

The author may need to explain:

## Methodology

What was done and how.

## Expected Result

What result is expected before independent reproduction begins.

## Interpretation

What the result means.

## Limitations

What limitations affect the work.

## Why It Matters

Why the result is important.

Additional information may include:

- non-determinism notes;
- hardware requirements;
- dataset limitations;
- known variability.

ELSHWORK should never pretend that these semantic explanations can always be generated automatically.

---

# Verification Readiness

A Research Revision should have a clear readiness state.

The primary state should be:

```text
READY
```

or:

```text
NOT_READY
```

Not just a percentage.

If the revision is not ready, the reason must be visible immediately.

Example:

```text
Not ready for independent verification

Missing:

- Methodology
- Expected result
- Reproduction instructions
```

This is more useful than simply showing:

```text
82% complete
```

without explaining what is missing.

---

# Passport Completeness

A secondary completeness breakdown may still be useful.

Example:

```text
Technical data        Complete
Methodology           Complete
Expected result       Locked
Environment           Complete
Artifacts             Complete
Limitations           Missing

Verification readiness:
NOT_READY
```

The system should not create a false impression of completeness simply because the automatically collected technical metadata is present.

---

# Independent Reproduction

An Independent Reproduction is a dedicated domain object.

It is not:

- a comment;
- a checkbox;
- a generic review;
- a manually assigned status.

A reproduction may contain:

- reproducer identity;
- target Research Revision;
- source commit;
- environment;
- execution parameters;
- observed result;
- conclusion;
- notes;
- evidence;
- timestamps;
- verification history.

---

# Independence

The author of a Research Revision must not be able to independently reproduce their own work.

Backend rule:

```text
revision.authorId == reproducerId
→ DENIED
```

The same rule must apply even if the frontend hides the button.

The backend remains the security boundary.

---

# Conflict of Interest

A reproduction or expert review may include a conflict-of-interest declaration.

Possible states may include:

```text
NONE
DECLARED
POTENTIAL
CONFIRMED
```

Examples may include:

- same organization;
- co-author relationship;
- direct project participation;
- financial relationship;
- other material conflicts.

A reproduction with a confirmed conflict may remain visible in history, but should not count as a fully independent verification.

History should not be deleted simply because a conflict exists.

---

# Reproduction Lifecycle

A possible state machine:

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

A reproduction may also be:

```text
CANCELLED
```

Every critical transition should record:

- actor;
- timestamp;
- previous state;
- new state;
- reason where applicable.

---

# Verification History

Verification history must not be overwritten.

Example:

```text
SUBMITTED
2026-09-01

UNDER_REVIEW
2026-09-02

NEEDS_CHANGES
Reason:
Missing environment evidence

UPDATED
2026-09-03

UNDER_REVIEW

VERIFIED
2026-09-04
```

The final status may be:

```text
VERIFIED
```

but the path to that status must remain inspectable.

---

# Verification Is Not the Same as Confirmation

This distinction is critical.

A reproduction can be performed correctly and still fail to reproduce the original result.

Example:

```text
Expected result:
94.2%

Observed result:
81.1%

Verification quality:
VERIFIED

Outcome:
NOT_CONFIRMED
```

This means:

- the reproduction was legitimate;
- the evidence was sufficient;
- the process was reviewed;
- the original result was not confirmed.

The platform should not reward only positive outcomes.

Negative reproduction results are also valuable evidence.

---

# Reproduction Outcomes

Possible outcomes may include:

```text
CONFIRMED
PARTIALLY_CONFIRMED
NOT_CONFIRMED
INCONCLUSIVE
```

These outcomes are separate from the verification status of the reproduction itself.

For example:

```text
Verification:
VERIFIED

Outcome:
INCONCLUSIVE
```

is a valid result.

---

# Expert Review vs Independent Reproduction

These are different trust layers.

## Expert Review asks:

> Is the methodology, reasoning or interpretation sound?

## Independent Reproduction asks:

> Can an independent participant reproduce the claimed result?

A research page may therefore show:

```text
Expert Reviews:
2 approved

Independent Reproductions:
3 verified
1 inconclusive
```

There should not be one universal "Verified" badge that hides these distinctions.

---

# Evidence

A reproduction may include evidence such as:

- execution logs;
- Forge runs;
- result files;
- screenshots;
- artifacts;
- dataset references;
- environment snapshots;
- code revisions;
- metrics.

Evidence should record its origin.

Example:

```text
Evidence

Type:
Execution artifact

Source:
Forge Run #128

Repository:
example/project

Commit:
a82c41f

Created:
2026-09-01
```

An external URL alone should not automatically be treated as platform-verified evidence.

---

# Environment Snapshot

Where possible, ELSHWORK may capture environment information such as:

```text
runtime
runtimeVersion
operatingSystem
architecture
dependencyManifest
dependencyLockHash
containerImage
hardware
gpu
```

The system should not claim perfect environment reproducibility when it does not have enough information.

---

# External Datasets

If a dataset is not hosted by ELSHWORK, the system should clearly mark it as external.

Possible metadata:

```text
URL
version
checksum
accessedAt
```

ELSHWORK must not claim to verify an external resource that it did not actually verify.

---

# Forge Integration

If a reproduction is executed through Forge, ELSHWORK may automatically attach:

- pipeline ID;
- run ID;
- job;
- runner metadata;
- runtime;
- execution duration;
- artifacts;
- log references;
- outcome.

Forge should not be mandatory.

A reproduction can also be performed locally with appropriate evidence.

---

# Developer Identity Integration

Independent reproduction is valuable professional work.

A successfully verified reproduction may produce a backend-generated contribution event.

Example:

```text
Independent Reproduction
↓
Verified
↓
VerifiedContributionEvent
↓
Research Contribution
↓
Domain Reputation
↓
Developer Identity
```

The client must never choose the score value.

---

# Reputation

A verified reproduction may influence:

- ELSH Score;
- Domain Reputation;
- Verified Expertise;
- Research achievements;
- contribution history.

The impact may depend on factors such as:

- independence;
- evidence quality;
- domain expertise;
- complexity;
- conflict of interest;
- completeness;
- review quality.

The exact anti-gaming formula does not need to be fully exposed.

But the reasons should be explainable.

Example:

```text
Contribution impact:
HIGH

Factors:

✓ Independent reproduction
✓ Complete execution evidence
✓ Verified domain expertise
✓ No conflict of interest
✓ Reviewed result
```

---

# Anti-Gaming Principles

The system should prevent:

- self-reproduction;
- self-verification;
- duplicate contribution events;
- client-defined score values;
- repeated use of the same evidence to generate unlimited reputation;
- demo activity affecting real reputation;
- backdated client timestamps becoming authoritative;
- silent revision changes after reproduction begins.

Trust-critical logic must be backend-controlled.

---

# Demo Data Isolation

Demo and sample Research must never affect real professional reputation.

Example:

```text
isDemo = true
```

should imply:

```text
ELSH Score delta = 0
Domain Reputation delta = 0
Achievements = none
Rankings unaffected
```

The UI should also visibly label the content:

```text
DEMO
SAMPLE DATA
```

But the label is not the security control.

The backend is.

---

# Example

A simple demonstration:

```text
Research:
Model Benchmark

Revision 1

Expected result:
94.2%

Commit:
a82c41f
```

Reproduction #1:

```text
Observed:
93.9%

Outcome:
CONFIRMED

Verification:
VERIFIED
```

Reproduction #2:

```text
Observed:
94.1%

Outcome:
CONFIRMED

Verification:
VERIFIED
```

The page may show:

```text
Revision 1

Expected:
94.2%

Independent reproductions:
2 verified
```

---

# New Revision Example

The author changes preprocessing and creates Revision 2.

```text
Revision 2

Expected:
96.4%

Independent reproductions:
0

Status:
NOT YET VERIFIED
```

The previous state remains visible:

```text
Revision 1

2 verified reproductions
```

This demonstrates version-bound trust.

---

# Research UI

The user should not need to understand internal architecture terms.

Instead of showing labels such as:

```text
ResearchRevision
EnvironmentEvidence
VerifiedContributionEvent
```

the UI should use simple language:

```text
Research version
Code and version
Environment
Expected result
Independent checks
Observed result
Verification history
```

The architecture may be complex.

The interface should not be.

---

# Evidence Chain

ELSHWORK may use the term **Evidence Chain** internally or in public communication.

The concept represents the connected history:

```text
Research
↓
Revision
↓
Code
↓
Environment
↓
Execution
↓
Independent Reproduction
↓
Verification
```

In the user interface, simpler wording may be preferable.

For example:

> How this result was verified

or:

> Verification history

The final terminology should be validated through cold-start user testing.

---

# Product Validation

The reproducibility workflow should not expand indefinitely without user testing.

Before wider rollout:

1. complete the smallest full workflow;
2. run an internal cold-start test;
3. run an independent cold-start test;
4. fix critical UX blockers;
5. run a pilot with 10–20 users.

The core test scenario:

```text
Publish
↓
Passport
↓
Reproduce
↓
Verify
↓
Reputation
```

---

# Cold-Start Principle

A new user should not need internal ELSHWORK terminology to complete the workflow.

Success means:

- the user knows what to do next;
- technical metadata is captured automatically where possible;
- human input is limited to meaning and interpretation;
- the user understands why the status exists;
- the user can explain what exactly was reproduced.

---

# The Goal

The purpose of this system is not to create a larger number of badges.

The goal is to preserve a verifiable history of trust.

ELSHWORK should be able to answer:

> What was claimed?

> Which version was evaluated?

> What code was used?

> Who tried to reproduce it?

> What did they observe?

> Was the reproduction itself trustworthy?

> Does this verification still apply to the current revision?

---

# The Principle

> **Don't just publish results. Prove they can be reproduced.**

And:

> **Do not trust the badge. Check the evidence.**

---

# ELSHWORK

**Build. Share. Research. Verify. Ship.**

https://www.elshwork.com
