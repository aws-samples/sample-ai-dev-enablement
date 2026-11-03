# teardown

This step releases what the run created that costs money or blocks the next run: worktrees, stacks, pipelines, accounts. Without it the cost stays invisible until a bill or quota refusal, long after the merge. The rows differ in what triggers the release and whether anything verifies it.

**Owns:** Releasing the worktrees, stacks, pipelines and accounts the run created.

**Ask the human:** How do you clean up what a run created?

**Look for:** cloud resources or worktrees a run creates.

**Defaults:** trigger `agent`, eval `code`.

## Good looks like

- Every worktree, stack or account the run created is removed or kept with a reason.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- worktree-teardown-guarded (agent trigger, code eval) - when: worktrees in use - You remove each run worktree, refusing any that is dirty, lacks an upstream, or is ahead of base. Any refusal fails the step. In a workflow that never pushes, drop the upstream and ahead-of-base refusals and keep the dirty-tree refusal
  - ref: [superpowers `finishing-a-development-branch`](https://github.com/obra/superpowers/blob/main/skills/finishing-a-development-branch/SKILL.md) - point at the skill; install only with approval, DSOP step names the refusal conditions
- iac-destroy-ordered (agent trigger, code eval) - when: the repo declares infrastructure as code - You confirm the change landed and the stacks are this run's and unshared, then destroy them in reverse dependency order
- teardown-checklist (agent trigger, agent eval) - when: any - You keep one row per created resource with its disposition: removed, kept with a reason, or left for a person

## Notes

Cloud teardown has no published implementation and every known trigger is human-initiated, so a repo creating cloud resources writes `iac-destroy-ordered` as its own commands.

`worktree-teardown-guarded` and `iac-destroy-ordered` act on different systems, so a repo needing both takes both. `teardown-checklist` covers the rest and lists any resource class no row covers.

## Emit

When: the run created worktrees, stacks or accounts

Compiled file: the shape in `references/dsop-template.md`, filled with teardown commands, what may be kept.
