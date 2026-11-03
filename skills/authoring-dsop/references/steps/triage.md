# triage

Rates the change before any work, so it gets the right task type in the workflow. Without it a
risky change takes the short path, or a one-line fix walks through research and plans.

**Owns:** the repo's task types (the columns of the workflow table), the criteria that sort a
change into one, and the type each change takes.

**Ask the human:** What kinds of work come into this repo, and how do you tell them apart?
The agent proposes a list from the probe; the human confirms, merges or renames.

**Look for:** ticket types, labels and templates; commit prefixes over the last few hundred
commits (conventional commits, `fix:`, `feat:`, `chore:`, `docs:`, `deps:`); PR templates;
branch names; release notes; risk lists, CODEOWNERS, module boundaries.

**Defaults:** trigger `agent`, eval `agent`. Runs right after `ready`, and again after
`research`.

## Probe

While compiling the DSOP:

1. Find the kinds of change this repo makes, and how often. Read recent history without
   merges, plus labels, templates and tickets. Use whatever tells kinds apart here: files
   touched, prefixes, labels or ticket types.
2. Merge rare types into a near one. Aim for a handful of types. Start from the defaults
   (Docs, Fix, Full) when the repo shows none.
3. For each type, write its criteria: the label, prefix, path or size that sorts a change into
   it, plus risk and complexity limits.
4. Propose each type as a column of the workflow table, with its `*`, `?` and `H` marks.
   Quote two real tickets or commits per type, with its count and share. Mark a type
   uncertain when its rule is a guess.
5. The human confirms the types, criteria and marks in step 4.

## Good looks like

- Complete output: one line with the task type, risk and complexity, and the rule that decided
  each. This line alone brings the step to level 2.

- Type, risk and complexity written in one line, with the rule that decided each.
- Each type has criteria a script or the agent can check, and real examples from the repo.
- The default types are Docs, Fix and Full; a repo may add, merge or rename them.
- Unsure means high.
- The highest risk or complexity wins. Docs with high risk takes the Full task type.
- The type is stated before the next step.
- After `research`, triage runs again and may move the change to a lighter task type.

## Implementations

- label-to-type map (agent trigger, code eval) - when: issues carry type or risk labels -
  You read the labels and pick the task type from a committed map.
- path risk rules (hook trigger, code eval) - when: risky paths are known - A script marks a
  change high risk when it touches a listed path.
- written rules (agent trigger, agent eval) - when: any - You apply the triage rules in
  `references/workflow.md` and state the one line.

## Emit

When: starting any change, right after ready, and again after research

Compiled file: the shape in `references/dsop-template.md`, filled with the repo's triage rules
and each type's column in the workflow table.
