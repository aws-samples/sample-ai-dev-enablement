# validate

This step drives the change once through a real entry point before review. Without it a change can pass the suite while broken where a user touches it. The rows differ in what counts as proof and whether a machine or a reader judges it.

**Owns:** Proving the change through a real entry point, and the artefact that attaches that proof to the change.

**Ask the human:** How do you prove a change through a real entry point?

**Look for:** an e2e command, a real entry point, screenshot or recording capture.

**Defaults:** trigger `agent`, eval `code`. Can send the run back to an earlier step; set a bound.

## Good looks like

- Proved through a real entry point, scaled to risk.
- A proof artifact is attached to the change: screenshot or video for UI, terminal recording or before/after output for CLI, request/response log for a service.
- No claim without command output.

## Quick options

- proof capture: Playwright screenshots and video, ffmpeg to GIF, asciinema or vhs terminal recordings, tui-test for TUIs, captured before/after command output or request logs

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- e2e-acceptance-gate (agent trigger, code eval) - when: version control, an e2e command exists - You seed a checkbox ledger from the plan's acceptance lines. The step refuses while any box is open or the e2e command fails
  - ref: [unlazy](https://github.com/Leonxlnx/unlazy) - adapt, copying its `GATES.md` ledger written before the work
- fail-closed-acceptance (agent trigger, code eval) - when: a reference artefact can be committed - You byte-match the emitted artefact against a committed reference and guard against generator drift. Absent proof fails the step
- high-fidelity-mocks (agent trigger, code eval) - when: the path crosses a component or service that cannot be run for real - You mock the surrounding system well enough to exercise the real path, where a stub would just agree with the code. Agent-written mocks keep the cost affordable
  - ref: [qa-skills `service-virtualization`](https://github.com/petrkindlmann/qa-skills/blob/main/skills/service-virtualization/SKILL.md) - point at the skill; install only with approval; it picks in-process mock, HTTP stub, record-replay, fault injection or ephemeral real service, and blocks real calls in CI
- proof-artefact-attached (agent trigger, agent eval) - when: the change alters observable behaviour - You attach the proof (screenshot, recording, diagram or captured test run) so a reviewer judges intent without re-running anything
  - ref: [addyosmani `browser-testing-with-devtools`](https://github.com/addyosmani/agent-skills/blob/main/skills/browser-testing-with-devtools/SKILL.md) - point at the skill; install only with approval, to capture console, network and visual output on a browser surface
- evidence-before-claim (agent trigger, agent eval) - when: any - You claim nothing complete without pasting or pointing at supporting command output. An unverified claim is itself a defect
  - ref: [superpowers `verification-before-completion`](https://github.com/obra/superpowers/blob/main/skills/verification-before-completion/SKILL.md) - point at the skill; install only with approval
- single-ci-command (agent trigger, code eval) - when: a containerised or scripted CI entry point exists - You run the whole gate from one command, locally and in CI, with a stated coverage floor, so local and remote green match
  - ref: [addyosmani `ci-cd-and-automation`](https://github.com/addyosmani/agent-skills/blob/main/skills/ci-cd-and-automation/SKILL.md) - point at the skill; install only with approval, DSOP step names the one command and the coverage floor
- production-entry-point (agent trigger, agent eval) - when: any - You send a request through the real artefact's production entry point and record the response, scaled to triage risk
- generated-verification-skill (agent trigger, agent eval) - when: the app has a UI, CLI or service a user drives - You generate a repo-local skill that starts the app, drives each user-facing feature as a user does, and names the end state that proves it works. The e2e check runs that skill
  - ref: [pstack `create-verification-skill`](https://github.com/cursor/plugins/blob/main/pstack/skills/create-verification-skill/SKILL.md) - point at the skill; install only with approval; it writes under `.cursor/skills/`, so fork the target path for another harness

## Notes

`high-fidelity-mocks`'s mocks let the validation run happen; `codify-tests`'s `doubles-at-the-boundary` makes the codified test deterministic. A repo picking both installs the skill once.

## Emit

When: about to claim it works

Compiled file: the shape in `references/dsop-template.md`, filled with entry point, proof capture tool, where the proof is attached.
