# learnings

This step writes what the run learned where the next run reads it, and deletes what no longer holds. Without it every session re-derives the same mistake and the DSOP never improves. The rows differ in where the learning lands: memory, a steering file, or this DSOP.

**Owns:** Writing what the run taught into memory and into the DSOP, and dropping what no longer holds.

**Ask the human:** How do you write down what a run taught, so the next run knows it?

**Look for:** a memory or steering file the agent reads.

**Defaults:** trigger `agent`, eval `agent`.

## Good looks like

- What surprised the run goes to the repo's memory file, each line naming the failure it fixes.
- Lines that never change behaviour get deleted.

## Quick options

- memory file

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- backpass-gradient-step (agent trigger, agent eval) - when: node and npx available - You run a read-only backward pass over past transcripts that proposes memory-file edits. Each needs quotes from two distinct sessions, and a human applies it
  - ref: [backpass](https://github.com/kunchenguid/backpass) - point at the plugin; install only with approval, DSOP step names the two-session evidence rule
- self-editing-sop (agent trigger, agent eval) - when: any - You read the DSOP's learnings section as binding at the start. At the end you append only new learnings, each naming the failure it corrects
- run-retro (agent trigger, agent eval) - when: any - After every merged ticket, you read the ticket history, session log, gate findings, review comments and CI reds. You split lessons: code to the DSOP's learnings section, workflow to `GAPS.md`, domain to a KB draft. You ask about the proposed lessons once, as `approve.md` describes, and file only kept ones
  - ref: [mattpocock `retro`](https://github.com/mattpocock/skills/blob/main/skills/in-progress/retro/SKILL.md) - point at the skill; install only with approval. Alternative: [pstack `reflect`](https://github.com/cursor/plugins/blob/main/pstack/skills/reflect/SKILL.md) to route each learning to a skill edit - point at the skill; install only with approval
- promote-to-skill (agent trigger, agent eval) - when: the learning recurs across runs - You promote a frequently hit learning from a memory line to its own skill, written to be triggered
  - ref: [superpowers `writing-skills`](https://github.com/obra/superpowers/blob/main/skills/writing-skills/SKILL.md) - point at the skill; install only with approval. Alternative: [pstack `correct`](https://github.com/cursor/plugins/blob/main/pstack/skills/correct/SKILL.md) to make a repeated mistake impossible through types, a lint or a test - point at the skill; install only with approval
- cross-run-memory (agent trigger, agent eval) - when: an orchestrator holds state between runs - You keep memory that survives the run, with a metareview pass over it, so the next run learns without a person moving anything
- learnings-hit-feedback (agent trigger, agent eval) - when: any - You log a hit whenever a learning changes what you do, and add new ones at the end. A never-hit entry is deletable

## Notes

Distinct from `domain-kb`: learnings record what a run taught about code and workflow, and change often.

## Emit

When: the run surprised you

Compiled file: the shape in `references/dsop-template.md`, filled with memory file, how to prune.
