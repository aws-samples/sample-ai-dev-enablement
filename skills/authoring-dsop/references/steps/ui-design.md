# ui-design

This step designs a user-facing change before it is built. Without it every correction to UI-first code costs a rebuild, and new screens drift from existing ones. Skip it when the probe finds no UI surface. The rows differ in what the design starts from.

**Owns:** Designing a user-facing change from the repo's own components before building it. Only when the probe finds a UI surface.

**Ask the human:** How do you design a UI change before you build it?

**Look for:** a design system, component library, Storybook, design tokens, screenshots.

**Defaults:** trigger `agent`, eval `agent`.

## Good looks like

- Built from the repo's design system.
- Mockup options before code.
- Before/after screenshots.

## Implementations

Cheapest first. The repo's own practice comes before any of these. Links may be stale: check
them, and search for a newer one when a link fails.

- design-system-first (agent trigger, agent eval) - when: the repo has a component library or design tokens - You build only from the repo's components and tokens. The user picks from a mockup or screenshot of each option before code
- mockup-options-first (agent trigger, agent eval) - when: a UI surface exists - You show a few mockup options before code, then build the chosen one
  - ref: [anthropics `frontend-design`](https://github.com/anthropics/skills/tree/main/skills/frontend-design) - point at the skill; install only with approval
- screenshot-before-after (agent trigger, agent eval) - when: a UI surface exists - You attach before and after captures of the screen to the change
- accessible-ui-build (agent trigger, agent eval) - when: the change adds or changes UI - You build from the design system's components and check contrast, keyboard use and labels before review
  - ref: [addyosmani `frontend-ui-engineering`](https://github.com/addyosmani/agent-skills/blob/main/skills/frontend-ui-engineering/SKILL.md) - point at the skill; install only with approval with its accessibility checklist

## Notes

Runs between `acceptance` and `approve-plan`. Use only when the probe finds a UI surface.

## Emit

When: the change touches a UI

Compiled file: the shape in `references/dsop-template.md`, filled with design system, mockup step, before/after capture.
