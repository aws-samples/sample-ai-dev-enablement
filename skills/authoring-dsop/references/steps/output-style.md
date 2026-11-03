# output-style

This step fixes the shape of everything the run says afterwards: status lines, commit messages, completion reports. Without it each step invents a format, and finished steps read like narrated ones. Output style depends most on the model, so the step names the model it was tuned against.

**Owns:** The shape every later step writes in, and the model it was tuned for.

**Ask the human:** How do you want each step, commit and final report written, and for which model?

**Look for:** an output-style setting, a model pin.

**Defaults:** trigger `agent`, eval `agent`.

## Good looks like

- Reports state what changed and the proof, one line per step.
- Tuned for a named model.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- model-pinned-output-contract (agent trigger, agent eval) - when: any - You write the expected shape of status lines, commit messages and completion reports, naming the model it was tuned against. A model change reopens it
  - ref: [Opus 5 prompting guide](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5) - adapt; the contract states its verbosity and over-verification guidance
- harness-output-style (hook trigger, code eval) - when: the harness carries an output-style or persona setting - You commit the style as harness configuration, so it applies even when the agent forgets
  - ref: [Claude Code output styles](https://docs.claude.com/en/docs/claude-code/output-styles) - point at the tool; install only with approval, DSOP step names the style file only
- one-line-report-shape (agent trigger, agent eval) - when: any - You write one sentence per finished step: what changed and the proving artefact. No restated request, no summary of a summary

## Notes

`docs`'s `slop-free-prose` governs the prose this contract produces.

## Emit

When: writing a status line, commit message or final report

Compiled file: the shape in `references/dsop-template.md`, filled with the report shape and the model it was tuned for.
