# Software Engineering Module Laws and Invariants

The law the `software-engineering` process module adds to catalyst's ten
(the kernel's `INVARIANTS.md`), then the invariants it comes from. A session
loads only the law; `catalyst why SE-L1` or `catalyst why INV-SE-n` explains the
rest. This file extends the kernel's and never overrides it
(`MODULE-SPECIFICATION.md` §6.4).

## The law

- **SE-L1 — The tier decides the artifact.** A feature is a requirement
  (`REQ-`) whose steps (`STEP-`) are opened as the work happens, at least one
  before it closes; a fix is a bug (`BUG-`) against the rule it restores; a
  chore changes no rule's behaviour and has no artifact. A feature from the
  roadmap (`FEAT-`) becomes a requirement when work on it starts, never a
  bug.

<!-- catalyst: end of the session brief -->

## The invariants

The module's own invariants are numbered in its namespace, `INV-SE-n`.
Five came from the kernel, which keeps a one-line placeholder for each old
number and never reuses it: INV-9, INV-14, INV-15, INV-27 and INV-28, now
INV-SE-1 to INV-SE-5; `module.yaml` lists them under `aliases:`, so
`catalyst why INV-27` still answers. Entries marked *(module part)* extend
a kernel invariant and keep its number.

| Invariant | Law | Enforced by |
|---|---|---|
| INV-5 chain (module part) | L2, SE-L1 | `validate` |
| INV-SE-1 requirements, not bugs, for new work | SE-L1 | `validate` |
| INV-SE-2 persisted backlog | L6 | `/show-backlog` regenerates it, `check` |
| INV-SE-3 machine-maintained roadmaps | L6 | `/roadmap`, `check` |
| INV-16 role signing (module part) | L7, L4 | `check`, every writing verb |
| INV-26 signed entity IDs (module part) | L7 | `id next`, `check` |
| INV-SE-4 ceremony follows the tier | L2, SE-L1 | journal `tier`, `required_when_closed`, `check` |
| INV-SE-5 tests with optional links | L6 | `/create-test` back-references, `check` |

## The invariants in full

## Structural

- **INV-5 — Chain invariant *(module part)*.** This module's grounding type
  is `rule`: `REQ`/`BUG`/`HK`/`TEST` → rule → domain. `STEP-`, `FEAT-`, and
  `RM-` are exempt from asserting a rule target of their own (INV-SE-1,
  INV-SE-4). With an agile project-management plugin active (kernel INV-22),
  the chain reads `epic → story → task → REQ`/`BUG`/`HK` → rule → domain;
  without one, `REQ`/`BUG`/`HK` chains directly to rule → domain, the same
  way house-keeping's "no rule applies" is already a legitimate, explicit
  answer. A chore (INV-SE-4) carries no artifact and, explicitly, no rule.
- **INV-SE-1 — Requirements, not bugs, for new work.** `FEAT-` entries are
  non-rule-linked roadmap. When work on one starts it becomes a `REQ-` (never a
  `BUG-`), which is vetted against every rule, assigned a domain, and measured.
- **INV-SE-2 — Persisted backlog.** `development/BACKLOG.md` always exists,
  seeded from `templates/backlog.template.md`. It is never hand-edited —
  `/show-backlog` overwrites it in full every run, so it can't drift from
  the real indexes.
- **INV-SE-3 — Machine-maintained roadmap tracking.** `development/roadmaps/`
  and its `roadmaps.md` index always exist (empty is fine); individual named
  roadmaps are created only via `/roadmap add`. In every
  `development/roadmaps/<name>.md`, the Status/Linked columns are set only
  by `/roadmap` and `/show-backlog` — never hand-edited.
  `/roadmap remove` never deletes a roadmap with linked items; it retires
  it in place.
- **INV-16 — Role signing *(module part)*.** Every dev-artifact
  (`BUG-`/`REQ-`/`HK-`/`TEST-`), step, feature, and roadmap item carries a
  `Signed-off-by` field; with kernel 0.93.0 or later, its signer's roles
  must grant the command that writes it (`/create`, or its alias such as
  `/create-req`; `/status`), as for the kernel's.
- **INV-26 — Signed entity IDs *(module part)*.** Every `BUG-`/`REQ-`/
  `HK-`/`TEST-`, `FEAT-`, `RM-`, and `STEP-` ID carries its
  creator/signer's `userid` as a trailing `-XXXXXXXX` suffix, assigned once
  at creation and never changed thereafter, appended after the zero-padded
  sequence number (`Rules-of-Rules.md` rr-META-020).
- **INV-SE-4 — Ceremony follows the tier; steps record the work.** Every
  change is a tier, stated before it starts and escalated if it grows
  (kernel `CODE-OF-CONDUCT.md` §9): a **chore** (no rule's behaviour
  changes) has no artifact — one journal entry, `--tier chore`, no
  targets; a **fix** (restores a documented rule's behaviour) is a `BUG-`
  targeting that rule, steps optional; a **feature** (new or changed
  behaviour) is a `REQ-`, with steps opened as the work happens and at
  least one before it closes (`Steps` is `required_when_closed`).
  `STEP-NNNNNN` names exactly one parent (`Parent`: a `REQ-` or `BUG-`),
  records one concrete unit of work toward it, lives in `steps/` (full
  INV-20 treatment) and inherits its parent's rule target (exempt from
  INV-5's targeting). No requirement or bug reaches a closed state
  (`Completed`/`Abandoned`, `Closed`/`WontFix`) while one of its steps is open (`Rules-of-Rules.md` rr-SE-003). A
  roadmap row's `Linked` field is a list.
- **INV-SE-5 — Tests are development artifacts with optional (0,n) links.**
  `TEST-NNNNNN` (its template generated from `module.yaml`) joined the
  `(BUG|REQ|HK|TEST)` development-artifact format at framework `0.30.0`
  — unlike `STEP-`/`FEAT-`/`RM-`, it is **not** exempt from the chain
  invariant (INV-5): a test always carries its own `Targets`/`Domain`
  and is subject to `CODE-OF-CONDUCT.md` §1. Its own top-level
  `tests/` folder, sibling of `requirements/`/`steps/`, full INV-20
  treatment. Two additional, independent `(0,n)` fields — `Requirements`
  (zero or more `REQ-NNNNNN`) and `Steps` (zero or more `STEP-NNNNNN`)
  it verifies — both optional; a test naming neither is valid as long as
  it still carries `Targets`/`Domain`. Many-to-many: one requirement or
  step may be verified by several tests, and one test may verify several
  requirements and/or steps at once. Back-referenced on the other side:
  a requirement and a step each gain their own `Tests` field, listing
  every `TEST-NNNNNN` that names them — populated automatically by
  `/create-test` in the same action, never hand-edited
  (`Rules-of-Rules.md` rr-SE-004).
