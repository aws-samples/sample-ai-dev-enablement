# ci-watch

This step watches the change after merge and reacts when it breaks. Without it the next person to push inherits someone else's red build. The rows differ in how they observe the build and whether anything keeps watching on a schedule.

**Owns:** Following the change past the merge, and reacting when it breaks.

**Ask the human:** How do you follow a change after merge, and react when it breaks?

**Look for:** a build queryable by API or CLI, a scheduler.

**Defaults:** trigger `ci`, eval `code`. Can send the run back to an earlier step; set a bound.

## Good looks like

- After push, watch CI to green or name the owner of the red.

## Quick options

- `gh run watch` or the CI CLI

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- convergence-loop (ci trigger, code eval) - when: the build and review hosts are queryable by CLI - You loop until a computed boolean holds: a review exists at head and no thread waits on you. Every tenth round you recap and judge
- cron-watch-survives-death (ci trigger, code eval) - when: a scheduler is available - You schedule a five-minute job whose prompt includes the review id, branch, run id and exit condition, and act only on an observed change
- prose-watch-and-report (agent trigger, agent eval) - when: any - You check the build at most three times, recording each look. If green never came, you say so and name its owner
- ci-pipeline-authored (agent trigger, code eval) - when: no CI runs the repo checks yet - You write the pipeline that runs the repo's own checks on every push, so the watch has a build to read
  - ref: [addyosmani `ci-cd-and-automation`](https://github.com/addyosmani/agent-skills/blob/main/skills/ci-cd-and-automation/SKILL.md) - point at the skill; install only with approval

## Notes

`checks`, `gate`, `signoff` and `ci-watch` split one review concern across gates. Order them by evaluator: code, then agent, then human, then what the pipeline triggers. `checks` states which concerns leave review altogether; `signoff` covers what remains.

## Emit

When: after a push

Compiled file: the shape in `references/dsop-template.md`, filled with CI watch command, who owns a red build.
