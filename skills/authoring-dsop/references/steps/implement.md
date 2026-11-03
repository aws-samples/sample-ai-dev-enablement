# implement

This step writes the code: the order of edits, the size of an increment, and what ends coding. Without it the agent edits in the order it noticed things and stops when it feels finished.

**Owns:** Plan to working code: test-first order, increment size, and the condition that ends coding.

**Ask the human:** How do you write the code: test first, and in what increments?

**Look for:** the test command, mutation or coverage tooling.

**Defaults:** trigger `agent`, eval `agent`. Can send the run back to an earlier step; set a bound.

## Good looks like

- TDD red-green-refactor, small increments, debug root cause before patching.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- test-first-increment (agent trigger, agent eval) - when: a test command exists - You write a failing test naming the behaviour, then edit the implementation. One behaviour per increment; the suite is green before the next
  - ref: [superpowers `test-driven-development`](https://github.com/obra/superpowers/blob/main/skills/test-driven-development/SKILL.md) or [mattpocock `tdd`](https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md) - point at the skill; install only with approval, DSOP step names it as the required read before the first edit. Alternative: [addyosmani `test-driven-development`](https://github.com/addyosmani/agent-skills/blob/main/skills/test-driven-development/SKILL.md), or [pstack `tdd`](https://github.com/cursor/plugins/blob/main/pstack/skills/tdd/SKILL.md) where the test target is cheap - point at the skill; install only with approval
- plan-step-execution (agent trigger, agent eval) - when: a written plan exists - You work the plan step by step, marking each done in the plan file. Where the plan runs out, you stop and do not improvise
  - ref: [superpowers `executing-plans`](https://github.com/obra/superpowers/blob/main/skills/executing-plans/SKILL.md) or [mattpocock `implement`](https://github.com/mattpocock/skills/blob/main/skills/engineering/implement/SKILL.md) - point at the skill; install only with approval. Alternative: [addyosmani `incremental-implementation`](https://github.com/addyosmani/agent-skills/blob/main/skills/incremental-implementation/SKILL.md) - point at the skill; install only with approval
- spec-driven-implement (agent trigger, agent eval) - when: a committed spec exists - You implement from the spec, ignoring the ticket text. A spec line with no test is an open question; you never guess
  - ref: [mattpocock `implement-spec`](https://github.com/mattpocock/skills/blob/main/skills/in-progress/implement-spec/SKILL.md) - point at the skill; install only with approval
- delegated-increments (agent trigger, agent eval) - when: harness supports subagents - You give each independent plan step to one subagent; a fixed contract is all they share. You integrate the work
  - ref: [superpowers `subagent-driven-development`](https://github.com/obra/superpowers/blob/main/skills/subagent-driven-development/SKILL.md) + [`dispatching-parallel-agents`](https://github.com/obra/superpowers/blob/main/skills/dispatching-parallel-agents/SKILL.md) - point at the skill; install only with approval
- mutation-hardened-increment (agent trigger, code eval) - when: a mutation testing tool exists - You close an increment only when every surviving mutant in the changed lines is killed or recorded as accepted
  - ref: [trailofbits `mutation-testing`](https://github.com/trailofbits/skills/blob/main/plugins/mutation-testing/skills/mutation-testing/SKILL.md) - point at the skill; install only with approval, DSOP step names the surviving-mutant condition
- debug-before-patch (agent trigger, agent eval) - when: any - On an unexpected failure you find the cause before changing anything, and record it with the fix
  - ref: [superpowers `systematic-debugging`](https://github.com/obra/superpowers/blob/main/skills/systematic-debugging/SKILL.md) or [mattpocock `diagnosing-bugs`](https://github.com/mattpocock/skills/blob/main/skills/engineering/diagnosing-bugs/SKILL.md) - point at the skill; install only with approval. Alternative: [addyosmani `debugging-and-error-recovery`](https://github.com/addyosmani/agent-skills/blob/main/skills/debugging-and-error-recovery/SKILL.md) - point at the skill; install only with approval
- plan-step-ledger (agent trigger, agent eval) - when: any - You keep one line per plan step with its state and proving artefact. Coding ends when every line is closed or explicitly deferred

## Emit

When: writing code

Compiled file: the shape in `references/dsop-template.md`, filled with test command, increment size, debugging rule.
