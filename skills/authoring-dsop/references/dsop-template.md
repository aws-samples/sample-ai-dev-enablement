# DSOP template

The compiled `SKILL.md` holds the repo's workflow table: one row per step, with its file and a
mark per task type. Each step file says when it runs and how this repo does that kind of work. Drop rows whose step is `not used`.

## Compiled `SKILL.md`

````markdown
---
name: dsop
description: How changes are made in <repo>. Use for every change, fix, review or release in this repo.
---

# <repo> DSOP

Start at `ready`, then follow the workflow below. Skip a required step only with a one-line reason;
the gate checks it. Any mark means read the step's file before you act. The DSOP must cost less than the change: a one-line fix reads the required steps
`ready`, `triage`, `implement`, `validate`, `codify-tests`, `security`, `checks`, `cleanup`,
`gate` and `push`.

Commands: test `<cmd>`, lint `<cmd>`, build `<cmd>`, e2e `<cmd>`.

## Workflow

<triage table: type | when. No share column.>

| step | file | <task type> | <task type> | ... |
|---|---|---|---|---|
<one row per used step, in workflow order, marks `*`, `?`, `H`, `H?` or empty, from the table confirmed in step 4>

<the mark legend from references/workflow.md>

Always: before any action on the confirmed list of mutating actions (<confirmed list>), read
`steps/gate.md`. A hook enforces it.

Blocked: retry once. Then stop and state the tool, the call and the error. Do not route around it.

Open improvements: `GAPS.md`. Decisions: `DECISIONS.md`.
````

Levels, task type shares and the score go to `DECISIONS.md`, not into `SKILL.md`.

## Compiled step file

One per used step, in `steps/<name>.md`. Keep each under about 30 lines.

````markdown
# <step> in <repo>

When: <when the step runs, with any rule from the workflow table>

Do:
- <the repo's command or step, with the command>

Done when: <the complete output the step leaves, so nothing is missing>

Good looks like:
- <lines from the authoring step file, adjusted to this repo>

Watch for:
- <repo specifics, known traps>

Gap: <GAPS.md line, if any>
````
