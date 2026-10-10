# Rules of Rules — software-engineering module

> Module contribution (`MODULE-SPECIFICATION.md` §6.1): appended to the
> deployed `rules/Rules-of-Rules.md`, after the kernel's handbook, under
> `### From module software-engineering`. It never replaces kernel content;
> the meta-rules that moved here keep their `rr-META-NNN` IDs.

This module's work is bugs (`BUG-`), requirements (`REQ-`), house-keeping
items (`HK-`) and tests (`TEST-`), each targeting the rules it serves;
features (`FEAT-`), roadmap items (`RM-`) and steps (`STEP-`) record ideas,
directions and work done, and target no rule of their own. Their fields,
statuses and files are the entity definitions' (`definitions/`) and
`ARTIFACT-LAYOUT.md`'s.

## The tier decides the artifact (SE-L1)

| Tier | When | What it needs |
|---|---|---|
| chore | no rule's behaviour changes: a typo, formatting, a comment, a dependency bump, a documentation fix | no artifact: one journal entry, `--tier chore`, no target |
| fix | restores the behaviour a documented rule describes | a `BUG-` targeting that rule; steps optional |
| feature | new or changed behaviour | a `REQ-` vetted against every rule document (`rr-META-001`), targeting or proposing rules; steps opened as the work happens, at least one before it closes; the tests its rules' test plans call for |

A house-keeping item records maintenance that may name "no rule" explicitly.
A bug never introduces a rule; new behaviour is always a requirement.

## 9. `rr-META-009` Features are ideas; requirements are work

A `FEAT-` records a possible future capability — an idea, not a claim about
behaviour. It is never itself implemented or done. When work on it starts,
open a `REQ-` (never a `BUG-`) for the rules it needs; the feature records
which requirements came from it.

## 10. `rr-META-010` Roadmap items decompose into requirements

An `RM-` row records a direction an outside source named. When it is worth
tracking, `/create-feature` opens a `FEAT-` for it; a roadmap item of real
size becomes several requirements, each with its own rules and steps,
rather than one oversized requirement. Its `Status` and `Linked` columns are
derived by `/show-backlog`, never a second source of truth. A named roadmap
whose rows are linked is retired, never deleted.

## 21. `rr-META-021` Steps record the work as it happens

A `STEP-` is one concrete unit of work toward exactly one requirement or
bug (`Parent`): what was changed, run and verified. Open it when that work
starts, close it when it ends — never in advance, never backfilled. A step
inherits its parent's rules. A requirement or bug does not close while one
of its steps is open; a step is not `done` without its Verification, and
`abandoned` needs a reason there.

## 22. `rr-META-022` Tests are grounded like requirements

A `TEST-` targets the rules it verifies, like a bug or a requirement, and
may name the requirements and steps it verifies (both optional, many to
many; `catalyst new` and `catalyst link` keep the back-references). A test
is not `Passing` without its Actual outcome from a real run; `Failing` and
`Disabled` say why there.

## Closing work: what `validate` cannot check

`catalyst validate` checks that a closed requirement names a step. The
rest is judgment: a requirement is `Completed` once its acceptance criteria
and rule targets are in the implementation, and a bug is `Closed` once its
test plan names the test covering the fix.
