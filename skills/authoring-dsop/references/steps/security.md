# security

This step sets what the run may touch and what must never leave the repo. Skipped, it ends in a leaked secret or real customer data in a fixture, which no later gate takes back. The rows differ in scope: data used, commands issued, bytes committed.

**Owns:** What you may touch, which data you may use, and what must never be written out.

**Ask the human:** How do you keep secrets and real data out, and what may the agent touch?

**Look for:** secret scanning, a real-data policy.

**Defaults:** trigger `agent`, eval `code`.

## Good looks like

- No secrets or real data in the diff.
- Threat model for auth, data or public API changes.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- secret-and-data-floor (agent trigger, code eval) - when: version control - You sweep the staged diff for secret patterns, real-data fixture filenames and a staged env file. The exit code is the verdict
- security-assessment (agent trigger, agent eval) - when: a static analyser exists - You run the analyser and judge each finding in context. A separate pass filters false positives and names what it dropped
  - ref: [trailofbits `semgrep`](https://github.com/trailofbits/skills/blob/main/plugins/static-analysis/skills/semgrep/SKILL.md) then [`fp-check`](https://github.com/trailofbits/skills/blob/main/plugins/fp-check/skills/fp-check/SKILL.md) - point at the skill; install only with approval, one per pass as this row requires; fp-check gives a per-finding true or false positive verdict with evidence. Alternative: [addyosmani `security-and-hardening`](https://github.com/addyosmani/agent-skills/blob/main/skills/security-and-hardening/SKILL.md) - point at the skill; install only with approval
- destructive-command-guard (hook trigger, code eval) - when: harness with pre-tool hooks - You block destructive and history-rewriting commands in the harness, so the agent cannot run one at all
  - ref: [mattpocock `git-guardrails-claude-code`](https://github.com/mattpocock/skills/blob/main/skills/misc/git-guardrails-claude-code/SKILL.md) - point at the skill; install only with approval; it writes a pre-tool hook blocking the command. DSOP names the forbidden set; the shipped list covers git verbs only
- data-handling-rule (agent trigger, agent eval) - when: any - You follow three rules: no real secrets, synthetic or fixture data with recorded permission, and nothing emitted an outside reader should not see

## Emit

When: touching auth, data, secrets or a public API

Compiled file: the shape in `references/dsop-template.md`, filled with scan command, what must never be committed, threat model trigger.
