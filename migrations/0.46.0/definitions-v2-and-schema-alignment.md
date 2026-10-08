# Migration 0.46.0 (module 2.4.0): corrected definitions, ETDs aligned with the templates

> Applies when a deployment of this module is synchronized past kernel
> `0.45.0` to `0.46.0` or later (module `2.3.x` → `2.4.0`). Never re-run
> once applied.

## What changed

1. **Three definitions get a `v2`** (`definitions/<type>/DEFINITION-<TYPE>-v2.md`;
   `v1` stays as it was, `INVARIANTS.md` INV-23):
   - `bug`: v1 placed bugs in a `requirements/` sibling folder; they live in
     `development/bugs/`, indexed in `development/bugs/bugs.md`. v2 also
     mentions a bug's steps.
   - `step`: v1 allowed only a `REQ-` as parent and called a requirement's
     closed state `done`. A step's `Parent` is a `REQ-` or a `BUG-` (module
     0.31.0 migration, `Rules-of-Rules.md` §21); the parent's closed states
     are `Completed`/`Abandoned` (requirement) and `Closed`/`WontFix` (bug).
   - `backlog`: v1 said "in-progress requirements"; the backlog lists every
     open requirement (`Draft`/`Proposed`/`Vetted`/`Active`).
2. **ETDs (`schemas/`) agree with the templates and rules.**
   - `bug.yaml`: `Severity` is `required: true` (the bug template and
     `CODE-OF-CONDUCT.md` §4 already required exactly one).
   - `roadmap.yaml`: `Linked` is a `ref-list` of `FEAT|REQ`, not a single
     `FEAT` ref (`Rules-of-Rules.md` §10, §21, INV-27). Roadmap rows are
     table rows (`naming: free-form`), so this changes no check result.
   - `step.yaml` gains the optional `Tests` (`ref-list` of `TEST`);
     `feature.yaml` the optional `Roadmap` (`ref` to `RM`) and
     `Requirement(s)` (`ref-list` of `REQ`); `house-keeping.yaml` the
     optional `Priority` (`High`/`Medium`/`Low`) — all fields the templates
     already had.
3. **Templates.** `house-keeping.template.md` gains the `Filename` row every
   other artifact template has. Cross-references corrected in
   `feature.template.md`, `requirement.template.md`, `test.template.md`,
   `backlog.template.md` and `roadmap.template.md` (they named index
   templates, `rules-of-development.md`, or the kernel's `INVARIANTS.md` for
   invariants this module owns).

## Steps

1. **Refresh the module** the ordinary sync way: replace
   `.criterion/modules/software-engineering/` with the whole `2.4.0` tree and
   recompose `CODE-OF-CONDUCT.md`, `Rules-of-Rules.md` and
   `Taskfile.common.yml`.
2. **Definitions.** A sync never overwrites a deployed definition (INV-23).
   For each of `bug`, `step` and `backlog` whose deployed
   `definitions/<type>.md` still reads `**Version** | 1`, tell the user what
   v1 gets wrong (above) and, with their assent, run
   `/migrate-definition <type> 2`. A deployment that keeps v1 is still valid;
   say that it then describes the type incorrectly.
3. **Bugs without a Severity.** `catalyst check` now reports
   `required-field` for every bug with an empty `Severity`. Show the list to
   the user and let them pick each value; **never infer a Severity on their
   behalf**. New dangling references in a step's `Tests`, a feature's
   `Roadmap`/`Requirement(s)` or an unexpected house-keeping `Priority` are
   reported the same way: show them, do not rewrite them silently.
4. **House-keeping template.** If the highest
   `development/house-keeping/templates/TEMPLATE-HOUSE-KEEPING-vN.md` has no
   `Filename` row, add `TEMPLATE-HOUSE-KEEPING-v<N+1>.md` from the module's
   `templates/house-keeping.template.md` and a row for it in the templates
   catalog (INV-20: never edit `vN` in place). The other templates changed
   only in cross-reference wording; refreshing them is optional, and follows
   the same new-version rule.
5. **Version.** Record module `2.4.0` in `DEPLOYMENT.md`; journal every file
   touched (`catalyst journal append --action sync`, `intent` naming this
   migration). Never rewrite the journal itself (INV-17).

## Rollback

Step 2 can be reversed by `/migrate-definition <type> 1`. Step 3 only fills
fields the user chose. Step 4 adds a template version; the previous one
stays. Reverting the module tree to `2.3.1` restores the old ETDs.
