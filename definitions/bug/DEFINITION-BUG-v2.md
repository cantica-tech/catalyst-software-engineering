# `bug` — entity definition (v2)

| Field | Value |
|---|---|
| **Entity type** | `bug` |
| **Version** | 2 |

## Description

A `BUG-NNNNNN` document asserts that an existing, documented rule does not hold
in the running application — it targets one or more rule IDs the observed
behavior violates, never introduces new behavior (that's a requirement's job).
It always carries a Severity (Critical/High/Medium/Low) and a non-empty Targets
field, and may list the `STEP-NNNNNN` steps recording its fix. Lives under
`development/bugs/`, registered in `development/bugs/bugs.md`.

(v2 corrects v1's location: bugs never lived in a `requirements/` sibling
folder, and v1 did not mention a bug's steps.)
