# push

This step decides how work leaves the machine and who may send it. Without it unreviewed commits reach the remote before any later gate runs. The rows differ in whether the harness or only the agent's step can refuse the push.

**Owns:** Commit shape, how the push runs, and who is allowed to run it.

**Ask the human:** How do you commit and push, and who may push?

**Look for:** whether the agent or the review tool publishes, hook support.

**Defaults:** trigger `agent`, eval `code`.

## Good looks like

- Conventional commits or the repo's format.
- Who may push is stated.

## Quick options

- conventional commits

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- review-gate-hook (hook trigger, code eval) - when: harness with pre-tool hooks - You block the commit unless a gate record covers the digest of exactly this staged patch
  - ref: [mattpocock `git-guardrails-claude-code`](https://github.com/mattpocock/skills/blob/main/skills/misc/git-guardrails-claude-code/SKILL.md) - point at the skill; install only with approval as a fork adapted to this harness, because it retargets the matcher from git verbs to `git commit`, gated on the review record; or reference hook if `security` installed it. The human accept event writes the review record, never the agent
- review-tool-owns-push (hook trigger, code eval) - when: the review tool publishes - You commit locally and stop. A denied-command entry, a blocking hook or unprivileged credentials remove push capability
- retrying-race-aware-push (agent trigger, code eval) - when: version control - On a rejected push you rebase and re-run the whole gate sequence, bounded by an attempt count and a wall-clock deadline
- pr-as-a-step (agent trigger, agent eval) - when: the host has pull or merge requests - You open the request as its own step, with a stated body shape, a linked ticket and evidence that the gates passed
  - ref: [mattpocock `pr`](https://github.com/mattpocock/skills/blob/main/skills/in-progress/pr/SKILL.md) - point at the skill; install only with approval. Alternative: [humanlayer `visual-pr`](https://github.com/humanlayer/skills/blob/main/plugins/visual-pr/skills/visual-pr/SKILL.md) for a visual outline of the change in the body - point at the skill; install only with approval
- prose-review-rule (agent trigger, agent eval) - when: any - You commit only after this change's review passed, first recording the verdict, the reviewers and the staged paths it covers

## Emit

When: committing or pushing

Compiled file: the shape in `references/dsop-template.md`, filled with commit format, who pushes, how the push runs.
