# release

No public implementation covers this step; every row is the repo's own.

**Owns:** How the change reaches an environment, how it is promoted, and what limits a bad change's damage.

**Ask the human:** How does a change reach an environment, and what limits a bad one?

**Look for:** deploy target, environments, personal stacks, feature flags.

**Defaults:** trigger `ci`, eval `code`.

## Good looks like

- Deploy separate from release.
- Canary or flag.
- Named alarm and rollback.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- progressive-delivery-contained (ci trigger, code eval) - when: the pipeline can stage a deployment and a metric can gate it - You reach production through a one-box or canary stage with automatic rollback on named metrics. The step names the metrics and the rollback trigger
- flag-decoupled-release (agent trigger, code eval) - when: a feature-flag mechanism exists - You merge dark, then open the flag to a stated group, separating deploy from release. The step names the flag and the error condition that closes it
- release-event-gate (ci trigger, code eval) - when: a CI system exists - You gate on a release approval separate from code review, naming this change's production alarms. A failure opens a routed fix task
- preprod-stack-refresh (agent trigger, code eval) - when: the repo declares a personal cloud stack - At close-out you refresh the continuously deployed pre-prod stack, mirroring the teardown command, and verify the refresh replaced what changed
- promote-path-record (agent trigger, code eval) - when: any - Before claiming a release you check a promote-path file naming two or more environments, each with approval and rollback lines and no placeholder
- launch-checklist (agent trigger, agent eval) - when: the change ships to users - Before release you walk a pre-launch checklist: flags, monitoring, rollback, docs. An unchecked item stops the release
  - ref: [addyosmani `shipping-and-launch`](https://github.com/addyosmani/agent-skills/blob/main/skills/shipping-and-launch/SKILL.md) - point at the skill; install only with approval with its checklists
- instrument-before-release (agent trigger, agent eval) - when: the change runs in production - You add logs, metrics and an alert for the new path before it ships, and name the alert that fires when it fails
  - ref: [addyosmani `observability-and-instrumentation`](https://github.com/addyosmani/agent-skills/blob/main/skills/observability-and-instrumentation/SKILL.md) - point at the skill; install only with approval with its checklist
- rollout-plan-by-change-type (agent trigger, code eval) - when: the repo lists rollout fields per change type - You write the plan's compatibility, flag, rollout steps, metric with threshold, and rollback owner and command before building. `plan`'s `spec-triple-gate` refuses a plan with a missing field
- script-deploy-agent-watch (agent trigger, code eval) - when: a deploy script and a rollback script exist - You run the deploy script per rollout step and never edit it mid-release. You poll the plan's metrics for each step's wait, then step forward or act by the repo's rollback choice: run the rollback script, ask, or report the pipeline's rollback

## Notes

This step decides how a change reaches production and how a bad one is contained. More merged code at unchanged quality means more incidents, and [DORA](https://dora.dev/research/2025/dora-report/) reports AI adoption hurting delivery stability for a second consecutive year. Faster agent merges with review as the only containment raise the incident rate and gain nothing else.

Whatever row is picked, the agent never deploys forward past a breach of a plan metric without a person. `rollout-plan-by-change-type` and `script-deploy-agent-watch` pair: the plan names what `script-deploy-agent-watch` watches.

Every row above the floor needs small releases, which the rest of the DSOP already asks for. A repo that cannot deploy small cannot contain a bad change.

## Emit

When: deploying or releasing

Compiled file: the shape in `references/dsop-template.md`, filled with deploy and release commands, flag or canary, alarm and rollback.
