# Migration 0.122.0 (module 3.7.0): each creating type names its command

> Applies when a deployment of this module is synchronized to module `3.7.0` (`3.6.x` → `3.7.0`); the module needs
> kernel `0.122.0` or later (`created_by`, fw-STRUCTURE-000026). Never re-run once applied.

## What changed

1. **`created_by: /<command>`** is declared on six types, each naming the command that creates it:
   `BUG` → `/create-bug`, `REQ` → `/create-req`, `TEST` → `/create-test`, `STEP` → `/create-step`,
   `FEAT` → `/create-feature`, `ADR` → `/create-adr`.
2. For each of these types the kernel composes **no generic `/create <type>` part**, and `catalyst new <PREFIX>`
   refuses unless it runs with `--command` naming the declared command. The module's own commands already pass
   `--command`, so their behaviour does not change.
3. `HK` (house-keeping) keeps the generic create: it has no creating command in the module's catalogue.

## Steps

1. **Refresh the module** the ordinary sync way (`/sync-framework`) and recompose `CODE-OF-CONDUCT.md`.
2. **Check the declared commands are the ones the deployment runs.** `catalyst spec create <type>` gives the declared
   command's procedure for each type above.
3. **Version.** Record module `3.7.0` in `DEPLOYMENT.md`; journal the files touched.
