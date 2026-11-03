# codify-tests

This step turns `validate`'s one-off run into a committed test, so the next change re-checks it without spending tokens. Without it a change passes on a screenshot and leaves no test.

**Owns:** Turning the validated run into a committed automated test, so the next change re-checks it for free.

**Ask the human:** How do you decide a behaviour deserves a committed test, and where it goes?

**Look for:** the suite that owns e2e tests, whether CI runs it.

**Defaults:** trigger `agent`, eval `code`.

## Good looks like

- The validated path lands as a test at the cheapest layer that can fail.
- Each new test protects a stated behaviour.
- Skipped and quarantined counts do not grow.

## Quick options

- snapshot or golden files for CLI output

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- codify-validated-run (agent trigger, code eval) - when: a test suite exists - You land the validated path as an automated test in the same change. A captured artefact alone keeps the change open
  - ref: [qa-skills `ai-test-generation`](https://github.com/petrkindlmann/qa-skills/blob/main/skills/ai-test-generation/SKILL.md) - point at the skill; install only with approval, to turn the diff into test code with a designed oracle; DSOP step names the suite the new test joins
- test-value-gate (agent trigger, agent eval) - when: a test suite exists - Before adding or changing a test, you state what behaviour or contract it protects, which credible regression fails it, and why existing proof misses it. Each contract has one owning test at its strongest boundary; you extend it or its table. You refuse tests that need a test-only seam, compare the subject with itself, or assert call shape
  - ref: [openclaw `test-audit`](https://github.com/openclaw/openclaw/blob/main/.agents/skills/test-audit/SKILL.md) - point at the skill; install only with approval (MIT), its authoring gate only
- explore-then-codify (agent trigger, agent eval) - when: the change has a driveable surface - You drive the surface as a user would, then turn the successful path into a repeatable test, paying for the manual pass once
  - ref: [qa-skills `exploratory-testing`](https://github.com/petrkindlmann/qa-skills/blob/main/skills/exploratory-testing/SKILL.md), or [anthropics `webapp-testing`](https://github.com/anthropics/skills/tree/main/skills/webapp-testing) for a web surface - point at the skill; install only with approval
- test-placement-by-layer (agent trigger, agent eval) - when: the suite is layered - You place the test at the cheapest layer that can fail for this reason, adding no end-to-end test to a suite that has one
  - ref: [qa-skills `test-strategy`](https://github.com/petrkindlmann/qa-skills/blob/main/skills/test-strategy/SKILL.md) - point at the skill; install only with approval, for its test-level and pyramid analysis only, scoped to this change, skipping its default multi-quarter document
- doubles-at-the-boundary (agent trigger, agent eval) - when: the path crosses an external dependency - You replace the dependency at the boundary, never inside the code under test, so the test is deterministic but not fictional
  - ref: [qa-skills `contract-testing`](https://github.com/petrkindlmann/qa-skills/blob/main/skills/contract-testing/SKILL.md) - point at the skill; install only with approval, when the boundary is a service the repo does not own
- suite-health-guard (agent trigger, code eval) - when: a suite exists and can report skips - You record the suite's skipped and quarantined count with the change, and refuse a new test that would join the skip list
  - ref: [qa-skills `test-suite-curation`](https://github.com/petrkindlmann/qa-skills/blob/main/skills/test-suite-curation/SKILL.md) - point at the skill; install only with approval, for its CI-history mining of never-failing and flaky tests; DSOP step names the skip-count condition
- recorded-manual-steps (agent trigger, agent eval) - when: any - When nothing can be automated, you commit numbered reproduction steps beside the change and name what blocks automation

## Notes

`codify-validated-run` decides which run becomes a test and `test-value-gate` whether a test earns its place; a repo takes both. A repo that keeps reaching `recorded-manual-steps` has nowhere to put the test, a finding for `status`.

## Emit

When: a validated path should stay checked

Compiled file: the shape in `references/dsop-template.md`, filled with suite and layer for the new test, skip rule.
