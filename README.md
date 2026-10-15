# Software Engineering Process Module

Standard software engineering process module for Catalyst.

This module declares entity types, template artifacts, and slash-command specifications for managing software engineering processes within a Catalyst deployment.

## Contents

- **`module.yaml`**: The module in catalyst's module format v3: its metadata, tiers, the namespace `SE` of its meta-rules (`rr-SE-NNN`) and invariants (`INV-SE-n`) with `aliases:` for the IDs they had before, and its entity types (`BUG`, `REQ`, `HK`, `TEST`, `STEP`, `FEAT`, `RM`) — fields, states, guards, sections and definitions — from which catalyst generates each type's item template and definition.
- **`version.txt`**: Module version indicator, the same as `module.yaml`'s `version`.
- **`templates/`**: The templates catalyst does not generate: the backlog, a named roadmap's file, and the requirements and features indexes.
- **`code-of-conduct.module.md`**: The module's document types and slash commands (`create-req` and its alias `create-requirement`, `create-bug`, `create-test`, `create-feature`, `create-step`, `show-backlog`, `roadmap` (its old `roadmap-add|remove|update|merge` kept as aliases)), composed into the deployed `CODE-OF-CONDUCT.md` §4; agents get them as prompts of the `catalyst mcp` server, and no command file is written into a project.
- **`definitions/`**: The backlog's frozen, versioned definition (`MODULE-SPECIFICATION.md` §6.3); the entity types' definitions are generated from `module.yaml`.
- **`migrations/`**: One-time migrations for the module's entity shapes, indexed in `migrations/migrations.md`.
- **`INVARIANTS.module.md`**: The module's invariants (INV-5/16/26 module parts, INV-SE-1, INV-SE-2, INV-SE-3, INV-SE-4, INV-SE-5), read together with the kernel's `INVARIANTS.md`.

## The development-artifact layer

The module supplies the dev-artifact layer of catalyst's chain
(work items → dev artifacts → rules → rules of rules). Its grounding type
is `rule`: `REQ-`/`BUG-`/`HK-`/`TEST-` artifacts each target (or propose) a
rule, which belongs to a domain.

- **Ceremony tiers.** Every change is a **chore** (no rule's behaviour
  changes: one journal entry, no artifact), a **fix** (a `BUG-` restoring
  a documented rule, steps optional) or a **feature** (a `REQ-` with at
  least one step before it closes). The agent states the tier before
  starting and escalates if the change grows (INV-SE-4).
- **Features and roadmaps.** Sitting above the chain, `FEAT-NNNNNN`
  feature entries (`features/`) are a separate, optional,
  **non-rule-linked** layer — a place to write down new or future app
  functionality before it's ready to be measured. A feature is never
  itself implemented: when work on one starts, that work is tracked as a
  `REQ-NNNNNN` (never a `BUG-NNNNNN`), vetted against every rule, assigned
  a domain, and measured (INV-SE-1). Features can be bulk-populated from
  named roadmaps (`development/roadmaps/<name>.md`, one file per external
  source ingested via `/roadmap add|update|merge`, retired via
  `/roadmap remove`), which hold `RM-NNNNNN` items triaged into a `FEAT-`
  when someone decides one's worth tracking. A roadmap row's `Linked`
  field names every `FEAT-`/`REQ-` associated with it — a roadmap item of
  real size decomposes into more than one requirement (INV-SE-3).
- **Steps.** Below a requirement or a bug, `STEP-NNNNNN` records
  (`steps/`) are one concrete unit of implementation work — files touched,
  commands run, how it was verified — created via `/create-step` as work
  happens. A requirement needs at least one before it closes; a bug's are
  optional. Neither reaches a closed state until every step opened
  against it is done or abandoned (INV-SE-4).
- **Tests.** `TEST-NNNNNN` records (`tests/`) are a fourth rule-targeting
  dev-artifact type, created via `/create-test` with their own
  `Targets`/`Domain`. A test may also name `(0,n)` requirements and
  `(0,n)` steps it verifies; the other side's `Tests` field is
  back-filled automatically (INV-SE-5).
- **Backlog.** `development/BACKLOG.md` is seeded from
  `templates/backlog.template.md` and overwritten in full by
  `/show-backlog` on every run, which also refreshes every roadmap's
  Status/Linked columns — never hand-edited, not an artifact type, no
  `templates/` of its own (INV-SE-2).

Every dev artifact, step, feature, and roadmap item carries a
`Signed-off-by` field, and every ID carries its signer's `userid` suffix
(INV-16, INV-26).

## Deployed layout

Each entity type gets the kernel's uniform artifact-type shape (kernel
`ARTIFACT-LAYOUT.md`, INV-20: a versioned `templates/` subdirectory, a
`README.md`, and a catalog):

```
    requirements/      requirements.md   TEMPLATE-REQUIREMENT-vN.md
    steps/             steps.md          TEMPLATE-STEP-vN.md
    tests/             tests.md          TEMPLATE-TEST-vN.md
    features/          features.md       TEMPLATE-FEATURE-vN.md
    development/
        roadmaps/      roadmaps.md       TEMPLATE-ROADMAP-vN.md
        bugs/          bugs.md           TEMPLATE-BUG-vN.md
        house-keeping/ house-keeping.md  TEMPLATE-HOUSE-KEEPING-vN.md
        BACKLOG.md     # generated by /show-backlog (INV-SE-2)
```

## Suggested role actions

The kernel's default `roles.json` phrases role actions generically. With
this module active, the typical commands per role are:

| Role | Module commands |
|---|---|
| Product Owner | `/create-req`, approve/prioritize requirements, `/create-feature`, `/roadmap` |
| Scrum Master / Delivery Lead | `/show-backlog` |
| Tech Lead / Architect | vet a requirement's rule target(s) |
| Developer | `/create-bug`, `/create-req` (implementation-driven), implement `REQ-`/`BUG-` |
| QA / Tester | `/create-bug`, `/create-test`, verify a rule's test coverage |
| Stakeholder | propose `FEAT-` ideas, propose roadmap items |

## License

Apache License 2.0 — see `LICENSE`. Contribution and security policies are
those of the [catalyst](https://github.com/oliben67/catalyst) repository.
