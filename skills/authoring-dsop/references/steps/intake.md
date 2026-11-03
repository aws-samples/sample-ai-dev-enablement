# intake

This step finds work and claims it, so two agents or people never pick the same ticket. Without
it the agent waits to be told what to do, or takes work someone else holds.

**Owns:** listing open work, filtering it, picking one item, claiming it with a lease, renewing
the lease, and releasing it.

**Ask the human:** Where does work come from, which tickets may the agent take, and how does it
claim one?

**Look for:** a ticket system with a CLI or MCP server, labels or states for agent work
("agent-ready", "needs review"), assignee rules, board columns, CODEOWNERS.

**Defaults:** trigger `agent`, eval `code`. Runs before `ready`.

## Good looks like

- List: open, unclaimed tickets, filtered by label, state, component or task type, for example
  only "needs review".
- Pick: by the repo's rule (priority, then age).
- Claim: assign the ticket and write the lease: claimed by `<agent>` until `<time>`. Default
  lease 2 hours; the repo may change it.
- Renew the lease before it ends while work goes on.
- Release on stop, block or timeout: unassign, so the ticket is free again.
- A claim whose lease ended counts as free to the next agent.
- Done when: one ticket is claimed with a lease, or the list was empty and that is reported.

## Implementations

Cheapest first. The repo's own practice comes before any of these.

- ticket-query-cli (agent trigger, code eval) - when: the ticket system has a CLI or MCP server
  - You list with a saved filter (`gh issue list --label agent-ready --assignee ""`, a JQL
  query, a saved tracker view) and claim with one command
- lease-comment (agent trigger, code eval) - when: the ticket system has no lease field - You
  write the lease as a comment line `claimed-by: <agent> until: <ISO time>` and read it back
  before claiming
- board-column (agent trigger, agent eval) - when: work moves on a board - You take the top card
  of the ready column and move it to in progress

Claim, renew and release skip the gate: they change no code and are easy to undo.

## Emit

When: starting work that comes from a ticket system

Compiled file: the shape in `references/dsop-template.md`, filled with the list command and
filter, the pick rule, the claim and release commands, and the lease length.
