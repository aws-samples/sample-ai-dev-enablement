# ready

This step checks the run can start: a clean tree, a working toolchain, and every skill and tool a later step names reachable from the repo. Without it the run hits a missing command mid-change, or skips a step naming an absent skill and records it as followed. The rows differ in how much they check, and whether the harness or the agent checks.

**Owns:** The checks before work starts: the change is ready (ticket, scope, who accepts it), a clean tree, a built workspace, the tools later steps call, and the skills and MCP servers this DSOP names reachable from the repo.

**Ask the human:** How do you know a change, and the workspace, are ready to start?

**Look for:** version control, worktrees, the commands later steps call, a ticket system, the committed skill root and MCP config.

**Defaults:** trigger `agent`, eval `code`.

## Good looks like

- Definition of ready: ticket, done statement, scope, undo.
- Clean tree, tools present.
- Every skill and MCP server the DSOP names is committed or declared in the repo.
- The coding tool in use is the one the DSOP expects, and its hook config lists the gate
  matchers. Another tool, or missing matchers: stop and tell the human.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- clean-tree-check (agent trigger, code eval) - when: version control - You check the tree and sibling worktrees are clean, the branch is not behind upstream, and required commands exist. A non-zero exit stops the run
  - ref: [superpowers `using-git-worktrees`](https://github.com/obra/superpowers/blob/main/skills/using-git-worktrees/SKILL.md) - point at the skill; install only with approval, for the isolation half
- agent-deps-local (agent trigger, code eval) - when: the DSOP names a skill, a hook or an MCP tool - You check each named skill is committed under a repo skill root, and each called MCP server is in the repo's MCP config by env var name, never value. Harness-only installs list their install command. Anything missing fails the gate
- toolchain-setup-once (agent trigger, code eval) - when: a setup routine can be committed - You run toolchain setup once and record which installers the repo accepted or refused, so later runs skip the probe
  - ref: [trailofbits `devcontainer-setup`](https://github.com/trailofbits/skills/blob/main/plugins/devcontainer-setup/skills/devcontainer-setup/SKILL.md) - point at the skill; install only with approval; it commits the language toolchain as a container definition, and the DSOP step names the tooling it must leave
- precommit-floor (hook trigger, code eval) - when: a commit hook manager can be added - You install the hook set once. Format, lint and secret checks then run before every agent commit
  - ref: [mattpocock `setup-pre-commit`](https://github.com/mattpocock/skills/blob/main/skills/misc/setup-pre-commit/SKILL.md) - point at the skill; install only with approval, run once, then reference hook
- ticket-intake (agent trigger, agent eval) - when: a ticket system exists - Before the first edit, you return a ticket lacking an accepting owner, a done statement or a scope to the requester, and work waits
- definition-of-ready (agent trigger, agent eval) - when: any - Before the first edit you answer in writing: ticket id, checkable done statement, what is out of scope, how to undo it

## Notes

A repo normally takes `clean-tree-check` for the tree and commands and `agent-deps-local` for the DSOP's own dependencies. `agent-deps-local` has no published implementation: it reads paths and config against the install list step 6 collects, so it runs after the picks it verifies.

## Emit

When: starting a change

Compiled file: the shape in `references/dsop-template.md`, filled with definition of ready, clean-tree command, required tools and skills.
