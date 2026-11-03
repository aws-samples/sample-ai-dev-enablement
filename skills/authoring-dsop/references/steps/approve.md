# approve

Human points before work starts on the Full task type: `approve-intent` after triage and
`approve-plan` before implement. Without them an agent can build the wrong thing well.

**Owns:** the human decisions to do the work at all, and to accept the plan.

**Ask the human:** Do you want to approve the intent, the plan, or both, before code is written?
This is a policy row: the human decides. Default: both on for the Full task type.

**Look for:** design review rules, RFC or ADR approval, PR templates asking for a plan.

**Defaults:** trigger `agent`, eval `human`. Can send the run back to `research` or `plan`; set a
bound.

## Good looks like

- `approve-intent`: one paragraph of what and why, the triage line, and a yes or no.
- `approve-plan`: the plan, acceptance lines and any UI mockup, linked, with one question.
- The answer is recorded on the ticket or in the session.

## Quick options

- a plan file the human comments on

## Implementations

- needs-you entry (human trigger, human eval) - when: any - You post the summary, the decision
  and links, and wait for the answer.
- plan review page (agent trigger, human eval) - when: the plan has diagrams or UI - You render
  one page with the plan, acceptance and mockups.
  - ref: [visual-explainer](https://github.com/nicobailon/visual-explainer/blob/main/plugins/visual-explainer/SKILL.md) - point at the skill; install only with approval

## Emit

When: a Full task type change needs intent or plan approval

Compiled file: the shape in `references/dsop-template.md`, filled with which approvals are on,
where to ask, and what to link.
