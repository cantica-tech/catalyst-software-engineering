# Migration 0.87.0 (module 3.4.0): an assignee on work, dates on roadmap rows

> Applies when a deployment of this module is synchronized to module `3.4.0` (`3.3.x` → `3.4.0`), owner decision B9.
> Never re-run once applied.

## What changed

1. **`Assignee`**, a registered user, optional, on bugs, requirements, house-keeping items, tests and steps
   (kernel `0.87.0`'s `check` warns on a name that is no registered user).
2. **`Start` and `Due`**, optional dates, on roadmap rows: the roadmap template's table gains the two columns, taken
   from a roadmap's source when it gives them.

## Steps

1. **Refresh the module** the ordinary sync way and recompose `CODE-OF-CONDUCT.md`.
2. **Judgment, optional:** the deployed item templates are frozen (INV-20); a deployment that wants the `Assignee`
   row in new items adds each type's next template version from the module's generated one, journaled. Without it,
   `catalyst new … --field Assignee=<user>` adds the row.
3. **Judgment, optional:** an existing roadmap file may gain the `Start` and `Due` columns, empty for its rows, as a
   journaled edit; nothing reads them as required.
4. **Version.** Record module `3.4.0` in `DEPLOYMENT.md`; journal the files touched.
