# research

This step closes unknowns before the spec commits to an answer. Skipped, an unknown returns as rework, worst with an external service whose behaviour is never fully specified. Its artefact (a research report, a throwaway prototype, a read reference sample) feeds the spec and the implementation.

**Owns:** Naming the unknowns and closing each one, with its artefact, before the spec commits to them.

**Ask the human:** How do you close unknowns before you commit to a design?

**Look for:** disposable environments or accounts, external services the change depends on.

**Defaults:** trigger `agent`, eval `agent`.

## Good looks like

- Unknowns listed and each closed with an artifact (spike, learning test, cited reference sample) before the plan.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- unknowns-before-spec (agent trigger, agent eval) - when: any - You list the change's unverified dependencies (feasibility, external behaviour, what good looks like) and close each with a named artefact before planning
  - ref: [mattpocock `research`](https://github.com/mattpocock/skills/blob/main/skills/engineering/research/SKILL.md) - point at the skill; install only with approval, DSOP step names the artefact each unknown must produce. Alternative: [poteto `how`](https://github.com/poteto/how/blob/main/skills/how/SKILL.md) or [pstack `how`](https://github.com/cursor/plugins/blob/main/pstack/skills/how/SKILL.md) to explain the code before changing it, [pstack `why`](https://github.com/cursor/plugins/blob/main/pstack/skills/why/SKILL.md) for why it is that way - point at the skill; install only with approval
- throwaway-prototype (agent trigger, agent eval) - when: a disposable workspace or environment is available - You build the smallest thing that answers the question somewhere disposable. You keep the finding and discard the code
  - ref: [mattpocock `prototype`](https://github.com/mattpocock/skills/blob/main/skills/engineering/prototype/SKILL.md) - point at the skill; install only with approval, paired with `teardown` so the environment is released
- learning-test (agent trigger, code eval) - when: a test command exists, the unknown is an external behaviour - You answer the unknown with a committed test against the real dependency. The test re-checks whenever that dependency moves
- reference-sample-transfer (agent trigger, agent eval) - when: a working example is in reach - You read a working reference (library example, sibling repo, vendor sample) and cite it in the spec as your pattern
  - ref: [addyosmani `source-driven-development`](https://github.com/addyosmani/agent-skills/blob/main/skills/source-driven-development/SKILL.md) - point at the skill; install only with approval; it refuses any implementation decision not traced to a citable source
- open-questions-list (agent trigger, agent eval) - when: any - You keep one line per unknown: answered with a pointer, accepted risk, or open and blocking. Any blocking line stops planning

## Notes

Nothing published blocks planning on this list, so only `open-questions-list` enforces the step.

Whichever row you pick, `plan` cites the artefact as a constraint's source and `implement` cites it where the code follows it. Uncited, it looks like skipped research.

This step sets which artefacts research or a prototype hands over for review: a report, screenshots, a video, a running deploy URL, a demo script. The agent proposes them and the user keeps or drops each. Whatever is chosen, a report states the question it answers, and a running prototype has a TTL; `teardown` removes it once past.

## Emit

When: an unknown blocks the design

Compiled file: the shape in `references/dsop-template.md`, filled with how unknowns are closed, and where the artifact goes.
