# checks

Agent-generated volume makes unautomated concerns in review unaffordable. This step gives each concern below an owner outside review, and the compile checks the owner exists:

**Owns:** Static analysis, the review service, and the loop that clears their findings.

**Ask the human:** How do linters, analysers and review services check a change?

**Look for:** a review service, linter, analyser, type checker.

**Defaults:** trigger `agent`, eval `code`. Can send the run back to an earlier step; set a bound.

## Good looks like

- Linters, analysers and review service run locally on the diff before push.
- A reviewer gets a brief: what changed, why, risk, proof.

## Quick options

- run CI's local jobs

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- measured-external-reviewer (agent trigger, code eval) - when: a command-line tool can be installed - You run a reviewer whose file selection and bundling are code, so coverage holds on large changesets. Its precision is published on a public benchmark
  - ref: [open-code-review](https://github.com/alibaba/open-code-review) - point at the tool; install only with approval, DSOP step names the command and takes its findings as the verdict; CI runs the same review through its [action](https://github.com/alibaba/open-code-review/blob/main/action.yml)
- delegated-rule-review (agent trigger, agent eval) - when: a command-line tool can be installed, and the reviewer may not call its own model endpoint - You let code choose the files and the review rules, then review each file against its rules yourself. Findings name the rule they apply
  - ref: [open-code-review delegation mode](https://github.com/alibaba/open-code-review/blob/main/skills/open-code-review-delegate/SKILL.md) - point at the tool (`ocr`) and the skill; install only with approval; `ocr delegate preview` selects the files, `ocr delegate rule` resolves their rules
- converged-boolean (agent trigger, code eval) - when: the review host is queryable by CLI - You compute one boolean from three facts: the reviewed commit equals local head, no thread is unresolved, and the review decision is approved
- static-analysis-backoff (agent trigger, code eval) - when: an external review host exists - You run a complexity check, then an analyser fix loop keyed on stable finding ids. A breaker counts only newly bred findings
- review-request-and-response (agent trigger, agent eval) - when: any - You request review and respond to it as separate steps, with rules for what a request contains and when to argue with a suggestion
  - ref: [superpowers `requesting-code-review`](https://github.com/obra/superpowers/blob/main/skills/requesting-code-review/SKILL.md) + [`receiving-code-review`](https://github.com/obra/superpowers/blob/main/skills/receiving-code-review/SKILL.md) - point at the skill; install only with approval
- dual-reviewer-convergence (agent trigger, agent eval) - when: harness supports subagents - You run two independent reviewers on the same diff and ledger. Both must reach the stated threshold on quality, production-readiness and coverage
  - ref: [mattpocock `code-review`](https://github.com/mattpocock/skills/blob/main/skills/engineering/code-review/SKILL.md) - point at the skill; install only with approval, run twice under separate contexts. Alternative: [pstack `interrogate`](https://github.com/cursor/plugins/blob/main/pstack/skills/interrogate/SKILL.md) for reviewers on different models - point at the skill; install only with approval
- dimension-reviewers (agent trigger, agent eval) - when: harness with plugin support - You dispatch one reviewer per dimension at once: correctness, architecture, naming, performance, accessibility, docs
  - ref: [addyosmani `code-review-and-quality`](https://github.com/addyosmani/agent-skills/blob/main/skills/code-review-and-quality/SKILL.md) for the axes and approval standard, with [superpowers `dispatching-parallel-agents`](https://github.com/obra/superpowers/blob/main/skills/dispatching-parallel-agents/SKILL.md) to run one reviewer per axis - point at the skill; install only with approval, DSOP step names only the repo's axes
- review-time-compute-budget (agent trigger, agent eval) - when: the harness can select a model or effort level per step - You set a review budget above the implementation one: stronger model, higher effort, dimensions split across subagents. A miss here is paid in production
- review-brief (agent trigger, agent eval) - when: a person or a second agent reviews the change - In a fresh session you write the reviewer a brief: what changed and why, the risk, each concern `checks` left for review with its files first, and links to the proof. The reviewer decides; the brief never passes the change
- category-owner-floor (agent trigger, code eval) - when: any - You give every configured checker its own review category: linter, tests, analyser, type checker. The first non-zero exit is the verdict

## Notes

`checks`, `gate`, `signoff` and `ci-watch` split one review concern across gates. Order them by evaluator: code, then agent, then human, then what the pipeline triggers. `checks` states which concerns leave review altogether; `signoff` covers what remains.

An unowned concern stays in review, on record. The remainder is for `gate` and `signoff`.

Only `measured-external-reviewer` has its own measurement, so only it may block unmodified. `delegated-rule-review` runs the same file selection and rules, but the host agent reviews, so `measured-external-reviewer`'s benchmark does not apply to it and it stays advisory. Its recall is deliberately below a general-purpose agent's, so lower rows stay in the chain.

## Emit

When: about to push

Compiled file: the shape in `references/dsop-template.md`, filled with local CI jobs, linters, review service, review brief.
