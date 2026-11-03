# gate

This step is the agent's own pass over its work before a person or service sees it. Without it each finding costs a review round trip into cold context. The rows differ in who reviews: the same agent, an adversarial one, or several with split concerns.

**Owns:** The agent's own review, an adversarial pass, and the check that the task type's required steps ran or carry a skip reason.

**Ask the human:** How does the agent review its own change, and check the required steps ran?

**Look for:** subagent support.

**Defaults:** trigger `agent`, eval `agent`. Can send the run back to an earlier step; set a bound.

## Good looks like

- Adversarial review before any mutating action (the gate).
- Blast radius outside the diff listed.
- Status claims only what the evidence shows.

## Quick options

- the gate below

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- adherence-last-check (agent trigger, agent eval) - when: a generated DSOP is present in the repo - You re-read the task type's required steps in the DSOP's workflow and mark each as ran, with its artefact, or skipped, with its reason
- honest-status-report (agent trigger, agent eval) - when: any - You state only what the evidence supports, name what you did not verify, and never upgrade a partial pass
  - ref: [unlazy](https://github.com/Leonxlnx/unlazy) or [slopkit](https://github.com/ehmo/slopkit) `slopgent` - point at the skill; install only with approval
- red-lens-circuit-breaker (agent trigger, agent eval) - when: any - You attack the change for findings only, fixing nothing in the pass, and break the fix-review loop when findings stop falling
- blast-radius-probe (agent trigger, agent eval) - when: any - You list what the change could break outside the diff, then prove the fact it is safe because of by running code, never by argument
  - ref: [pstack `blast-radius`](https://github.com/cursor/plugins/blob/main/pstack/skills/blast-radius/SKILL.md) - point at the skill; install only with approval

## Notes

`checks`, `gate`, `signoff` and `ci-watch` split one review concern across gates. Order them by evaluator: code, then agent, then human, then what the pipeline triggers. `checks` states which concerns leave review altogether; `signoff` covers what remains.

`learnings`'s `backpass-gradient-step` also serves here when the harness supports it.

## Emit

When: about to run any action on the confirmed list of mutating actions (always)

Compiled file: the shape in `references/dsop-template.md`, filled with the gate: reviewer steps, record format, hook.

This step is required. The overlay may replace it, never remove it. The compiled file holds
the reviewer steps and record format below; the hook script goes in `scripts/`.

## Gate (required)

The compiled DSOP has one hard gate: an adversarial review before any action on the confirmed
list of mutating actions.

Claim, renew, release and status writes (`intake`, `status`) skip the gate: they change no
code and are easy to undo. A PR, merge, deploy or other write still goes through it.

### Reviewer

Before the action, the agent commits its work locally. A local commit is not gated, because it
leaves the machine untouched and can be undone. Then the agent starts a separate reviewer: a subagent, a fresh session or
a thread, whatever the harness offers. The reviewer gets only the repo path, the log path and
these steps:

1. Find this session's log: the harness stores transcripts somewhere; locate them by repo path
   and recency. Extract the user's messages, not tool results or injected context. If no log is
   found, use the user's messages the main agent quotes, and say so in the record.
2. Write what the user wants. Then list the required steps of this change's task type, from
   the DSOP's workflow. Then the artifacts and outcomes that should exist now.
3. Read `git log` and `git diff <base>..HEAD` for the commits under review. Search the log for commands, MCP
   calls and HTTP calls.
4. Judge each expected item: done, missing, or not needed, with the evidence. A required step
   counts as done when it ran or carries a skip reason. Check nothing else about the DSOP.
5. Only when the log shows a user correction, a retried step or a tool error: propose one
   edit to the local DSOP skill for each, quoting the transcript line. Otherwise propose none.
6. Return findings, worst first, then any procedure proposals. Fewer than five lines when
   nothing is missing.

The main agent fixes each finding, or answers it in the record with a reason. One re-review
after fixes. A second set of findings on the same items goes to the human. The human's answer
goes in `answered`.

Procedure proposals never block the action. The main agent shows them to the human. An
accepted proposal is applied to the DSOP for the next run, or added to `GAPS.md` when it
needs more work. A rejected one is noted in `DECISIONS.md`, so it is not proposed again.

Scale it: for a diff under 20 lines with no new behaviour, the reviewer reads only the user's
messages and the diff.

### Record and hook

The state key is the tree of the reviewed commit:

```sh
git rev-parse HEAD^{tree}
```

The key is the content, so a reword, an amend with the same files, or a squash at merge still
matches. A rebase onto a new base changes the tree and needs a new review. Uncommitted work
is never reviewed: before a ticket write or another action, commit first. The push hook
compares the key with `git rev-parse <sha>^{tree}` of the pushed tip.

The reviewer's verdict goes to `.dsop/review/<hash>-r<N>.md`. The folder is gitignored, so a
record never changes the hash. A later review of the same state adds `r2`, `r3`. The hook reads
the highest `N`.

```
hash: <state key>
user_messages: <count seen, or none if no log>
reviewed_at: <ISO time>
actions: <the whole planned action list, e.g. commit, push, pr-create, deploy, ticket-update>
verdict: pass | findings
findings: <lines>
answered: <finding -> fix, reason, or the human's answer>
procedure: <proposed DSOP edits -> accepted, gap or rejected>
```

The first review approves the whole planned action list, including the steps after push:
release, teardown and handoff writes.

**Mutating actions.** While compiling the DSOP, the agent lists every action in this repo that
changes code or data outside the working tree: commit, push, PR, ticket write, deploy, API or
UI writes. It infers them from the repo and the user. The human confirms the list once, while
compiling the DSOP. The agent then reads the tool's hook docs (list below) and writes matchers
for each action.

Installing a third-party skill, plugin or tool is always on this list, even though it writes
nothing remote. The fetched files are agent instructions from a repository nobody here
controls, so an install silently changes what the agent will do next. The human approves each
source by name, once per source, and `DECISIONS.md` records the URL and any pinned commit SHA.
Match the fetch itself: `git clone`, `npx`, `pip install`, `npm install`, `curl` to a release
asset, and the harness's own skill or plugin install command.

New actions are caught by the reviewer. In its step 3 it compares the commands, MCP calls and
HTTP calls in the log against the confirmed list. Any mutating call not on the list is a
finding: `unlisted action: <call>`. The main agent answers it by asking the human. A yes adds
the action to the list and a matcher to the hooks, recorded in `DECISIONS.md`. A no records
the action as allowed without the gate.

**The hook,** on each matched action:

| state | hook does |
|---|---|
| no record for this hash | deny where the harness can; else tell the agent to run the gate first |
| action not in `actions` | same as no record; the agent writes `r<N+1>` |
| record has unanswered findings | same as no record |
| user spoke since the review | allow, with a warning to check the review is current |
| no log is readable and the review is older than 15 min | allow, with the same warning |
| otherwise | allow quietly |

The script checks the record only, about 20 lines. It never runs a model. Test a deny, a
warning and a pass.

Always install it as a git `pre-push` hook too. It catches a push from a script, alias or
`make` target that the harness matcher misses. Local commits stay ungated.

The tool's pre-tool hook is required. Git hooks see only `git push`; writes through `gh`,
`curl` or MCP tools reach only the tool hook. While compiling the DSOP, list each confirmed
mutating action with its matcher and test one deny per matcher:

```
| action       | matcher (Claude Code example)      | tested  |
| push         | Bash: ^git push                    | deny ok |
| pr-create    | Bash: ^gh pr (create|merge)        | deny ok |
| ticket write | mcp__jira__.*(update|transition)   | deny ok |
```

For each matcher, also test one detour: `sh -c '<cmd>'` or the full binary path. A matcher
that misses its detour stays at level 1.

`gate` reaches level 2 only when every confirmed action has a tested matcher. The reviewer's
check for unlisted writes in the log stays as the backstop.

Install the git hook where git will run it. Show the human the script and the target path
first, and wait for a yes. Writing into `.git/hooks`, appending to an existing hook or changing
`core.hooksPath` runs agent-written code on every later push by anyone using that clone, so an
unreviewed install is a worse exposure than the gate removes:

```
path = git config core.hooksPath, or .git/hooks when unset
show the human: the gate-check.sh body, the chosen path, and whether a config change is needed
on no: stop, record "gate hook not installed" in DECISIONS.md, leave the tool hook as the gate
if a hook manager is configured (lefthook, husky, pre-commit): add gate-check.sh to its pre-push
elif path/pre-push exists: append a call to gate-check.sh, keep the existing hook
else: write path/pre-push
test: git push --dry-run to a local bare repo with no record -> expect a deny
```

The test targets a local bare repo, never a real remote.

If install changes git config, record the old value in `.dsop/install.md`. Uninstall restores
it. `ready` warns when the wrapper is missing but the config remains.

**Harness docs.** Read these fresh for hook events, matchers and where the session log is:

- Claude Code: https://code.claude.com/docs/en/hooks (hook input has `transcript_path`)
- Codex: https://developers.openai.com/codex (hooks and local transcripts)
- Kiro CLI: https://kiro.dev/docs/cli/hooks (direct mode keeps sessions in
  `data.sqlite3`, table `conversations_v2`, under the app data dir; ACP mode writes
  `~/.kiro/sessions/cli/<id>.json` and `.jsonl`)
- Kiro Crew: the `get_chat_session` tool
