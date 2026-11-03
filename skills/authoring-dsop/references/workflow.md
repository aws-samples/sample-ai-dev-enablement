# Default workflow

The compiled DSOP copies this table into its `SKILL.md`, under `## Workflow`, and adjusts it
to the repo. An `org/workflow.md` replaces it. The user may change any step, rule or task type.
Levels and task type shares are score data: they go to `DECISIONS.md`, not into the table.

## Triage

These are the default task types. While compiling, `triage` proposes the repo's own types from
its tickets and commit history, with criteria and examples. Each type is one column below.
Rate the change, then pick the first type that fits:

| type | when |
|---|---|
| **Docs** | docs only, low risk |
| **Fix** | bug or chore, low risk and low complexity |
| **Full** | anything else: feature, high risk or high complexity |

- **Type:** docs, bug, chore, feature. A chore is a refactor, dependency bump, revert or
  config change.
- **Risk:** high when the change touches auth, data, money, a public API or production config.
- **Complexity:** high when the change touches more than one module or service, needs a new
  dependency, schema or API, or the agent cannot state the fix in one line.
- The highest rating wins. Unsure means high.
- On **Full**, triage runs again after `research` and may move the change to **Fix**.

## Steps

Run the steps of your task type top to bottom.

| step | file | Docs | Fix | Full |
|---|---|---|---|---|
| intake | `steps/intake.md` | ? | ? | ? |
| ready | `steps/ready.md` | * | * | * |
| triage | `steps/triage.md` | * | * | * |
| approve-intent | `steps/approve.md` |  |  | H |
| research | `steps/research.md` |  |  | ? |
| plan | `steps/plan.md` |  |  | * |
| acceptance | `steps/acceptance.md` |  |  | * |
| ui-design | `steps/ui-design.md` |  |  | ? |
| approve-plan | `steps/approve.md` |  |  | H |
| implement | `steps/implement.md` |  | * | * |
| validate | `steps/validate.md` |  | * | * |
| codify-tests | `steps/codify-tests.md` |  | * | * |
| security | `steps/security.md` |  | * | * |
| checks | `steps/checks.md` | * | * | * |
| cleanup | `steps/cleanup.md` |  | * | * |
| docs | `steps/docs.md` | * | ? | * |
| domain-kb | `steps/domain-kb.md` |  |  | ? |
| commit | `steps/push.md` | * | * | * |
| gate | `steps/gate.md` | * | * | * |
| push | `steps/push.md` | * | * | * |
| signoff | `steps/signoff.md` |  |  | H? |
| ci-watch | `steps/ci-watch.md` | ? | ? | ? |
| release | `steps/release.md` |  | ? | H? |
| teardown | `steps/teardown.md` |  | ? | ? |
| status | `steps/status.md` | ? | ? | ? |
| learnings | `steps/learnings.md` | ? | ? | ? |
| handoff | `steps/handoff.md` | ? | ? | ? |

Any mark means: read the file before you act. The file's `When:` line says when the step runs.

| mark | means |
|---|---|
| `*` | required; skip only with a one-line reason, which the gate checks |
| `?` | runs when the file's `When:` line holds |
| `H` | a person decides; `H?` when the file's `When:` line holds |
| empty | not on this task type |

The rules below go into each compiled step file's `When:` line, not into the table:

- `intake`: work comes from a ticket system; list, claim with a lease, release on stop
- `approve-intent`, `approve-plan`: see `steps/approve.md`
- `research`: an unknown blocks the design; then triage again
- `ui-design`: a UI changes
- `docs`: on Fix, when behaviour or an interface changed
- `domain-kb`: outer behaviour changed
- `commit`: local, not gated; the gate reviews this commit range
- `gate`: before every confirmed mutating action
- `signoff`: high risk, or policy requires it
- `ci-watch`: after push; if red, back to `implement` (Docs: `docs`), max 3
- `release`: the repo deploys; `H` when policy requires it
- `teardown`: the run created worktrees, stacks or accounts
- `status`: a ticket system or team chat exists; after claim, push or PR, when blocked, at handoff
- `learnings`: something surprised the run
- `handoff`: work is left for another session

Every step follows `output-style`. The order is a default; the agent may reorder with a
one-line reason.

## Task type levels

`DECISIONS.md` records one line per task type. A task type's level is the lowest level of its
required steps, and 3 only with no `H` step:

```
Docs 3 | Fix 1 (validate is level 1; H: release) | Full 2 (H: approve-intent, approve-plan, signoff, release)
```
