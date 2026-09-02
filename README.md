# coding-prompter — Figma Design → Coding Agent Implementation Prompt

Register this skill in Figma's custom skill feature, select the finished FRAME(s) on the canvas, and run it from the chat. It interviews you about your target tech stack and working environment, then assembles a ready-to-paste implementation prompt for your coding agent of choice — Figma file key, node ID, image-asset fetching, design-token mapping, and Dev Mode annotations all included.

---

## What this is

A design being "done" in Figma and a coding agent being able to implement it well are two different things. The gap is usually the handoff prompt: does it point at the right node? Does it tell the agent to fetch real images instead of placeholders? Does it ask for variables/styles/components to be mapped to code-level tokens instead of hardcoded values? Most of the time this gets written from scratch, inconsistently, once per handoff.

This skill turns that handoff into a short interview. It asks what you're building with (or reads your existing project's conventions), then generates a complete, structured prompt — ready to hand to Claude Code, Cursor, or any other coding agent that has Figma MCP access.

```
Select finished FRAME(s) on canvas
        │
        ▼
Do you already have KMRVID Claude Skills?
        │
   ┌────┴────┐
  yes         no
   │           │
   ▼           ▼
Pick a ready-made   New environment or existing?
/figma-* command         │
                    ┌─────┴─────┐
                   new       existing
                    │           │
                    ▼           ▼
              pick tech stack   describe conventions
                    │           │
                    └─────┬─────┘
                          ▼
              Full implementation prompt
        (design ref / assets / tokens / annotations)
```

---

## Scope

- **Read-only against the Figma file.** This skill never edits the file — it only reads the current selection's file key, node ID, and frame name to build the prompt text.
- If the user already has **KMRVID Claude Skills**, it skips straight to handing over one of that bundle's ready-made slash commands instead of generating a custom prompt — no need to reinvent what that pipeline already automates.
- The generated prompt assumes the target coding agent has **Figma MCP** access (`download_assets`, variable/style/component reads, Dev Mode annotations). It does not call any coding-side tools itself.
- **Not a guarantee of implementation quality.** The generated prompt is a starting-point example. Actual coding accuracy depends on the Figma file's own structure (layer names, auto-layout, variable binding, etc.) and the target coding environment's own guidelines — this skill always appends that caveat alongside the generated prompt.

---

## Requirements

- **Figma (Design Agent / custom skill feature).** Register `SKILL.md` as a custom skill — no external MCP servers or API keys required on the Figma side.
- The coding agent that receives the generated prompt needs Figma MCP configured to actually act on it.

---

## Repo structure

```
skills/
  coding-prompter/
    SKILL.md          — skill definition to register in Figma's custom skill feature
    LICENSE
```

---

## Getting started

**Once published on Figma Community**, you'll be able to add this skill directly from the [AI Skills library](https://www.figma.com/community/ai-skills) — no download needed. Until then, register it from source:

**From source:** clone this repo and register the skill file yourself.

```bash
git clone https://github.com/gaspanik/coding-prompter-skill
```

**1. Register `skills/coding-prompter/SKILL.md`** in Figma's custom skill feature.

**2. Select the finished FRAME(s)** on the canvas.

**3. Run it from the chat**

```
/coding-prompter
```

```
Generate an implementation prompt for this frame
```

```
このフレームの実装プロンプトを作って
```

---

## Example output

```
## Implementation Task

Implement the pricing page hero section.

## Design Reference

Reference the following design using Figma MCP:

- File key: `AbC123XyZ`
- Node ID: `42:108` (frame name: Pricing / Hero)

Faithfully reproduce the design's layout, colors, typography, and spacing.

## Image Assets

Don't use placeholders for images used in the design — fetch the actual
images using Figma MCP's `download_assets` tool...

## Design Tokens

If the Figma file defines variables, styles, or components, reflect them
appropriately in the code...

## Tech Stack

Next.js + Tailwind CSS

## Notes

- Faithfully reproduce the design's appearance
- Account for responsive behavior
- Be mindful of accessibility (ARIA attributes, semantic HTML)
- Split the implementation into appropriate components
```

---

## Part of a larger set

This skill is one of 24 Figma-agent skills bundled in **KMRVID Figma Skills** — covering AI-slop-resistant page generation, multi-layout-pattern exploration, layer cleanup, contrast/accessibility checks, tokenization, component audits, ALT text suggestions, and more: [gaspanik.gumroad.com/l/kmrvid-figmaskills](https://gaspanik.gumroad.com/l/kmrvid-figmaskills)

---

Built by Masaaki Komori - [@cipher](https://x.com/cipher) · Skill for [Figma](https://www.figma.com/)
