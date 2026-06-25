# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

ABURWORK is **not a software project** — there is no build system, package manager, test
suite, or runnable application. It is a curated **design-directive library**: a set of
"skills" (markdown rule files) that govern how Claude generates premium, anti-generic
frontend code and design assets. The deliverable is whatever UI/code/images you produce
*for the user's request*, shaped by these rules — not changes to this repo itself.

There are no commands to build, lint, or test. Do not look for or invent them.

## Structure

- `.claude/rules/llms.txt` — the **skill router/index**. One line per skill describing
  what it is for. Read this first to pick the right skill for a request.
- `.claude/rules/<skill>/SKILL.md` — each skill's full directive. These are loaded as
  project instructions and **override default behavior**.
- `.claude/rules/stitch-skill/DESIGN.md` — a generated, ready-to-use design system
  (tokens, palette, typography, anti-patterns) that doubles as the canonical example of
  what the `stitch-skill` produces.
- `assets/ABURWORK_logo_transparent.png` — the brand logo.
- `assets/Recurify Webflow Template/` — a third-party Webflow export kept as a **visual
  reference** (HTML/CSS/JS/fonts/media). Reference for design analysis only; it is not
  wired into anything and should not be treated as the app.

## How the skills work together

The skills fall into three functional groups (see `llms.txt` for the authoritative list):

1. **Code-generating design skills** — `taste-skill` (the default/main one), `gpt-taste`,
   `soft-skill`, `minimalist-skill`, `brutalist-skill`, `stitch-skill`. Each encodes a
   distinct aesthetic with explicit baseline dials (e.g. `DESIGN_VARIANCE`,
   `MOTION_INTENSITY`, `VISUAL_DENSITY`) and a banned-patterns list.
2. **Image-generation skills** (no code) — `imagegen-frontend-web`,
   `imagegen-frontend-mobile`, `brandkit`. `image-to-code-skill` bridges the two:
   generate reference images first, analyze them, then implement.
3. **Cross-cutting enforcers** — `output-skill` (never emit placeholder/`// ...` code,
   always deliver complete output) and `redesign-skill` (audit-and-fix an existing
   project rather than rewrite).

### Critical: pick ONE aesthetic skill, do not blend

The aesthetic skills deliberately **conflict** with each other, and following all of them
at once produces incoherent output. Examples:
- `gpt-taste` *prefers* centered cinematic heroes and inline-image-in-heading; `stitch`
  and `taste-skill` *ban* centered heroes above variance 4.
- `minimalist`/`brutalist` enforce strict monochrome and ban motion libraries; `soft` and
  `taste` lean on spring physics, glassmorphism, and rich motion.

Route through `llms.txt` to select the single skill that matches the user's request, then
follow that skill's dials and bans. Treat `output-skill` and `redesign-skill` as additive
(they layer on top of whichever aesthetic skill you chose).

## Shared conventions enforced across (most) skills

These recur across the design skills and are the house style — honor them unless a chosen
skill explicitly overrides:

- **Banned font:** `Inter` (and generic system fonts) for premium contexts. Use `Geist`,
  `Satoshi`, `Cabinet Grotesk`, or `Outfit`.
- **No emojis** anywhere — in code, comments, content, or alt text.
- **No pure black** (`#000000`) — use off-black / Zinc-950.
- **No "AI purple/blue" neon gradients or outer glows.** Max one accent, saturation < 80%.
- **No 3-equal-column card rows.** Use bento/asymmetric/zig-zag grids.
- **Full-height = `min-h-[100dvh]`**, never `h-screen`.
- **Animate only `transform` and `opacity`**; never `top/left/width/height`.
- **No generic placeholder content** — no "John Doe"/"Acme", no fake round numbers, no
  "Elevate/Seamless/Unleash" copy. Images via `picsum.photos/seed/{...}`, never Unsplash.
- **Verify dependencies** against the target project's `package.json` before importing any
  library, and check the Tailwind major version before using v4-only syntax.

## Working in this repo

- If asked to add or edit a skill, follow the existing `.claude/rules/<name>/SKILL.md`
  layout and add a matching one-line entry to `.claude/rules/llms.txt`.
- This directory is not a git repository.
