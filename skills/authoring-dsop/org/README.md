# org/

Optional. Owned by the organisation's platform team, never by upstream.

`steps/<name>.md` files, same format as `../references/steps/`:

- a new name adds a step (place it with `workflow.md`);
- the same name replaces the shipped one (mark weakened lines `relaxed: <reason>`);
- a file holding only `removed: <reason>` drops it;
- `locked` on an option makes it policy.

`workflow.md` replaces the default table and triage rules.

Rules are in `../SKILL.md`, section "Organisation overlay".
