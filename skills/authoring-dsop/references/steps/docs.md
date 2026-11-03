# docs

This step records what happened to every document the change touched, including ones it made wrong. Without it docs drift and mislead the next agent. The rows differ in whether a doc is paired to code, reviewed, or just updated.

**Owns:** ADRs, runbooks, manuals, and what happens to each doc the change touches. A decision a later change must respect gets an ADR.

**Ask the human:** How do you keep docs and decisions in step with the code?

**Look for:** where docs and ADRs live, docs paired to code.

**Defaults:** trigger `agent`, eval `agent`.

## Good looks like

- Each touched doc updated or left with a reason.
- ADR for a decision later work must respect.
- Diagrams kept as source files and regenerated.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- per-doc-disposition (agent trigger, agent eval) - when: any - You mark one row per doc the change could affect: updated, created, or left as-is with a reason
- adr-on-decision (agent trigger, agent eval) - when: any - When the change makes or reverses a decision later changes must respect, you write an ADR: context, decision, consequences. A contradicting change supersedes the ADR in the same commit
- doc-drift-check (agent trigger, code eval) - when: a code-path-to-doc map can be maintained - The step fails when the diff touches a mapped path but not its paired doc
- written-for-agents (agent trigger, agent eval) - when: the repo's docs are read by agents as well as people - You write each doc for agents: stated triggers, no restated examples, no text whose removal changes nothing
  - ref: [mattpocock `writing-for-agents`](https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-for-agents/SKILL.md) - point at the skill; install only with approval. Alternative: [pstack `technical-writing`](https://github.com/cursor/plugins/blob/main/pstack/skills/technical-writing/SKILL.md) - point at the skill; install only with approval
- diagram-as-artefact (agent trigger, agent eval) - when: the change alters a structure worth drawing - You emit the diagram as a self-contained file from a named layout grammar, and regenerate it instead of hand-editing
  - ref: [diagram-design](https://github.com/cathrynlavery/diagram-design) - point at the skill; install only with approval. Alternative: [humanlayer `show-me`](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md) - point at the skill; install only with approval. Alternative: [visual-explainer](https://github.com/nicobailon/visual-explainer/blob/main/plugins/visual-explainer/SKILL.md) for a self-contained HTML page per diagram, diff review or plan review - point at the skill; install only with approval
- slop-free-prose (agent trigger, code eval) - when: the change ships prose a person reads - You lint the docs for machine-writing tells, then check that facts, numbers, quotes and stated uncertainty survived the edit
  - ref: [slopkit `slopbeth`](https://github.com/ehmo/slopkit/blob/main/skills/slopbeth/SKILL.md) - point at the skill; install only with approval, DSOP step names the lint command and the preservation check. Alternative: [pstack `unslop`](https://github.com/cursor/plugins/blob/main/pstack/skills/unslop/SKILL.md) - point at the skill; install only with approval
- kb-adr-digest (agent trigger, agent eval) - when: the repo holds ADRs or runbooks - You invoke four separate steps: ADR digestion, runbook digestion, staleness checking, reference checking
  - ref: [addyosmani `documentation-and-adrs`](https://github.com/addyosmani/agent-skills/blob/main/skills/documentation-and-adrs/SKILL.md) - point at the skill; install only with approval, DSOP step names the four steps separately
- drafted-decision-greenlit (agent trigger, human eval) - when: the repo keeps ADRs or RFCs - You draft the ADR or RFC in the PR, and the person greenlights it as `approve.md` describes. Accepted, it is merged with its status set; rejected, the finding goes back to plan
- docs-per-change-type (agent trigger, code eval) - when: the repo maps change types to docs - Each change type lists the docs it must update, such as API reference and changelog for a public API change. The step fails when the change type lists a doc the diff leaves untouched

## Notes

`docs-per-change-type` layers over `per-doc-disposition`: it names the docs a change type always needs, and `per-doc-disposition` still covers every other doc the change could affect.

## Emit

When: a doc or decision changes

Compiled file: the shape in `references/dsop-template.md`, filled with which docs pair with which code, ADR path.
