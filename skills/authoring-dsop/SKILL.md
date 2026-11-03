---
name: authoring-dsop
description: Write a repository's development SOP as a skill in that repository. Use when a repo needs one, when its DSOP is stale, or when the user asks for the next DSOP improvement.
---

# Authoring a DSOP

A DSOP is one skill in the repo. An agent loads it for every change. It holds a default
workflow table and one file per step, saying how this repo does that kind of work. Triage picks
a task type, and each task type lists its required steps. A change of task type Fix reads its required
steps. A new feature reads most of them.

The only hard gate is an adversarial review before any action on the confirmed list of
mutating actions. It is step `gate`.

## Principles

- **A workflow table, read on demand.** The compiled `SKILL.md` holds a workflow table: triage picks a task type, and
  each task type lists its required steps. The agent reads a step file only when it reaches that
  step. It may skip or reorder steps, with a one-line reason for a required one. There are no
  state files, and the gate is the only check.
- **Name the repo's command.** Every section ends in a command the repo has, or in the artifact
  it must leave. Prose about diligence does nothing.
- **Use known words.** Write "TDD red-green", "conventional commits", "ADR", "threat model". The
  model knows them. Spell out only what this repo does differently.
- **Prefer what the repo already does.** A practice the repo runs beats a new tool. Never adopt
  a found practice silently: the file may be dead.
- **Each line once.** If removing a line would not change what an agent does, remove it.
- **Proportion.** The DSOP must never cost more than the change. Say so in the DSOP itself.

## Steps

### 1. List the steps

The default workflow is `references/workflow.md`: a table of steps per task type, with rules, checks and
human points. Each step is one file in `references/steps/<name>.md`. List the shipped files and
any `org/steps/` files, merged by name; on the same name the org file wins. An
`org/workflow.md` replaces the default table and the triage rules.

```sh
ls references/steps org/steps 2>/dev/null
```

There is no general probe. Each step is probed on its own, in step 2.

### 2. Probe and propose by step

For each step, read its file, then search the repo using its **Look for** list. Cite the file
for every finding. Do not list the tree.

Probe in this order:

1. `ready` first. It finds the tool, its hook support, and the commands later steps call.
2. Every other step, in parallel if you can.
3. `triage`, using their results. It proposes the repo's task types, one column of the
   workflow table each, with the `*`, `?` and `H` marks.
4. `gate` last. Its list of mutating actions comes from every other probe (push, release,
   status, teardown, MCP and API writes), and its hooks use the hook support from `ready`.

Split the work across subagents when the harness offers them. Give each subagent one or more
step files and the repo path. Group steps that read the same files, for example `validate` and
`codify-tests`, or `checks` and `push`. Each subagent returns its rows and the commands it
found. Merge the rows and flag any that conflict for step 3. For a small repo, do it yourself,
in the same order.

For each step, return:

- **level** 1 or 2, or 0 when missing (see Levels and score);
- **evidence**: the files that show it, every one, not just the deciding one;
- **fill-in**: the DSOP lines you propose, with the command;
- **good looks like**: from the step file, adjusted to this repo;
- **options**: what the repo does today first, then the implementations from the step file
  that fit, cheapest first;
- **question**: the step's **Ask the human** question, with your proposed answer.

Mark an uncertain level as uncertain, with the evidence quoted. Do not guess.

A step that does not apply is written `not used: <reason>`. None is skipped silently.

### 3. Align

Give a fresh reviewer every row and the draft workflow table. It checks that commands agree,
marks match findings, the mutating list covers every write the probes found, every level has
evidence, and no practice is proposed twice. It returns row edits and open conflicts. Apply the
edits; mark open conflicts uncertain for the review. Run it once.

### 4. Review with the human

Show the fill-ins as one compact table: step, level, proposed line, uncertain yes or no.
Then ask about each uncertain or policy row, one per turn. Policy rows always need the human:
human sign-off (required, optional, none) and who may push.

The human's answer replaces your fill-in. Do not proceed on a row the human has not seen.

### 5. Write the gap list

Every step below level 2, every task type below level 3, and every option the repo has not
adopted becomes one line in
`<skill-root>/dsop/GAPS.md`:

```
- [ ] <step>: <what is missing>. Done when: <observable check>. Option: <implementation name>.
```

The human works this list later. A re-run ticks lines whose check now holds.

### 6. Emit

Follow `references/dsop-template.md`. Write `<skill-root>/dsop/SKILL.md` with the repo's workflow
table: one row per used step, its file, and a mark per task type. Write one `steps/<name>.md` per
used step, with the step's rule in its `When:` line. Levels and shares go to `DECISIONS.md`. Write the gate
from step `gate`: its compiled file, the hook script in `scripts/`, and the hook config for the
tool in use. Add `.dsop/review/` to the repo's `.gitignore`. Ask before adding a line to
`AGENTS.md`; add only one sentence that points at the DSOP, and only where the tool does not
find skills itself.

Write `<skill-root>/dsop/DECISIONS.md`: per step, the level, the pick, its origin (repo,
implementation list, authored) and the human's answer.

### 7. Prove it

Replay one task the repo already solved. Run it with and without the DSOP, withhold the tests
that shipped with it, let them decide. Report pass or fail per arm, tool calls per arm, and the
share of arm B's calls spent on DSOP files.

If the repo has no solved task with tests, run one small new task with and without the DSOP,
and record that it was a new task.

Both arms run locally and write nothing outside the working tree: no push, no PR, no ticket
write, no deploy. The replay happens before the human has watched the gate refuse anything, so
the gate is not yet trusted to hold a remote write back. Use a local bare repo as the remote
when an arm needs one.

Above 30% on a small fix is a defect in the DSOP. Cut the step files the run read most, then
re-run. Record the result in `DECISIONS.md`, or record that it was not run.

## Step files

`references/steps/<name>.md` holds one step: what it owns, the question to ask the human, what
to look for in the repo, its default trigger and eval, what good looks like, quick options,
implementations with links to public skills and tools, and an **Emit** section with its `When:` line
line and what its compiled file holds. Names match the nodes in `references/workflow.md`;
`approve-intent` and `approve-plan` both read `approve.md`.

## Levels and score

| level | of | means |
|---|---|---|
| 0 | step | missing, with no reason |
| 1 | step | written down and fully specified; a step marked `not used: <reason>` also lists here |
| 2 | step | the agent can reach every system the step needs (CLI, MCP, API, dashboard), and the step says what complete output it leaves, so the agent delivers the whole outcome; a later review or sign-off is fine |
| 3 | task type | the task type runs from `ready` to `handoff` with no human point |

A task type's level is the lowest level of its required steps. It is 3 only when every required
step is at level 2 and the task type has no human point.

The repo score starts at 100 and loses points from two fixed pools, so the number of steps or
task types does not change the range:

```
step deduction      = 60 x (sum of step gaps) / (used steps)
                      gap: level 0 = 1, level 1 = 0.5, level 2 = 0
task type deduction = 40 x (share of changes whose task type is below level 3)
score               = 100 - step deduction - task type deduction, rounded
```

- Level 2 is probed with the access of the developer the DSOP is for, used through the agent.
- Level 3 is probed with the identity the agent runs as when no human is present. On a local
  or personal environment that may be the developer's identity. A separate agent identity is
  recommended but not required.
- The score line states the identities probed, for example `probed as: <developer>, <agent>`.
- A rule the agent proposed counts as level 1 only after the human confirms it in step 4.
- Compute the score after step 4, from the levels the human confirmed. A row still uncertain
  counts at its lower level and is listed under the score as uncertain.
- A step marked `not used: <reason>` is left out of the count.
- Shares come from the `triage` probe's counts of recent tickets or commits per task type.
  Without history, each task type gets an equal share; `DECISIONS.md` records that.
- A task type with any human point is below level 3. 100 means every step reaches level 2 and
  the agent runs every task type alone. No repo may reach it today; that is expected.
- The score report lists the fixes that raise the score most, with the points each would earn,
  and they go into `GAPS.md`.

Some concerns are not steps, because they apply to the repo or the run, not to each change:
harness setup (skill vendoring, sandboxing, worktree isolation, which tool drives each surface),
run governance (cost and model budget, stopping rule, the blocked protocol), and portfolio work
(roadmap, background execution). Record them once in the DSOP's own sections, not per step.

## Organisation overlay

A platform team may add `org/steps/` beside this skill, with files in the same format.

- A new name adds a step, for example `threat-model.md`. Place it with an `org/workflow.md`.
- The same name replaces the shipped file. It keeps the shipped "good looks like" lines
  unless it marks one `relaxed: <reason>`.
- A file holding only `removed: <reason>` drops that step. Tell the human it was removed and
  why. Step `gate` is required and cannot be removed.
- An option marked `locked` is policy. Tell the human it is locked; offer no alternative.
- Org options are offered before shipped ones.
- Every default rule, the triage rules and the default workflow can be changed this way.
- `DECISIONS.md` records each org file and its version that shaped a pick.

To upgrade, replace everything except `org/`. A team vendors the skill by shipping it with
`org/` filled in, or by dropping `org/` beside an upstream install.

## Rules that still bind

- **No bare prohibition.** Every MUST NOT says what breaks if ignored.
- **No enumerated self-verification** in agent steps. Name one command and its pass condition.
- **An unmeasured model judge blocks only its own run.** The gate (step `gate`) may refuse a
  mutating action, because its findings go to the same agent, not to a person as a green.
- **No silent skip.** `not used` carries a reason.
- **A found practice is offered, not taken.**
- **Point at a skill, do not paraphrase it.** Name the skill or tool and its canonical URL
  instead of restating what it does. Installing one is a mutating action: propose it, and let
  the human approve that source by name before anything is fetched. A third-party repository is
  agent instructions you do not control, so an unapproved install hands your agent to whoever
  owns that repository. Where the human wants a source pinned, record the commit SHA in
  `DECISIONS.md`.
- **A failing check gets a bounded retry and an escape that records.** Never edit the check to
  pass.
- **Blocked means stop and say so.** Name the tool, the call and the error. Do not narrow scope
  silently.

## Glossary

- **skill root:** the folder where the tool in use finds skills in this repo, for example
  `.claude/skills/` or `.kiro/skills/`.
- **trigger:** what starts a step: `agent`, `hook`, `ci` or `human`.
- **eval:** what judges a step's result: `code`, `agent` or `human`.
- **mutating action:** an action that changes code or data outside the local repo, such as a
  push, PR, ticket write or deploy. A local commit is not one. The human confirms the list in step 4.
- **task type:** one column of the workflow table. Triage picks it.
- **human point:** an `H` or `H?` mark in the workflow table, where a person decides.
- **not used:** a step that does not apply to this repo, written `not used: <reason>`.
