# Migration 0.83.0 (module 3.3.0): decisions are entities

> Applies when a deployment of this module is synchronized to module `3.3.0` (`3.2.x` → `3.3.0`); the module needs
> kernel `0.83.0` or later, whose sync makes the new type's folder (an older one brought its definition only). Never re-run once applied.

## What changed

1. **A new type, `ADR-` (Decision)**, in `decisions/`, registered in `decisions/decisions.md`: its `Rules` (the
   rules it shapes), `Decided`, `Decided by`, `Supersedes` and its derived `Superseded by`; states `Proposed`,
   `Accepted`, `Rejected`, `Superseded`. Not grounded and not work: no tier, no steps.
2. **`/create adr`** (`/create-adr`) records one.
3. **`rr-SE-005`**: a decision is recorded, and superseded, never rewritten.

## Steps

1. **Refresh the module** the ordinary sync way (whole `3.3.0` tree) and recompose `CODE-OF-CONDUCT.md`,
   `Rules-of-Rules.md` and `Taskfile.common.yml`. `catalyst sync apply` creates the decision's definition, a new
   type's only (INV-23), and `catalyst index regen` its empty index.
2. **Judgment, optional:** if the project already keeps decision records outside the criterion (an `adr/` or
   `decisions/` folder of the product repository), offer to record each as an `ADR-` with the user's assent, in its
   original order, with its original date and author under `Decided` and `Decided by`, and the rules it shapes under
   `Rules`; say where each came from in its Context. Never invent a decision's author or date: ask, or leave the
   field blank.
3. **Version.** Record module `3.3.0` in `DEPLOYMENT.md`; journal the files touched (`--action sync`, intent naming
   this migration).
