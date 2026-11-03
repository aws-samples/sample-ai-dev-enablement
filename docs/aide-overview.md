# AI-native Developer Enablement (AIDE)

AIDE is a collection of good practices emerging across the software industry. It
covers the human skills, the team mechanisms and the technical capabilities that an
organization can adopt to improve its software delivery outcomes with AI. Every aspect
comes with a question you can use to assess your own organization.

> This document is educational material. It describes practices and patterns, not a
> deployable solution. See [DISCLAIMER](../DISCLAIMER.md).

## Personal agents first: the approach that shapes your AI platform

The industry is moving through a "PC revolution" of AI agents. Previously, a team that
wanted users to interact with AI inside its product built a custom AI experience and
embedded it in a website or app. Today, each user has a personal agent: a command
center that strings knowledge and capabilities together in one place. Instead of a
sales agent in the sales app and a calendar agent in the calendar app, users ask their
personal agent to "create a sales briefing for my meeting at 10:00 AM". They expect the
data and the actions to be available in their agent rather than somewhere else. Where a
service genuinely offers a better experience or better context management through its
own agent, that agent is typically exposed as an MCP server or over the A2A protocol so
the personal agent can call it.

A personal agent approach does not mean a single agent surface. Give users a choice of
terminal, desktop and web agents, and let them pick by preferred UX. Every agent should
install the same capabilities from one central registry for MCP servers and for
[agent plugins](https://aws.amazon.com/blogs/opensource/aws-supports-agent-plugins-an-open-standard-for-portable-agent-extensions/),
the open standard for portable agent extensions.

Beyond personal agents sits the always-on team agent, proactive and active across the
whole software development lifecycle. It onboards like an employee and operates in the
flow of daily team tasks, communications and activities. AWS demonstrates this pattern
with [AI Teammate](https://www.youtube.com/watch?v=4n95e0OntPo).

**Question:** What is your strategy for providing powerful agentic surfaces to your
employees? Which surfaces do you provide per role?

## The baseline capabilities of an AI platform

To provide agents and make them useful, a common set of platform capabilities has
emerged. Deciding these early helps enablement activities, which can then demonstrate
the real tools and integrations available in the organization.

| Capability | Implementation |
|---|---|
| **Personal agents:** the primary surfaces for your users, varying by role | terminal, app or web based agents |
| **AI tool catalog:** which tools and services are available, and what data may be processed with them | wiki pages, central MCP server |
| **AI resource catalog:** search and install MCP servers, tools, agents, skills, or full [agent plugins](https://aws.amazon.com/blogs/opensource/aws-supports-agent-plugins-an-open-standard-for-portable-agent-extensions/) | [AWS Agent Registry](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/registry.html), wiki pages, git repos, central MCP server, CLI |
| **MCP gateway:** central governance of available MCP servers, authorization, observability | [AgentCore Gateway](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway.html), [AgentCore Identity](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/identity.html) |
| **MCP servers and CLI tools:** eyes and hands for agents to interact with your organization | remote servers managed by [AgentCore Gateway](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway.html) or hosted on [AgentCore Runtime](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agents-tools-runtime.html) |
| **LLM gateway:** centrally provide and observe inference | [AgentCore Gateway](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway.html), [Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html) |
| **Security and observability:** central guardrails and traceability for LLMs and MCP servers | [AgentCore Observability](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/observability.html), [Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html), [AgentCore Policy](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/policy.html) |
| **Background agent runtime:** decouple agentic work from local, limited compute, enabling long-running autonomous tasks | [AgentCore Runtime](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agents-tools-runtime.html), [Lambda MicroVMs](https://docs.aws.amazon.com/lambda/latest/dg/lambda-microvms-guide.html) |

**Question:** How do you provide baseline AI capabilities like AI resource catalogs, MCP
servers and security or observability in your organization today?

## Structured AI coding enablement as the flywheel

DX, the research company that co-invented DORA and SPACE,
[found](https://getdx.com/report/ai-assisted-engineering-q4-impact-report/) that just
dropping an AI tool on developers does not work. A structured enablement strategy is
what raises code maintainability, change confidence and speed.

A program that scales usually has three layers. Program leads recruit and train
champions inside each organization using shared workshop material. Champions run that
material and mob programming sessions with their own teams. Teams then build fluency
through repeated practice on real work.

After an initial workshop, developers should have internalized the following concepts:

- reflect on the lethal trifecta (access to private data, ability to communicate
  externally, exposure to untrusted content) to prevent security incidents
- treat the agent as a strong collaborator that needs your critical thinking to produce
  good results
- try different models and vary the amount of procedural guidance
- express intent and technical or business context
- the human orchestrates and verifies, the agent executes
- manage context windows with handoffs and subagents to keep the agent smart
- use multiple modalities to provide context (voice, documents, images)
- run extensive grilling sessions with the agent to build a common understanding
- build throwaway prototypes to verify assumptions early
- when to "vibe", when to create plans, and how big the plans need to be
- when to rely on the latent knowledge of the LLM, and when to demand rigorous
  grounding in data and documentation
- let the agent self-review, and run adversarial subagent reviewers to improve quality
- give the agent eyes and hands for everything so it can stay in the loop and verify on
  its own
- decide what to review (critical, complex code) and how (alone, with an agent, with
  visualizations such as call graphs)
- push agents to verify work end to end and generate verification artifacts (diagrams,
  screenshots, videos)
- prefer deterministic code and checks over non-deterministic LLMs
- turn development processes, mistakes and non-obvious information into reusable
  guidance through skills and steering files
- balance pre-generated context with ad-hoc context
- use spec-driven development frameworks to automate common steps

Building AI fluency is not a one-time exercise to check off.
[DX found](https://getdx.com/blog/the-ai-efficiency-plateau/) that gains are not
necessarily persistent: of the developers who reached peak time savings, 66.1% reported
lower time savings in the quarters that followed. Teams can get stuck at their level of
knowledge, or burn out on effective but energy-consuming practices. New tools, models
and techniques also emerge constantly. Teams therefore need continual activities to
learn what works and what does not. Two repeated exercises work well:

1. **Mob programming** is collaborative development where the team solves a real task
   together using AI tools. The value comes from organic micro-learnings that emerge
   during the session. Random rotation forces engagement, and commands, flags,
   techniques and tool features spread through observation and discussion.
2. **AI retrospectives** gather team feedback each sprint on AI tool usage, challenges
   and learnings. This complements mob programming by capturing individual experience.
   Record and transcribe the 30-minute session, then turn it into a learning report for
   the team.

**Questions:** How do you ensure teams receive structured enablement at scale and adopt
recurring activities to grow their AI fluency? How familiar are your employees with the
concepts listed above?

# An AI-native SDLC northstar

This section walks through the phases of a mature, AI-native SDLC that fully embraces
repeatable agent orchestration as a first step towards highly autonomous software
development.

## Ideate and discuss requirements

Agents become far more valuable during ideation and requirements engineering once the
systems around them expose APIs, and those APIs are exposed as MCP servers. Make the
wiki, cloud drive, chat, task management and collaboration tools readable and writable
for every personal agent. For highly interactive team sessions, also known as
[mob elaboration](https://builder.aws.com/content/3CAPsa6qmz73Y7zGLKg28gTaTfL/ai-dlc-transform-product-leadership-beyond-just-coding),
speech-to-text tooling can stream the live transcription to an agent that keeps the
document under discussion up to date. Many existing collaboration platforms were not
designed for agents, which is driving a new class of AI-native editors where humans and
agents co-create documents in real time.

**Question:** How are you using AI during team ideation?

## AI onboarding of a software project

Many common pitfalls disappear when a code repository is properly prepared for AI
agents. The main parts of that preparation are:

- storing the complete agent configuration (MCP servers, plugins, agents, skills,
  rules, hooks) in the code repository, for versioning and portability
- adding shared technical and business domain expert skills
- creating and maintaining a
  [lightweight AGENTS.md that uses progressive disclosure](https://www.aihero.dev/a-complete-guide-to-agents-md)
- giving the agent sufficient eyes and hands to interact with the code and the local dev
  environment (reading logs, controlling browsers and CLIs)
- defining the development SOP

**Questions:** How do you help developers make their code repositories AI-ready? Which
shared skills do you provide?

## Development SOP

A development SOP (DSOP) is a project-local "super-skill" that composes other skills as
[chains](https://claude.com/blog/building-verification-loops-in-claude-code-with-skills)
or steering files into the custom development workflow of a particular project. It lets
you combine a vended framework, such as Superpowers for general software development
methodology, with organization-specific guidelines and procedures. Depending on the
project there may be further SOP skills for other tasks such as bug triage or
operational review. A DSOP can live in AGENTS.md, but more commonly it is a skill the
developer activates explicitly at the start of a session, and again later when context
window rot has made the agent forget parts of it.

A typical DSOP contains these steps:

1. Output style. Depending on the frontier LLM involved, shaping the output style
   [has a comeback](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5)
2. Ensuring the repo is clean, up to date and ready for changes
3. Running a spec-driven development or planning framework of choice to edit the code,
   with additional constraints such as:
   1. enforced end-to-end validation
   2. a specific deploy and release strategy
   3. security and privacy restrictions
   4. calling external code review services and static checkers
4. Explicit teardown of local or cloud resources used during development
5. Documentation tasks (ADRs, manuals)
6. Explicit self-review or adversarial subagent review: did the agent follow the DSOP?
   Findings can loop back to earlier steps
7. Human sign-off gate
8. Redundancy checks: optimize the codebase once the feature is verified to work, which
   can loop back to earlier steps
9. Persisting session learnings and improving the DSOP
10. Pushing the code
11. Backlog update
12. Handoff to a new session for the next task
13. Watching the CI/CD pipeline for the change

A DSOP combined with a spec-driven development framework is a first step towards
autonomous software development, because it encodes the complete process from
requirement to code landing in the repository, or even in production. Techniques such
as loop or graph engineering add further formalization and reliability to the workflow,
while the logical steps stay the same.

**Question:** How do you enable developers to create a comprehensive DSOP that includes
your best practices and guardrails?

## Useful DSOP aspects

The following sections list aspects commonly found in DSOPs.

### Prototype, research and gene transfer

A notable omission in some spec-driven development frameworks, which hurts more given
how confident LLMs behave, is reflecting on what is _not_ known at the start of
development: what is feasible, which library features or cloud services could simplify
delivery, how things fit together, or what good looks like. Jumping straight to the
spec can cause extensive rework, especially where external services are involved, since
their behavior is almost never fully specified. A web research report, a learning test
or prototype, or a reference code sample are effective "gene transfers": the agent uses
them to inform both the spec and the implementation.

**Question:** Do your developers have sufficient access to ephemeral environments and
throwaway cloud accounts to build high-fidelity prototypes with their agents?

### Spec-driven development

Probably the least surprising entry in the list. A DSOP typically instructs the agent to
invoke a spec-driven development framework or skill such as
[Superpowers](https://github.com/obra/superpowers). Some frameworks support the inverse
composition: rather than the DSOP calling the framework, the framework has extension
mechanisms to include the custom aspects of the DSOP. That can work, but it typically
falls short when the first step sits outside the framework. Since the agent starts
cutting corners anyway as requirements pile up, keeping the DSOP as the top structure
and re-invoking it works better for eventually running all the steps.

**Question:** How are your developers performing spec-driven development today?

### Quality gates and self-review

Quality gates and self-review detect and address behavioral mistakes and missing or
incorrect output automatically.

Checks can be invoked in two ways:

1. **Agent-invoked gates inside the framework and the DSOP.** Methodologies such as
   Superpowers carry their own verification steps, so the check arrives with the
   workflow rather than being bolted on. Anthropic describe the general move of
   [turning manual verification steps into skills](https://claude.com/blog/building-verification-loops-in-claude-code-with-skills),
   which then form an iterative feedback loop.
2. **Deterministically-invoked gates as part of an orchestration framework or an agent
   hook configuration.** Loop and graph frameworks such as
   [autoloop](https://mikeyobrien.github.io/autoloop/) or dynamic workflows keep an
   agent running until a check passes rather than until it says it is done. Stop and
   tool-calling hooks are commonly used to
   [keep agents on track during long horizon work](https://github.com/anthropics/cwc-long-running-agents).
   The improvement here is that the agent harness triggers the checks through code, so
   the agent cannot forget to run them.

The nature of a gate can also be deterministic or agent-based:

1. Deterministic checks run code to check for the presence or content of files, or run
   tests, linters or other procedures that return an expected value.
2. Agent-based techniques, which can be combined:
   1. Self-reviews typically run in a subagent prompted to be an adversarial reviewer.
   2. [Backpressure](https://mikeyobrien.github.io/ralph-orchestrator/concepts/backpressure/),
      a term from loop engineering, is created by asking the agent to provide a set of
      evidence proving the work is complete. See also the next section.

Ideally every gate would have both a deterministic trigger and a deterministic
evaluation. The drawback is that reliable but "dumb" checks can become an obstacle to
completion: when the delivered changes are correct but incompatible with the
expectations of the checks, this leads to endless loops, or forces the agent to cheat.

**Question:** Which quality gates are your developers employing today as part of their
AI-assisted workflows?

### Enforce end-to-end verification with proofs

End-to-end verification gets its own entry because it is more heavyweight and shares
many traits with traditional end-to-end testing. Boris Cherny puts the quality gain at
[two to three times](https://x.com/bcherny/status/2044847858634064115).

A few elements make the approach especially effective:

1. **High-fidelity mocks** support component tests, so the agent can exercise a
   realistic system. This used to be too much effort; it is now possible because agents
   can write the mocks for you.
2. **Eyes and hands for the agent**, so verification happens during generation rather
   than after it. Instruct the agent to interact with the application like a real user,
   capture screenshots or videos after each change, and loop until the task is done.
   This works especially well when the agent can first run the verification manually and
   then turn it into a repeatable end-to-end test that does not require spending tokens.
3. **The proof as a required artifact.** A diagram, screenshot, short video or test run
   attached to the change helps a reviewer quickly grasp whether the agent worked
   towards the right goal. This is becoming a common requirement for open source
   projects, to quickly judge the intent of an AI-generated PR.

**Question:** What proof does a change carry today before a human looks at it?

### Deploy and release strategy

How you deploy and release is one of the biggest levers for creating room to learn and
for pushing delivery velocity. Teams usually need to raise the bar on their delivery
process first, and then give the agent the context to take it into account while
generating the spec.

More code means more incidents during deployment even when quality is stable, so you
need to be prepared. DORA has found a negative impact of AI on software delivery
stability for two consecutive years. Many mitigation techniques have been known for
decades and now become mandatory:

- Continuous delivery. Small releases reduce risk, and the disciplines are the same ones
  AI-assisted development already demands: move in small steps, follow a plan, keep a
  strong automated safety net.
- Progressive delivery. Automatic rollbacks, one-box deployments and canary releases
  contain a bad change rather than requiring someone to catch it. Decouple deployments
  from releases by gating new functionality behind feature flags, which can be enabled
  for small user groups and disabled if error metrics rise. The
  [Amazon Builders' Library](https://aws.amazon.com/builders-library/automating-safe-hands-off-deployments/)
  describes these patterns in detail.

**Question:** What percentage of your teams have truly adopted continuous and
progressive delivery?

## Code review

As AI pushes manual code review load towards unmanageable, teams discover that their
reviews historically carried a lot of concerns that should have been automated in the
first place:

- checking for required artifacts (end-to-end proofs, ADRs): rules, scripts
- syntax and style: linters and formatters
- code quality: static analysis
- security flaws: static analysis and dependency scanning
- logic bugs: automated tests, when written correctly during development
- performance regressions: automated performance tests
- breaking changes: automated contract tests

What a code review, pre-assessed and live-assisted by AI, should cover:

- presence and clarity of intent: why does this change exist, which decisions did it
  make, and why
- risk tiering: how much scrutiny the review needs, and where agents can summarize or
  visualize changes for easier human inspection
- meaningful naming
- domain boundary analysis
- coupling checks (SRP, DRY, circular dependencies)
- documentation drift
- AI anti-patterns (massive edits of tests, or recurring mistakes, persisted as
  learnings)

Test-time compute (spending more tokens and using frontier models) and context window
management (dividing the review aspects between multiple subagents) both increase the
quality of AI-assisted code reviews.

**Questions:** Have you successfully removed automatable concerns from your code
reviews? How are developers running AI-assisted code reviews?

## Persisting decisions and learnings

In complex projects you need to understand why the AI, and the developer, made certain
choices. That is where
[automated decision logs (ADLs)](https://addyosmani.com/blog/automated-decision-logs/)
and architecture decision records (ADRs) come in. They are a targeted, low-overhead
mechanism for capturing the reasoning behind significant AI-driven code modifications.
Think of it less as a comprehensive log and more as a structured set of notes to self,
generated automatically.

For learnings, when you catch an agent doing something undesirable, stop it immediately
and redirect it. Instruct agents to record corrections in AGENTS.md or a separate file
and continue with the updated guidance. In a new project, spending the first few coding
sessions closely watching the agent to catch a batch of corrections goes a long way
towards more autonomous sessions afterwards. A step often overlooked is pruning outdated
learnings. In the best case stale learnings waste tokens; in the worst case they
actively confuse the agent.

ADRs and learnings are easy to generate, but they compress what happened in a particular
agent session. Some teams experiment with tools such as [Entire](https://entire.io/)
that persist the full agent conversation as part of a code change. That allows ad-hoc
deep dives later to understand complex decisions and investigate subtle behavioral
issues of the agent. It also allows asynchronous "dreaming" processes that ingest all new
conversations on a schedule to identify recurring patterns.

**Question:** How do your developers store ADRs and learnings today?

## Working towards an uneventful CI/CD pipeline

Running an AI code reviewer on every code review is now common. When that reviewer runs
only after a change is submitted to the repository, as part of the CI/CD pipeline, it
creates an unnecessary review round-trip: the agent and the developer could have
addressed the feedback during development. Shifting left, by giving coding agents the
ability to invoke the remote code review against their local diff, raises the rate of
direct merges without additional iterations. The general insight is that enabling agents
to run most or all steps of the CI/CD pipeline during development removes slow feedback
loops that were designed for humans.

**Question:** How much can developers shift left their CI/CD pipeline, meaning which
steps can their agents run locally to incorporate the feedback immediately?

# Moving towards autonomous software development

High-autonomy AI techniques, sometimes called dark software factories, set an agent
running on a large task with minimal human oversight. They can work extremely well under
the right conditions: the behavior of the system must be well defined, acceptance
criteria must be clearly specified, and you need good backpressure such as regression
tests so the agent can verify its own work. Autonomous development works best when the
outcome is completely and deterministically verifiable. A prime example is a language
migration, where the new code's behavior can be extensively verified against the old
code, ideally with a copy of live production traffic. Drafting bug fixes can also work
well.

Where coding tasks meet those conditions, and the agentic orchestration (the harness) has
a high-quality DSOP and spends sufficient test-time compute, pull request acceptance
rates can reach 85% or higher, as in
[this customer case](https://www.deliveryhero.com/newsroom/delivery-hero-unveils-herogen-autonomous-ai-agent-unlocks-130-person-engineering-output/).

Contrast that with building a new product or a larger feature. When you develop
something that does not exist yet, you make product decisions constantly. What should
happen when the user does X? What is the right UX pattern for this edge case? Those are
judgment calls rather than implementation details. Hand them to a high-autonomy agent
without clear acceptance criteria and it will make decisions that satisfy the goal it was
given, while many of those decisions reflect poor product judgment you will not discover
until someone tries to use it. You also deprive yourself of exploring variants quickly
and reworking them in the moment, which is one of the larger benefits of using AI agents.

There is a middle ground that grows with your organization. In a cloud-based environment
for coding agents, loop harnesses such as
[autoloop](https://mikeyobrien.github.io/autoloop/) can run for hours, and those
long-running sessions persist independently of the user's computer. In contrast to other
software factory implementations, the user can return to the remote session at any time
and take back interactive control of the agent. That allows aiming for large batches of
autonomous work while correcting mistakes or wrong trajectories quickly through a short
human intervention. Those interventions then turn into persisted steering for the next
session, making the autonomous work more reliable.

Cloud-based agents with a dedicated sandbox and first-class human intervention mechanics
are the foundation for exercising this way of working, and for pushing the boundary of
what the agent can do autonomously.

**Question:** How are you enabling your developers to move seamlessly between letting
agents work autonomously and switching back to interactive collaboration?

# All questions at a glance

The questions below are the ones to work through with an organization. None of them have
a right answer. They are diagnostic, and the pattern of answers usually shows where the
constraint is.

**Coverage** is your own estimate of how widely the thing is spread across your teams:
0 none, 1 individuals (<10%), 2 pockets (10-30%), 3 supported but opt-in (30-50%),
4 default (50-80%), 5 universal (>80%). Note where you cannot estimate, because that
usually means nobody owns the capability.

| Section | Question | Answer | Coverage |
|---|---|---|---|
| Personal agents first | What is your strategy for providing powerful agentic surfaces to your employees? Which surfaces do you provide per role? | | |
| Baseline capabilities of an AI platform | How do you provide baseline AI capabilities like AI resource catalogs, MCP servers and security or observability in your organization today? | | |
| Structured AI coding enablement | How do you ensure teams receive structured enablement at scale and adopt recurring activities to grow their AI fluency? How familiar are your employees with the concepts listed? | | |
| Ideate and discuss requirements | How are you using AI during team ideation? | | |
| AI onboarding of a software project | How do you help developers make their code repositories AI-ready? Which shared skills do you provide? | | |
| Development SOP | How do you enable developers to create a comprehensive DSOP that includes your best practices and guardrails? | | |
| Prototype, research and gene transfer | Do your developers have sufficient access to ephemeral environments and throwaway cloud accounts to build high-fidelity prototypes with their agents? | | |
| Spec-driven development | How are your developers performing spec-driven development today? | | |
| Quality gates and self-review | Which quality gates are your developers employing today as part of their AI-assisted workflows? | | |
| Enforce end-to-end verification with proofs | What proof does a change carry today before a human looks at it? | | |
| Deploy and release strategy | What percentage of your teams have truly adopted continuous and progressive delivery? | | |
| Code review | Have you successfully removed automatable concerns from your code reviews? How are developers running AI-assisted code reviews? | | |
| Persisting decisions and learnings | How do your developers store ADRs and learnings today? | | |
| Working towards an uneventful CI/CD pipeline | How much can developers shift left their CI/CD pipeline, meaning which steps can their agents run locally to incorporate the feedback immediately? | | |
| Moving towards autonomous software development | How are you enabling your developers to move seamlessly between letting agents work autonomously and switching back to interactive collaboration? | | |
