# acceptance

This step states what the change must make true before it is built. Each criterion is a user-visible outcome with an observable pass condition. Without it `validate` becomes a judgement call.

**Owns:** What the change must make true, as user-visible journeys, written before building.

**Ask the human:** How do you say what a change must make true before you build it?

**Look for:** acceptance criteria or user journeys, and where they live.

**Defaults:** trigger `agent`, eval `agent`.

## Good looks like

- Given/when/then or user journeys written before code.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- executable-acceptance-criteria (agent trigger, code eval) - when: a feature-file convention exists or can be added - You write each criterion as a committed, structure-validated feature file in business language, derived from the plan, so the criteria run
  - ref: [`playwright-bdd`](https://github.com/vitalets/playwright-bdd/blob/main/skills/playwright-bdd/SKILL.md) - point at the skill; install only with approval; it refuses implementation until an approved, runnable feature file exists. Built for JavaScript; other stacks port it to their runner
- journeys-before-build (agent trigger, agent eval) - when: any - You write a few concrete user journeys: actor, action, the outcome they see. No edit starts before them, and `validate` validates against these only
  - ref: [pm-skills `deliver-acceptance-criteria`](https://github.com/product-on-purpose/pm-skills/blob/main/skills/deliver-acceptance-criteria/SKILL.md) - point at the skill; install only with approval, for the business-language half: Given/When/Then over happy path, failure and non-functional expectation
- quality-targets-agreed (agent trigger, code eval) - when: the change has non-functional requirements that matter - You agree numeric targets (latency, coverage floor, error budget) before the build and converge on them, so "fast enough" is a number the run can fail
  - ref: [addyosmani `constraint-driven-development`](https://github.com/addyosmani/agent-skills/blob/main/skills/constraint-driven-development/SKILL.md) - point at the skill; install only with approval; it writes thresholds to a contract file and flags a diff that lowers the bar: a new suppression, a skipped test, a threshold edited down
- acceptance-lines-in-plan (agent trigger, agent eval) - when: a written plan exists - You write at least one acceptance line per plan step as an observable outcome. `plan`'s `spec-triple-gate` refuses a plan with none
  - ref: [unlazy](https://github.com/Leonxlnx/unlazy) - adapt, copying its ledger written before the work
- done-statement (agent trigger, agent eval) - when: any - You commit one sentence before the first edit: the user-visible change, and the condition that makes it true

## Notes

`output-style`'s contract governs how you write these. `codify-tests` turns a satisfied criterion into a committed test.

## Emit

When: about to build new behaviour

Compiled file: the shape in `references/dsop-template.md`, filled with acceptance form and where it lives.
