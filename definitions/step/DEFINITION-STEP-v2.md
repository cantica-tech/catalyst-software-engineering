# `step` — entity definition (v2)

| Field | Value |
|---|---|
| **Entity type** | `step` |
| **Version** | 2 |

## Description

A `STEP-NNNNNN` records one concrete unit of implementation work performed
toward exactly one parent — a `REQ-NNNNNN` requirement or a `BUG-NNNNNN`
bug, named in its `Parent` field — the files touched, commands run, and how
it was verified. Never a rule-linked claim of its own: it inherits its
parent's already-vetted rule target and exists purely to give that parent's
real implementation history a structured, itemized record instead of only
prose. A parent accumulates zero to many steps over its lifecycle and does
not move to a closed state (requirement: `Completed`/`Abandoned`; bug:
`Closed`/`WontFix`) until every listed step is `done` or `abandoned`; a
requirement also needs at least one step before it closes, a bug does not.
Lives in `steps/`, created via `/create-step`.

(v2 corrects v1, which allowed only a requirement as parent and named the
requirement's closed state `done`.)
