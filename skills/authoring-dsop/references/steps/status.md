# status

This step keeps the outside world in sync with the work: the ticket, the PR and the team chat.
Without it a claimed ticket looks idle, a finished one looks open, and the next run picks up
done work.

**Owns:** ticket state changes, links to the branch and PR, comments when blocked, filing what
the run found and skipped, and chat posts the repo expects.

**Ask the human:** Which ticket states and chat posts should the agent update, and when?

**Look for:** a ticket system and its states (Jira, GitHub issues, Linear), a required id in
the commit, PR templates linking tickets, a team chat or webhook the repo posts to.

**Defaults:** trigger `agent`, eval `code`. Runs at set points: after claim, after push or PR,
when blocked, at handoff.

## Good looks like

- After claim: ticket "in progress", branch linked.
- After push or PR: PR linked, ticket "in review". Ticket id in the commit.
- When blocked: a comment with the reason, then `intake` releases the claim.
- At handoff: ticket "done" or "waiting", skipped discoveries filed as tickets, chat post if
  the repo does that.
- Done when: the ticket shows the current state and every link, with nothing left to update.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- trailer-gate (agent trigger, code eval) - when: a ticket id pattern exists - The step fails
  when the commit message carries no trailer matching the repo's ticket-id pattern
- ticket-state-cli (agent trigger, code eval) - when: the ticket system has a CLI or MCP server
  (`gh issue edit`, `jira issue move`, a tracker MCP server) - You move the ticket and add links with one
  command per point
- discoveries-to-tickets (agent trigger, agent eval) - when: a ticket system exists - At
  handoff you turn every skipped discovery into a ticket with enough context to start it cold
  - ref: [mattpocock `to-tickets`](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-tickets/SKILL.md) + [`triage`](https://github.com/mattpocock/skills/blob/main/skills/engineering/triage/SKILL.md) - point at the skill; install only with approval
- chat-post (agent trigger, agent eval) - when: the team chat has a webhook, CLI or MCP server -
  You post the outcome and links where the team expects them
- prose-status-write (agent trigger, agent eval) - when: any - You record, where the repo keeps
  them, what landed with its link, what you skipped and why, and open discoveries

Claim, release and status writes skip the gate: they change no code and are easy to undo.

## Emit

When: a ticket or chat must show the work's current state

Compiled file: the shape in `references/dsop-template.md`, filled with the ticket system, its
states, the command per point, and where chat posts go.
