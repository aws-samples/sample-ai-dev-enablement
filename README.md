# Sample code for AI-native Developer Enablement (AIDE)

Companion material for the talk *"Making AI coding work at scale: a framework to improve
your SDLC with AI"*.

AI-native Developer Enablement (AIDE) is a collection of good practices emerging across
the software industry. It covers the human skills, the team mechanisms and the technical
capabilities that an organization can adopt to improve its software delivery outcomes
with AI. Every aspect comes with a question you can use to assess your own team or
organization.

This sample demonstrates those practices as agent skills you can read, adapt and run in
your own repositories.

> This repository is educational sample content. It is not production-ready software.
> Read [DISCLAIMER.md](DISCLAIMER.md) before you use anything here.

## Talk outline

1. **Personal agents first.** Why the agent surface, rather than an AI feature inside
   each app, is the unit of enablement. What that implies for your platform.
2. **The baseline capabilities of an AI platform.** The capability set that has emerged:
   agent surfaces, an AI tool catalog, an AI resource catalog, an MCP gateway, MCP
   servers and CLI tools, an LLM gateway, security and observability, and a background
   agent runtime.
3. **Structured enablement as the flywheel.** Why dropping a tool on developers does not
   work. Champion-led enablement, mob programming and AI retrospectives as recurring
   rituals. The concepts a developer should hold after the first workshop.
4. **An AI-native SDLC northstar.** Ideation, AI onboarding of a repository, the
   development SOP, code review, persisting decisions and learnings, and shifting the
   CI/CD pipeline left.
5. **The development SOP (DSOP).** A project-local super-skill that composes other
   skills into the workflow of one repository, and encodes the path from requirement to
   merged code.
6. **Moving towards autonomous software development.** Where high autonomy works, where
   it fails, and the middle ground of long-running cloud sessions with human
   intervention.
7. **Assess yourself.** Every section carries a diagnostic question. The pattern of
   answers shows where your constraint is.

## What is in this repository

| Path | Contents |
|---|---|
| [`docs/aide-overview.md`](docs/aide-overview.md) | The AIDE overview: every practice, with its assessment question, and the full question table |
| [`skills/authoring-dsop/`](skills/authoring-dsop/) | An agent skill that writes a repository's development SOP as a skill in that repository |

## Using the skills

Each folder under `skills/` is a self-contained agent skill: a `SKILL.md` with its
supporting files. Copy the folder into the place your agent reads skills from, for
example `.claude/skills/` or `.kiro/skills/`, then ask your agent to use it by name.

```sh
cp -r skills/authoring-dsop ~/your-repo/.claude/skills/
```

### `authoring-dsop`

Writes a repository's development SOP (DSOP) as a skill inside that repository. It
probes the repo per workflow step, proposes what the repo should do for each step based
on what the repo already does, reviews the proposals with you, scores the result and
emits the compiled DSOP plus a gap list.

A platform team can override any shipped step through the `org/` overlay, which lets an
organization ship its own policy without forking the skill. See
[`skills/authoring-dsop/org/README.md`](skills/authoring-dsop/org/README.md).

## Assessment questions

The question table at the end of [`docs/aide-overview.md`](docs/aide-overview.md) lists
every diagnostic question with an answer and a coverage column. Copy it, fill it in with
your team, and look at the pattern rather than the individual scores.

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for how to report a
security issue. Do not open a public GitHub issue for a security report.

## License

This library is licensed under the MIT-0 License. See [LICENSE](LICENSE).
