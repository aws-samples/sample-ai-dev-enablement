# plan

This step turns intent into something a later step can refuse. Without it the plan exists only in the agent's context, so no gate can spot a deviation. The rows differ in what may refuse the plan: a gate, critics, or the author's own answers.

**Owns:** Intent to spec to plan, the change's risk class and tenets, and the constraints that let you refuse a plan.

**Ask the human:** How do you get from intent to a plan, and how risky is this change?

**Look for:** a spec or plan template, tenets, ADRs.

**Defaults:** trigger `agent`, eval `agent`.

## Good looks like

- Plan sized to risk.
- Risk class by what it touches.
- ADRs and tenets that constrain it named.
- Domain and module boundaries drawn before a plan that crosses them.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- spec-triple-gate (agent trigger, code eval) - when: version control - You stay in planning until the requirements, design and plan files exist, carry no placeholder, and declare at least one acceptance line
- brainstorm-then-plan (agent trigger, agent eval) - when: any - You question openly until scope is agreed, then write the plan from that scope as a separate step
  - ref: [superpowers `brainstorming`](https://github.com/obra/superpowers/blob/main/skills/brainstorming/SKILL.md) + [`writing-plans`](https://github.com/obra/superpowers/blob/main/skills/writing-plans/SKILL.md) - point at the skill; install only with approval, DSOP step names both in order. Alternative: [addyosmani `interview-me`](https://github.com/addyosmani/agent-skills/blob/main/skills/interview-me/SKILL.md) for the questioning half - point at the skill; install only with approval
- spec-from-conversation (agent trigger, agent eval) - when: any - You turn the agreed intent into a committed spec, then into tickets that each trace to a spec line
  - ref: [mattpocock `to-spec`](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-spec/SKILL.md) + [`to-tickets`](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-tickets/SKILL.md) - point at the skill; install only with approval. Alternative: [addyosmani `spec-driven-development`](https://github.com/addyosmani/agent-skills/blob/main/skills/spec-driven-development/SKILL.md) + [addyosmani `planning-and-task-breakdown`](https://github.com/addyosmani/agent-skills/blob/main/skills/planning-and-task-breakdown/SKILL.md) - point at the skill; install only with approval
- design-before-plan (agent trigger, agent eval) - when: the change crosses module or domain boundaries - You model the domain and module boundaries before planning, so the plan cannot invent an architecture by accident
  - ref: [mattpocock `codebase-design`](https://github.com/mattpocock/skills/blob/main/skills/engineering/codebase-design/SKILL.md) + [`domain-modeling`](https://github.com/mattpocock/skills/blob/main/skills/engineering/domain-modeling/SKILL.md) - point at the skill; install only with approval. Alternative: [pstack `architect`](https://github.com/cursor/plugins/blob/main/pstack/skills/architect/SKILL.md) for types and signatures first, [addyosmani `api-and-interface-design`](https://github.com/addyosmani/agent-skills/blob/main/skills/api-and-interface-design/SKILL.md) for public interfaces - point at the skill; install only with approval
- plan-critics (agent trigger, agent eval) - when: harness supports subagents - You run four critic roles over the plan (acceptance, design, user experience, strategic fit) and answer their consolidated objections in it
  - ref: [addyosmani `doubt-driven-development`](https://github.com/addyosmani/agent-skills/blob/main/skills/doubt-driven-development/SKILL.md) for a fresh-context critic briefed to disprove, with [superpowers `dispatching-parallel-agents`](https://github.com/obra/superpowers/blob/main/skills/dispatching-parallel-agents/SKILL.md) to run one per dimension in parallel - point at the skill; install only with approval, DSOP step names the dimensions
- grill-the-plan (agent trigger, agent eval) - when: any - You question the plan one gap at a time and close each with your own answer before the next question
  - ref: [mattpocock `grilling`](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md) - point at the skill; install only with approval
- risk-and-tenets (agent trigger, agent eval) - when: any - You classify the plan's risk (low, medium, high) by what it touches: data, auth, public API, deploy path. You name the tenets and ADRs it follows. The class sets `release`'s containment and whether `signoff` needs a person
- probe-register (agent trigger, agent eval) - when: any - You run one feasibility spike and one disconfirming pass per unknown, and record both. Any open unknown blocks the plan
- research-design-outline (agent trigger, agent eval) - when: any - You research the code into a document, settle the design choices with the user in a second, outline the implementation, then implement from the outline. Each phase starts a fresh session and reads only the documents
  - ref: [humanlayer `rpi`](https://github.com/humanlayer/skills/blob/main/plugins/rpi/skills/rpi/SKILL.md) - point at the skill; install only with approval with its subagents; a Claude Code plugin, so fork and adapt the subagents for another harness
- vertical-slices (agent trigger, agent eval) - when: triage complexity is high - You split the plan into vertical slices, each one PR with its own tests, proof and rollout fields. A dependent slice names its prerequisite. Every acceptance criterion maps to a slice, and no slice ships one layer only

## Notes

A repo takes `risk-and-tenets` for risk beside its planning row. `vertical-slices` layers over any planning row: it decides how the plan splits, never how it is made.

## Emit

When: the change adds behaviour or crosses modules

Compiled file: the shape in `references/dsop-template.md`, filled with plan form, risk class, ADRs and tenets that constrain it.
