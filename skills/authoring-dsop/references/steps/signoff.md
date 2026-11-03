# signoff

A human point after `push`: a person reviews the pushed change before it merges. It runs when
triage risk is high or policy requires it. Without it a risky change merges on agent review
alone.

**Owns:** whether a person signs off a pushed change, and what they decide on.

**Ask the human:** Does a person sign off a change: required, optional, or not at all? This is a
policy row: the human decides, with no default.

**Look for:** PR approval rules, CODEOWNERS, branch protection.

**Defaults:** trigger `agent`, eval `human`. Runs after `push` when triage risk is high or
policy requires it. Can send the run back to `implement`; set a bound.

## Good looks like

- Policy set by the human: required, optional or none.
- A request carries a short summary, the decision asked, and links to every proof artifact.
- The person judges what only a person can: the reason for the change, whether names fit, and
  whether a domain boundary moved. `checks` covers the rest.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- pr-approval-rule (ci trigger, human eval) - when: the host has branch protection or
  CODEOWNERS - The host requires an approval from a named owner before merge.
- needs-you-entry (human trigger, human eval) - when: any - You ask as `approve.md` describes.
- visual-review-page (agent trigger, agent eval) - when: the user chose a review page for this
  decision - You generate one HTML page per human review: what changed, why, risk, data flow.
  You save it where the PR can link it and set `VISUAL_EXPLAINER_OUTPUT_DIR` there.
  - ref: [visual-explainer](https://github.com/nicobailon/visual-explainer/blob/main/plugins/visual-explainer/SKILL.md) - point at the skill; install only with approval, DSOP step names its diff review command and the output directory

## Emit

When: a pushed change needs a person's sign-off

Compiled file: the shape in `references/dsop-template.md`, filled with sign-off policy, how to
ask, what to link.
