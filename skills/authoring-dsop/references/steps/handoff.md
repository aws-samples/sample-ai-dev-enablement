# handoff

This step hands the next session everything it needs that is not in the code. Without it the next run re-derives context already paid for, and a stale handoff file misleads while looking authoritative. The rows differ in what stores the state and what keeps it current.

**Owns:** Everything the next session needs that the code lacks, including where and why the run left the plan.

**Ask the human:** How does the next session pick up, including where this run left the plan?

**Look for:** persisted transcripts or session state.

**Defaults:** trigger `agent`, eval `agent`.

## Good looks like

- A next session can resume: goal, state, next step, rejected approaches, where the run left the plan and why.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- resumable-handoff-note (agent trigger, agent eval) - when: any - You keep a handoff note on the ticket or PR: goal, where the run stopped, next action, rejected approaches, pointers, deferred findings, plan deviations with reasons. You re-read it cold to confirm it suffices
  - ref: [mattpocock `handoff`](https://github.com/mattpocock/skills/blob/main/skills/productivity/handoff/SKILL.md) - point at the skill; install only with approval, DSOP step names the sufficiency test. Alternative: [pstack `show-me-your-work`](https://github.com/cursor/plugins/blob/main/pstack/skills/show-me-your-work/SKILL.md) for a decision log beside it - point at the skill; install only with approval
- trajectory-handover (agent trigger, agent eval) - when: the harness persists transcripts - You export the session's turns append-only, beside a metadata file naming run, harness, model, times, goal and the handoff note
  - ref: [Entire](https://entire.io/) - point at the tool; install only with approval, DSOP step names the conversation artefact as part of the change
- session-log-artefact (agent trigger, code eval) - when: the user chose a session log as an output - You export the session, run the repo's secret scrub over it, and attach it to the PR as a comment, a file or a link. A missing or unscrubbed log fails the step

## Emit

When: ending a session with work left

Compiled file: the shape in `references/dsop-template.md`, filled with state note location and contents.
