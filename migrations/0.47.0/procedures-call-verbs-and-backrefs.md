# Migration 0.47.0 (module 2.5.0): procedures call the kernel's verbs; two back-references declared

> Applies when a deployment of this module is synchronized past kernel
> `0.46.x` to `0.47.0` or later (module `2.4.x` → `2.5.0`). Never re-run
> once applied.

## What changed

1. **Procedures call verbs.** `/create-bug`, `/create-req`, `/create-test`,
   `/create-feature` and `/create-step` run `catalyst new` for their
   mechanical half (ID, template, signature, back-references, index,
   journal); `/show-backlog` reads `catalyst backlog` and `catalyst list`.
   The vetting and the sections stay with the agent.
2. **Two back-references are declared** that the procedures kept by hand:
   a step's `Parent` is cited back in the parent's `Steps`; a test's `Steps`
   is cited back in each step's `Tests`. `catalyst validate` now reports a
   `backref` warning where one is missing.

## Steps

1. **Refresh the module** the ordinary sync way (whole `2.5.0` tree) and
   recompose `CODE-OF-CONDUCT.md`, `Rules-of-Rules.md` and
   `Taskfile.common.yml`.
2. **Back-references.** Run `catalyst validate`; for each `backref` warning
   on a step's `Parent` or a test's `Steps`, show it to the user and, with
   their assent, add the missing citation with `catalyst link <parent-or-step>
   Steps|Tests <id>`. Never rewrite the citing side.
3. **Version.** Record module `2.5.0` in `DEPLOYMENT.md`; journal the files
   touched (`--action sync`, intent naming this migration).

## Rollback

Reverting the module tree to `2.4.x` restores the procedures and drops the
two back-reference declarations; citations added in step 2 stay correct.
