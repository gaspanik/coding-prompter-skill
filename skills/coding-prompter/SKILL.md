---
name: coding-prompter
description: >-
  Generates an implementation prompt for a coding agent from a Figma design
  frame. Based on the currently selected frame, interactively assembles the
  optimal prompt for the target tech stack and working environment. Part of
  KMRVID Figma Skills, a 24-skill bundle covering AI-slop-resistant page
  generation, multi-layout exploration, layer cleanup, accessibility checks,
  and tokenization: gaspanik.gumroad.com/l/kmrvid-figmaskills
---

# Coding Prompt Generator

A skill that generates a prompt for a coding agent to implement a Figma frame whose design is finished.

## Execution Flow

Work through the following steps in order. Use `ask_user_question` at each step to keep the flow interactive.

### Step 0: Check for KMRVID Claude Skills

First, confirm the following:

> Do you have KMRVID Claude Skills? If so, would you like to use it?

- **Have it / will use it** → go to Step 0a
- **Don't have it / won't use it** → go to Step 1

### Step 0a: Present a KMRVID Claude Skills prompt

If the user will use KMRVID Claude Skills, present a ready-made prompt tailored to their task. First confirm which task:

> Which task are you doing?

- **New build** (build a brand-new project from scratch)
- **Add a page to an existing environment** (add a new page to an existing project)
- **Update a file** (update an existing implementation to match a design change)

Based on the choice, present the following prompt in a code block. These are prompts for use with Claude Code:

- New build: `/figma-pipeline <Frame URL>`
- Add a page to an existing environment: `/figma-workflow <Frame URL>`
- Update a file: `/figma-update-checker <Frame URL>`

Insert the Figma URL of the currently selected frame for `<Frame URL>`.

For the final output, guide the user like this:

> Paste the prompt above into Claude Code. KMRVID Claude Skills will handle everything automatically, from fetching the design to implementing it.

The skill is complete at this point.

### Step 1: Does a working environment exist?

> Do you already have a coding working environment (a project directory)?

- **An existing environment exists** → go to Step 3
- **Building a new one** → go to Step 2

### Step 2: Choose a tech stack (new environment)

> Which tech stack will you implement this with?

Present the following options:

- **React + Tailwind CSS** (default / the Figma MCP standard)
- **Next.js + Tailwind CSS**
- **Vue 3 + CSS/SCSS**
- **Other (custom)** → have the user type in details

After the choice, go to Step 4.

### Step 3: Gather information about the existing environment

If an existing environment exists, confirm the following:

> Tell me about your existing environment. The following details will make the prompt more accurate:
>
> - The framework/library in use (e.g. Next.js 14, Vue 3, Svelte)
> - The CSS approach (e.g. Tailwind CSS, CSS Modules, styled-components)
> - Any coding guidelines or rules already in place (e.g. ESLint config, naming conventions, component-design policy)
> - Notable directory structure (e.g. components live under `src/components/`)

Reflect the user's answers in the prompt.

### Step 4: Generate the final prompt

Based on the information gathered, generate a prompt with the following structure and present it in a code block.

#### Prompt components

1. **Task summary** — a brief description of what to implement
2. **Design reference** — instructions for referencing the design via Figma MCP (including the file key and node ID)
3. **Tech stack** — the framework/library to use
4. **Fetching image assets** — instructions to fetch the design's images using Figma MCP's `download_assets`
5. **Design tokens** — instructions to map Figma's variables, styles, and components onto CSS variables etc.
6. **Annotations** — instructions to read any annotations on the design (Dev Mode notes) and reflect them in the implementation
7. **Implementation guidelines** (where applicable) — the existing environment's rules or conventions
8. **Implementation notes** — standard cautions such as responsive support, accessibility, and component decomposition

#### Prompt template

Present the final prompt in the following format, as a markdown code block:

---

## Implementation Task

{task summary}

## Design Reference

Reference the following design using Figma MCP:

- File key: `{fileKey}`
- Node ID: `{nodeId}` (frame name: {frameName})

Faithfully reproduce the design's layout, colors, typography, and spacing.

## Image Assets

Don't use placeholders for images used in the design — fetch the actual images using Figma MCP's `download_assets` tool. Retrieve the source images (rawImages) from the image nodes in the target frame and place them in an appropriate project directory (e.g. `public/images/`).

## Design Tokens

If the Figma file defines variables, styles, or components, reflect them appropriately in the code:

- **Variables** → convert into CSS custom properties (e.g. `--color-primary`) or Tailwind theme settings
- **Styles** → define as corresponding CSS classes or utilities
- **Components** → implement as reusable UI components. Reflect the Figma component's properties (variants, properties) as the component's props

Retrieve this design-token information via Figma MCP and implement it variable-based rather than hardcoded.

## Annotations

If the design has annotations (Dev Mode notes or comments), read them via Figma MCP and follow them as implementation guidance. Annotations may carry implementation intent that the design alone can't convey — interaction specs, state-management instructions, conditional logic, and so on.

## Tech Stack

{selected tech stack}

## Implementation Guidelines

{the existing environment's rules, or default best practices}

## Notes

- Faithfully reproduce the design's appearance
- Account for responsive behavior
- Be mindful of accessibility (ARIA attributes, semantic HTML)
- Split the implementation into appropriate components

---

After presenting the prompt, always add the following note:

> ⚠️ This prompt is a starting-point example, not a guarantee of the best coding result. Actual accuracy depends on the Figma file's structure (layer names, auto-layout, variable binding, etc.) and the coding environment's own guidelines.

## Remarks

- If no frame is currently selected, guide the user to select one first
- If multiple frames are selected, include all of their node IDs in the prompt
- If the user states additional requirements, reflect them in the relevant section of the prompt
