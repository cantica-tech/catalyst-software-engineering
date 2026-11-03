# Migration 0.123.0 (module 3.8.0): Product Owner and Scrum Master run `/roadmap` and `/show-backlog`

> Applies when a deployment of this module is synchronized to module `3.8.0` (`3.7.x` → `3.8.0`). The module needs
> kernel `0.123.0` or later (`roles` in `module.yaml`, fw-IDENTITY-000014). Never re-run once applied.

## What changed

1. `module.yaml` declares `roles`: the kernel's `product-owner` and `scrum-master` roles may run `/roadmap` (with its
   aliases) and `/show-backlog`.
2. `catalyst role document` now prints those commands in both roles. Before, only an Admin could run either one,
   because no role granted them.

## Steps

1. **Refresh the module** the ordinary sync way (`/sync-framework`).
2. **Judgment, with the provider's admin:** give the deployment's provider the new grants, either by running
   `catalyst role document > roles.json` and then `catalyst-users roles provision roles.json` (it adds what is
   missing and deletes nothing), or by granting `cmd:roadmap` and `cmd:show-backlog` to those roles there. If no
   one holds either role, there is nothing to do.
3. **Version.** Record module `3.8.0` in `DEPLOYMENT.md`, and journal the files touched.
