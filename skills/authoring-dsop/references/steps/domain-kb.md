# domain-kb

This step keeps the facts about the business and its services that rarely change: domain words, processes and rules, and each service's outer behaviour. Without it every session re-derives the domain from code, and a KB that drifts or fills with code references misleads while looking authoritative. The rows differ in where the KB lives and what keeps it true.

**Owns:** The domain words, business processes and rules, and the outer facts of each service involved: domain, responsibilities, APIs, data models, main parts, features, use cases, and where its source of truth is. What this KB owns, and links to the KBs that own the rest.

**Ask the human:** Where are the domain words, business rules and the services that implement them written down?

**Look for:** a glossary, `docs/domain/`, a wiki or Confluence space, API or data catalogues, service READMEs.

**Defaults:** trigger `agent`, eval `human`.

## Good looks like

- Glossary and service facts in one place; outer behaviour changes get a KB edit the human approves.

## Quick options

- service READMEs

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- repo-kb (agent trigger, agent eval) - when: no team KB covers this domain - You keep `docs/domain/` with `COVERAGE.md`, `glossary.md`, `processes/` and `services/`. A service page states domain, responsibilities, APIs, data models, main parts, features, use cases and source of truth links
- coverage-links-only (agent trigger, agent eval) - when: a team KB, wiki or catalogue already covers the domain, and the agent can read it - You keep only `COVERAGE.md`: what this repo owns, and links to the KBs that own the rest. You copy no fact another KB owns
- kb-code-ref-check (agent trigger, code eval) - when: any KB in the repo - The step fails when a KB file holds a file and line, a symbol name from the repo, or a fenced code snippet. The repo's own grep or lint command is the check
- kb-release-draft (agent trigger, human eval) - when: changes merge to a main branch - After merge, you read the merged changes and draft KB edits only where outer behaviour changed: an API, a rule, a data field. The draft goes in a PR; the person greenlights it as `approve.md` describes
- llm-wiki (agent trigger, human eval) - when: the domain is large, or spread over many sources - You keep the KB as a wiki the agent writes and links from the sources it reads: one page per term, process and service, each linked from `COVERAGE.md`. Every page must pass `kb-code-ref-check`, and every edit follows the greenlight flow of `kb-release-draft`

## Notes

Distinct from `learnings`: the KB holds facts that rarely change and no code references (no file and line, function names or snippets). Runs after merge: the agent drafts KB edits where outer behaviour changed, and a person approves them. Plan and build read it.

A KB states what it covers in a coverage file, and links to every other KB for the rest. Allowed links: a repo, a folder, an API or data catalogue, a wiki page, another KB. Refused content: file and line, function or class names, code snippets.

`repo-kb` or `coverage-links-only` picks where the KB lives; the agent proposes from what assess found and the user picks. `kb-code-ref-check` and `kb-release-draft` layer over either one. `llm-wiki` is a form of `repo-kb` and needs `kb-code-ref-check`. A linked KB with no read path (CLI, API, MCP or browser) becomes a ticket via `status`.

## Emit

When: domain words or service behaviour change

Compiled file: the shape in `references/dsop-template.md`, filled with KB location, who approves edits.
