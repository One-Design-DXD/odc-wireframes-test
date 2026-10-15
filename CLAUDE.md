# CLAUDE.md — [Client Name]

This file is read automatically by Claude at the start of every session in this repo. It gives Claude the project-specific context it needs so you don't have to re-explain the client, the design system, or the conventions every time.

**For humans reading this:** keep this file tight. It's easy to let a CLAUDE.md bloat into a thousand-line brain dump, which eats tokens and makes Claude's instructions less reliable. Prune aggressively. If something hasn't been relevant in a month, delete it.

**For Claude:** treat everything in this file as authoritative for this project. If anything here conflicts with your general instructions, this file wins. If something here conflicts with a global skill, follow this file and note the conflict to the user.

---

## Project overview

**Client:** [Client name]
**Project:** [Brief description — "marketing site redesign," "internal dashboard," "design system rebuild"]
**Phase:** [Discovery / Strategy / Design / Build / Maintenance]
**Timeline:** [Key dates or milestones]
**Primary contact at client:** [Name, role]
**Project lead:** [Name]

**What we're delivering:** [One or two sentences on the actual output — a live site, a prototype, a design system, a research report, etc.]

---

## How this repo is organized

This project follows the standard client template. The top-level folders and their purpose:

- `research/` — synthesized research outputs. Reference for strategy and design decisions.
- `strategy/` — recommendations, roadmaps, content strategy.
- `design/` — design system notes and Figma references (`design/figma-references.md` lists all Figma URLs).
- `src/` — component code and Code Connect mappings (`*.figma.tsx` files).
- `prototype/` — generated prototype code.
- `.agent/skills/` — project-specific skills layered on top of global skills.

When you're asked to work on something, check the relevant folder first. If you need research context for a design decision, read from `research/` rather than asking the user to paste it in.

---

## Two-layer skills

This project uses two-layer skill architecture:

1. **Global skills** apply across all projects — general process logic and capabilities.
2. **Project-level skills** live in `.agent/skills/` in this repo and specialize outputs for this client.

When both apply, project-level skills take precedence. If a project skill doesn't exist for the task at hand, fall back to the global one.

If you notice a pattern worth codifying as a skill for this project, suggest it to the user rather than silently creating one.

---

## Voice and tone

[This is the section that probably matters most for any content Claude produces. Fill it in with real references, not abstractions.]

**How the client sounds:**
- [Adjective], [adjective], [adjective] — e.g., "warm but authoritative, never clinical"
- [Example of a sentence in-voice]
- [Example of a sentence that is NOT in-voice, and why]

**Reading level:** [e.g., "8th grade — their audience is time-pressed, not undereducated"]

**Words to use:** [client-preferred terminology]

**Words to avoid:** [words the client has explicitly nixed, or that don't fit the brand]

**Reference docs:**
- Full voice and tone guide: [link or path, e.g., `strategy/voice-and-tone.md`]
- Example copy we've approved: [link or path]

---

## Design system

The design system is **ODC Design System 3.0** in Figma (see `design/figma-references.md`). Its tokens are translated in `src/styles/tokens.css`, and `design/design-system.md` covers the naming, rationale and open questions. Use these tokens. Don't hardcode values.

**Key principles for this design system:**
- Everything is fluid (Utopia `clamp()`, 320px → 2560px). Type and space scale with the viewport, so don't add per-breakpoint overrides for them.
- Use semantic color tokens (`--fg-*`, `--bg-*`) in components, not primitives (`--color-*`, `--black48`). Hierarchy maps to APCA contrast levels.
- One family: Geist Sans (Geist Mono for code). Use the default font features in `--font-features`.

**Tokens and naming:** (Figma `/` → CSS `-`; camelCase is kept)
- Typography: `--text-{xs|sm|base|lg|xl|2xl…9xl}`. Text-style classes are `.{heading|paragraph|ui}-{step}-{weight}`, e.g. `.ui-base-400`.
- Spacing: `--space-{4xs…5xl}`, plus ranges like `--space-sm-5xl`.
- Color: primitives `--color-{hue}-{50…950}`, `--black{NN}`, `--white{NN}`. Tokens `--fg-{primary|invWeak|valid…}`, `--bg-{…}`.

**Components mapped via Code Connect:**
[List or reference — Claude should check `src/components/` for `*.figma.tsx` files to see the current state. If you add new component mappings, update this list.]

**Components NOT to use:**
- [e.g., "Old card variant in the archive page — deprecated, don't reference it."]

---

## Technical conventions

**Framework:** [e.g., React + TypeScript + Tailwind]
**Breakpoints:** min 320 · sm 704 (44rem) · md 1024 · lg 1440 · xl 1920 · max 2560. See `design/design-system.md`.
**Accessibility target:** WCAG 2.2 AA (non-negotiable across all projects)
**Browser support:** [e.g., "Last 2 versions of evergreen browsers; no IE"]

**File naming:**
- Components: `PascalCase.tsx`
- Code Connect mappings: `PascalCase.figma.tsx` (matches the component)
- Utility functions: `camelCase.ts`
- Markdown docs: `kebab-case.md`

**Import aliases:** [e.g., "Use `@/components/...` not relative paths"]

---

## Working with Figma on this project

See `docs/figma-workflow.md` for the full workflow. Project-specific notes:

- When pulling design context from Figma, always check Code Connect mappings before generating new component code.
- If you find a Figma frame using a detached component instance rather than the proper library component, flag it to the user — don't just build around it.
- When writing back to Figma, never write to the design system library file directly. Duplicate or work in a branch file. The library file is changed deliberately by designers.

---

## Working with research on this project

The research synthesis in `research/` is the source of truth for user insights, stakeholder perspectives, and client context. When you're asked a question that touches on user needs, what the client cares about, or what stakeholders said, check `research/` first.

Interview transcripts and raw notes may be referenced via `research/sources.md` (Google Drive links). Those are for human reference only — you can't read them directly. If raw research context is needed, ask the user to paste or export the relevant portion into the repo.

---

## Things this client has specifically asked for or against

[This section is where the gotchas live. It's the most important part of this file over the life of a project, and the most frequently updated. Every time the client says "please never do X" or "we always do Y," capture it here.]

- [e.g., "Client does not want any stock photography. Illustrations or nothing."]
- [e.g., "Avoid the word 'seamless' — they've explicitly said it's overused in their industry."]
- [e.g., "Client prefers em dashes to semicolons. Their in-house style guide calls for this."]
- [e.g., "Never reference competitors by name in copy, even favorably."]

---

## Preferred modes of working

**When in doubt, plan first.** For any task that would take more than a handful of tool calls, use plan mode. Return a detailed plan and wait for confirmation before executing. This saves tokens and prevents mistakes from half-understood prompts.

**Ask clarifying questions early.** If a request is ambiguous, ask before acting. It's cheaper to clarify upfront than to redo work.

**Use sub-agents for bounded tasks.** For tasks like "read the full Figma file and keep track of node IDs," delegate to a sub-agent rather than holding that context in the main session.

**Prune your own memory.** Between major tasks, review this file and your session memory and remove anything that's no longer relevant.

---

## Review and PR conventions

Nothing reaches `main` without a PR approved by [project lead name]. When you finish a piece of work:

1. Confirm you're on a branch (not `main`).
2. Group commits logically — one commit per meaningful chunk, not one giant commit.
3. Write commit messages that describe the *why*, not just the *what*. "Add hero section" is weak; "Add hero section per updated brand direction from client review" is better.
4. When the user asks you to prepare a PR description, include: what changed, why, how to test it, and any follow-up needed.
5. Never push directly to `main`, even if you have the permissions to do so.

---

## Open questions and unknowns

[Things that aren't yet decided. Update as the project progresses.]

- [e.g., "Final typography direction — two options under client review, decision expected by [date]."]
- [e.g., "Whether the blog section will be CMS-driven or markdown-based. Waiting on client infra team."]

---

## Project history and decisions

[Running log of significant decisions. Not everything — just the things that would be useful to surface six months from now when someone asks "why did we do it this way?"]

- **[Date]** — Chose [X] over [Y] because [reason].
- **[Date]** — Client approved [decision] in [meeting/review].

---

## What NOT to do

A few absolute rules for this project:

- **Do not write to `main`.** Always work on a branch.
- **Do not delete files you didn't create.** If a file seems obsolete, ask first.
- **Do not commit secrets, access tokens, or client credentials.** If you see any in the repo, flag it immediately.
- **Do not reference or paste content from files the user hasn't explicitly pointed you at.** Respect the scope of what's been asked.
- **Do not make up data.** If you need a value and don't have one, say so. Use clearly-labeled placeholders if placeholders are required.

---

## Meta: maintaining this file

This file is a living document. Update it when:

- The client tells you something that should affect future work
- You notice Claude repeating a mistake that a rule here would prevent
- A convention changes
- A project phase transitions

Don't update it for:

- One-off quirks that won't recur
- Temporary workarounds (those go in task-level discussion)
- Things Claude should figure out from reading the codebase itself

When in doubt, err toward keeping this file shorter. A focused CLAUDE.md produces more reliable Claude behavior than a comprehensive one.
