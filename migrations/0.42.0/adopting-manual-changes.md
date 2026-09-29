# Migration 0.42.0 (module 2.3.0): adopting changes made outside catalyst

> Apply together with the kernel's `migrations/0.42.0/changes-outside-catalyst.md`
> when a deployment of this module is synchronized past kernel `0.41.0` to
> `0.42.0` or later (module `2.2.0` → `2.3.0`). Never re-run once applied.

## What changed

1. **What an adopted change becomes** (`code-of-conduct.module.md` §4, the
   paragraph on `/adopt`). The kernel's `/adopt` accepts a product change
   committed outside catalyst into the journal; this module says what the
   change's tier requires: a chore is only the adopted entry, a fix a
   `BUG-`, a feature a `REQ-` with at least one `STEP-` recording the
   commit's work. Artifacts written this way say so (`Adopted from commit
   <sha>, written outside catalyst by <author>`), a step starts `done`, and
   no date is back-dated — the honest exception to INV-29, which otherwise
   forbids writing artifacts after the work.

No schema, template or definition changes.

## Steps for every deployment

1. **Recompose** `CODE-OF-CONDUCT.md` with the kernel's migration (the same
   `catalyst recompose` run): this module's §4 gains the `/adopt` paragraph.
2. **Refresh the module** tree in `.criterion/modules/software-engineering/`
   from the `2.3.0` release, and record module `2.3.0` in `DEPLOYMENT.md`.
3. **Journal** both with the kernel migration's entry.

## Rollback

Restore the `2.2.0` module tree and the previous `CODE-OF-CONDUCT.md`. No
artifact or journal line changes.
