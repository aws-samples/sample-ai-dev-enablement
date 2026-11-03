# cleanup

This step removes what the change added that nothing reaches: dead branches, unused imports, orphaned files and flags, single-caller abstractions. Without it dead code looks live to the next reader. Nothing published gates this: `done-includes-cleanup` and `self-review-sweep` are the repo's own, and `simplifier-pass` needs a simplification capability installed.

**Owns:** Removing what the change made unnecessary, once it works.

**Ask the human:** How do you remove what a change made unused, including tests that prove nothing?

**Look for:** a dead-code or coverage tool.

**Defaults:** trigger `agent`, eval `agent`. Can send the run back to an earlier step; set a bound.

## Good looks like

- What the change made unused is deleted in the same change.

## Quick options

- dead-code tool (knip, vulture, deadcode)

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- done-includes-cleanup (agent trigger, agent eval) - when: any - You remove any added symbol, file, flag or dependency not reachable from a named entry point, wired both ways and deployable
- self-review-sweep (agent trigger, agent eval) - when: any - Once the feature works you sweep dead branches, unused imports, unreachable guards and over-engineered abstractions, clearing each or recording a finding
- simplifier-pass (agent trigger, agent eval) - when: a simplification capability is available - You run it over the change, then list every sibling file, call site and config it left stale
  - ref: [mattpocock `improve-codebase-architecture`](https://github.com/mattpocock/skills/blob/main/skills/engineering/improve-codebase-architecture/SKILL.md) - point at the skill; install only with approval, scoped to the changed paths only, or [addyosmani `code-simplification`](https://github.com/addyosmani/agent-skills/blob/main/skills/code-simplification/SKILL.md) - point at the skill; install only with approval, keeping behaviour fixed while reducing complexity
- migrate-then-delete (agent trigger, agent eval) - when: the change replaces an API, module or system - You move every caller to the new path, then delete the old one in the same change, or open a dated follow-up for it
  - ref: [addyosmani `deprecation-and-migration`](https://github.com/addyosmani/agent-skills/blob/main/skills/deprecation-and-migration/SKILL.md) - point at the skill; install only with approval
- narrow-react-props (agent trigger, agent eval) - when: a React codebase - You narrow each changed component's prop types to the states live code passes, and delete variants only stories, tests or mocks use
  - ref: [humanlayer `narrow-react-prop-types`](https://github.com/humanlayer/skills/blob/main/plugins/narrow-react-prop-types/skills/narrow-react-prop-types/SKILL.md) - point at the skill; install only with approval

## Notes

A large suite of tests proving nothing is a ticket for `status` to file. An agent told to clean up stops early, so the entry sets a target: "remove the least useful 20% of tests, keep coverage within 2%". `codify-tests`'s test-value gate picks which tests go.

## Emit

When: the change made code unused

Compiled file: the shape in `references/dsop-template.md`, filled with dead-code tool, what to delete.
