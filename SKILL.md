---
name: codebase-to-course
description: "Turn any codebase into a beautiful, interactive single-page HTML course that teaches how the code works to non-technical people. Use this skill whenever someone wants to create an interactive course, tutorial, or educational walkthrough from a codebase or project. Also trigger when users mention 'turn this into a course,' 'explain this codebase interactively,' 'teach this code,' 'interactive tutorial from code,' 'codebase walkthrough,' 'learn from this codebase,' or 'make a course from this project.' This skill produces a stunning, self-contained HTML file with scroll-based navigation, animated visualizations, and code-with-plain-English side-by-side translations."
---

# Codebase-to-Course

Transform any codebase into a stunning, interactive course. The output is a **directory** containing a pre-built `styles.css`, `main.js`, per-module HTML files, and an assembled `index.html` — open it directly in the browser with no setup required (only external dependency: Google Fonts CDN). The course teaches how the code works through scroll-based modules, animated visualizations, and plain-English translations of code.

## First-Run Welcome

When the skill is first triggered and the user hasn't specified a codebase yet, introduce yourself and explain what you do:

> **I can turn any codebase into an interactive course that teaches how it works — no coding knowledge required.**
>
> Just point me at a project:
> - **A local folder** — e.g., "turn ./my-project into a course"
> - **A GitHub link** — e.g., "make a course from https://github.com/user/repo"
> - **The current project** — if you're already in a codebase, just say "turn this into a course"
>
> I'll read through the code, figure out how everything fits together, and generate a beautiful single-page HTML course with animated diagrams, plain-English code explanations, and step-by-step walkthroughs of how your data moves through the system. The whole thing runs in your browser — no setup needed.

If the user provides a GitHub link, clone the repo first (`git clone <url> /tmp/<repo-name>`) before starting the analysis. If they say "this codebase" or similar, use the current working directory.

## Who This Is For

The target learner is a **"vibe coder"** — someone who builds software by instructing AI coding tools in natural language, without a traditional CS education. They may have built this project themselves (without looking at the code), or they may have found an interesting open-source project on GitHub and want to understand how it's built. Either way, they don't yet understand what's happening under the hood.

**Assume zero technical background.** Every CS concept — from variables to APIs to databases — needs to be explained in plain language as if the learner has never encountered it. No jargon without definition. No "as you probably know." The tone should be like a smart friend explaining things, not a professor lecturing.

**Their goals are practical, not academic:**
- Have enough technical knowledge to effectively **steer AI coding tools** — make better architectural and tech stack decisions
- **Detect when AI is wrong** — spot hallucinations, catch bad patterns, know when something smells off
- **Intervene when AI gets stuck** — break out of bug loops, debug issues, unblock themselves
- Build more advanced software with **production-level quality and reliability**
- Be **technically fluent** enough to discuss decisions with engineers confidently
- **Acquire the vocabulary of software** — learn the precise technical terms so they can describe requirements clearly and unambiguously to AI coding agents (e.g., knowing to say "namespace package" instead of "shared folder thing")

**They are NOT trying to become software engineers.** They want coding as a superpower that amplifies what they're already good at. They don't need to write code from scratch — they need to *read* it, *understand* it, and *direct* it.

## Why This Approach Works

This skill inverts traditional CS education. The old model is: memorize concepts for years → eventually build something → finally see the point (most people quit before step 3). This model is: **build something first → experience it working → now understand how it works.**

The learner already has context that traditional students don't — they've *used* the app, they know what it does, they may have even described its features in natural language. The course meets them where they are: "You know that button you click? Here's what happens under the hood when you click it."

Every module answers **"why should I care?"** before "how does it work?" The answer to "why should I care?" is always practical: *because this knowledge helps you steer AI better, debug faster, or make smarter architectural decisions.*

The directory-based output is intentional: separating CSS/JS from content means AI never regenerates boilerplate, each module is written independently (keeping output size small and quality high), and the assembled `index.html` works offline with zero setup.

---

## The Process

### Phase 1: Codebase Analysis

Before writing course HTML, deeply understand the codebase. Read all the key files, trace the data flows, identify the "cast of characters" (main components/modules), and map how they communicate. Thoroughness here pays off — the more you understand, the better the course.

**What to extract:**
- The main "actors" (components, services, modules) and their responsibilities
- The primary user journey (what happens when someone uses the app end-to-end)
- Key APIs, data flows, and communication patterns
- The important data states and units at each stage: text, tokens, token IDs,
  requests, sequences, batches, blocks, bytes, and so on
- For iterative or pipelined paths, which step produces a value, which later step
  first consumes or stores it, and which completed-state snapshot a concurrent
  planner can see. Do not treat “selected,” “committed,” “returned,” “processed,”
  and “cached” as the same state.
- The logical owner, executing process, and physical storage location of important
  components and resources; these may be three different answers
- Which numbers configure runtime behavior, which describe current state, and which
  are only estimates or startup diagnostics
- For each status field or metric, its numerator or event count, denominator when
  applicable, aggregation window, reset point, process or engine scope, and whether
  it is always emitted, conditionally emitted, or exported through another surface
- For each resource-setting recommendation, the allocation order and arithmetic:
  which quantity is a total budget, which costs are fixed, which remainder changes,
  which startup or runtime stage fails, and whether raising or lowering the setting
  can actually affect that failure
- For each feature or optimization, the setting or default that permits it, the
  runtime condition that actually selects it, and the workload or resource condition
  required for a real benefit
- Clever engineering patterns (caching, lazy loading, error handling, etc.)
- Real bugs or gotchas (if visible in git history or comments)
- The tech stack and why each piece was chosen

**Figure out what the app does yourself** by reading the README, the main entry points, and the UI code. Don't ask the user to explain the product — they may not be familiar with it either. The course should open by explaining what the app does in plain language (a brief "here's what this thing does and why it's interesting") before diving into how it works. The first module should start with a concrete user action — "imagine you paste a YouTube URL and click Analyze — here's what happens under the hood."

**Start with the recent release history, not the code.** Spend a few minutes on what changed in roughly the six months before the commit the course will pin. Reading source and design docs tells you what *exists*; it does not tell you which of two coexisting things actually *runs*, and that is where courses go wrong. A mature project keeps the old implementation beside the new one for a release or two, and a default flips without a single line of the taught file changing. Design docs are worse than silent — they go stale in place, so a course that mines them teaches last year's system with this year's confidence.

```bash
gh release list --repo <owner>/<name> --limit 12
gh release view <tag> --repo <owner>/<name> --json body -q .body   # read Highlights first
```

Without `gh` or a network, a plain clone still gets most of it:

```bash
git tag --sort=-creatordate --format='%(creatordate:short)  %(refname:short)' | head -20
git log <previous-tag>..<tag-at-or-before-pin> --oneline | grep -iE 'default|deprecat|remov|breaking'
```

Two traps worth knowing before you run these:

- **Select releases by date, not by ancestry.** Many projects cut release tags on release branches that never merge back, so the tag is not an ancestor of the commit you are pinning. `git describe`, `git tag --contains` and `merge-base --is-ancestor` then all report that your commit predates releases it actually contains — off by several releases, confidently.
- **If the pinned commit sits on a mainline after the newest release**, those changes are in your commit but in nobody's notes. Sweep them with `git log <newest-tag-before-pin>..<pin> --oneline`.

Extract four things, in order of how badly they hurt a course: **defaults that flipped** (ask of every subsystem — is the file I am about to quote the one that runs by default at this commit?), **things removed** (watch for a concept outliving its implementation: the idea may still be how the system works while the class or kernel that carried the name is gone), **old and new sitting side by side** (a `v2/` beside a `v1/` — teach whichever runs by default and name the other, so a reader who opens the file is not reading dead code), and **notable additions** a competent user would now expect the course to cover.

Treat all of it as a pointer, never the authority. Release notes describe intent at release time; the code at the pinned commit is what the course explains, so verify every claim there. Notes routinely disagree with in-tree documentation — a README still calling something "experimental" while the config makes it the default is common, and the config wins.

### Phase 2: Curriculum Design

Structure the course as **4-6 modules**. Most courses need 4-6. Only go to 7-8 if the codebase genuinely has that many distinct concepts worth teaching. Fewer, better modules beat more, thinner ones.

The arc always starts from what the learner already knows (the user-facing behavior) and moves toward what they don't (the code underneath). Think of it as zooming in: start wide with the experience, then progressively peel back layers.

| Module Position | Purpose | Why it matters for a vibe coder |
|---|---|---|
| 1 | "Here's what this app does — and what happens when you use it" | Start with the product (what it does, why it's interesting), then trace a core user action into the code. Grounds everything in something concrete. |
| 2 | Meet the actors | Know which components exist so you can tell AI "put this logic in X, not Y" |
| 3 | How the pieces talk | Understand data flow so you can debug "it's not showing up" problems |
| 4 | The outside world (APIs, databases) | Know what's external so you can evaluate costs, rate limits, and failure modes |
| 5 | The clever tricks | Learn patterns (caching, chunking, error handling) so you can request them from AI |
| 6 | When things break | Build debugging intuition so you can escape AI bug loops |
| 7 | The big picture | See the full architecture so you can make better decisions about what to build next |

This is a **menu, not a checklist**. Pick the modules that serve the codebase — a simple CLI tool needs 4, not 7. Adapt the arc to the codebase's complexity.

**The key principle:** Every module should connect back to a practical skill — steering AI, debugging, making decisions. If a module doesn't help the learner DO something better, cut it or reframe it until it does.

**Each module should contain:**
- 3-6 screens (sub-sections that flow within the module)
- At least one code-with-English translation
- At least one explanatory visual (static or interactive)
- One or two "aha!" callout boxes with universal CS insights
- A metaphor only when it makes the mechanism easier to understand in fewer words.
  Omit it when the literal process is clearer.

**Core teaching elements:**
- **Code ↔ English Translation Blocks** — at least one per module.
- **Architecture and data flow** — when ownership and movement are both important,
  draw one static figure with process boundaries, components, arrows, payloads, and
  numbered stages. Do not require clicks to reveal the essential route.
- **Glossary support** — define central terms inline. Use a tooltip only for an
  optional definition of a genuinely technical term; do not tooltip basic or repeated
  words.
- **Interaction** — use animation, toggles, or a group chat only when the interaction
  matches the mechanism and improves comprehension. Interaction may highlight a
  complete explanation; it must not contain the only explanation.

**Do NOT present the curriculum for approval — just build it.** The user wants a course, not a planning document. Design the curriculum internally, then go straight to building. If they want changes, they'll tell you after seeing the result.

**Every course is built the same way:** write a brief per module (Phase 2.5), then hand the briefs to parallel subagents that write the HTML (Phase 3). There is no single-context path — even a small codebase goes through briefs. That is what keeps each module's quality high and each writing agent's context small.

### Phase 2.5: Module Briefs

Write a course contract and one brief per module before writing any HTML. This is the
critical step that enables parallel writing without letting independently written
modules invent different names, scopes, states, or visual rules.

Read `references/course-contract-template.md` and write
`course-name/briefs/00-course-contract.md` first. Record the pinned source revision and
default or optional execution paths, the canonical actor and process registry, shared
example labels, state and unit vocabulary, color assignments, target desktop/mobile
widths, source-snippet rules, HTML constraints, and a course-wide glossary registry
that records each term's first visible definition and its single optional tooltip
location. Then read
`references/module-brief-template.md` for the module structure and
`references/content-philosophy.md` for the content rules.

**For each module, write a brief to `course-name/briefs/0N-slug.md` containing:**
- Teaching arc (opening hook, key insight, and an optional metaphor)
- Central noun definitions and the exact lifecycle state used by the example
- A compact state-and-units ledger for the concrete example
- A step-indexed transition ledger for iterative paths: input state, work performed,
  state persisted, output selected or emitted, and the later step that first consumes
  or stores that output
- The ownership and resource lifecycle used by the example: who allocates, assigns,
  releases, reuses, or merely reports each resource, plus where it physically resides
  and which process executes the relevant operation
- The shape and cell meaning of every important table or tensor, plus operand and
  result definitions for every formula the learner must follow
- A claim ledger for consequential counts, limits, defaults, ownership boundaries,
  set/subset relationships, and performance claims, including source evidence and
  whether each value controls runtime behavior or only reports/estimates it. For a
  feature claim, separate configuration, runtime selection, and benefit conditions.
- For code conditions and assertions, the plain-English question being tested, both
  operands or readings, when each value was recorded, the expected relationship, and
  the consequence when the test fails. For mutable counters, include accumulation,
  reset, and reporting scope.
- For configuration advice, the controlled budget or policy, fixed versus adjustable
  costs, allocation order, failure stage, direction of change, and resulting trade-off.
- Pre-extracted code snippets (copy-pasted from the codebase with file paths and line numbers)
- Visual and interactive elements with enough detail to build them, including
  ownership zones, route, payloads, table dimensions, and repeat cadence when relevant
- Standalone wording for headings, legends, branch labels, and table headers; each
  must name its subject without depending on a nearby paragraph for its meaning
- Which sections of which reference files the writing agent needs
- What the previous and next modules cover (for transitions)

The code snippets are the critical token-saving step. By pre-extracting them into the brief, writing agents never need to read the codebase at all.

### Phase 3: Build the Course

The course output is a **directory**, not a single file. All CSS and JS are pre-built reference files — never regenerate them. Your job is to write only the HTML content.

**Output structure:**
```
course-name/
  styles.css       ← copied verbatim from references/styles.css
  main.js          ← copied verbatim from references/main.js
  _base.html       ← customized shell (title, accent color, nav dots)
  _footer.html     ← copied verbatim from references/_footer.html
  build.sh         ← copied verbatim from references/build.sh
  briefs/          ← course contract plus module briefs; keep with the course
  modules/
    01-intro.html
    02-actors.html
    ...
  index.html       ← assembled by build.sh (do not write manually)
```

**Step 1: Setup** — Create the course directory. Copy these four files verbatim using Read + Write (do not regenerate their contents):
- `references/styles.css` → `course-name/styles.css`
- `references/main.js` → `course-name/main.js`
- `references/_footer.html` → `course-name/_footer.html`
- `references/build.sh` → `course-name/build.sh`

**Step 2: Customize `_base.html`** — Read `references/_base.html`, then write it to `course-name/_base.html` with exactly three substitutions:
- Both instances of `COURSE_TITLE` → the actual course title
- The four `ACCENT_*` placeholders → the chosen accent color values (pick one palette from the comments in `_base.html`)
- `NAV_DOTS` → one `<button class="nav-dot" ...>` per module

**Step 3: Write modules** — Dispatch modules to subagents in batches of up to 3. Each agent receives:
- `course-name/briefs/00-course-contract.md`
- Its module brief (from `course-name/briefs/`)
- `references/content-philosophy.md` and `references/gotchas.md`
- Only the sections of `references/interactive-elements.md` and `references/design-system.md` listed in the brief

Each agent writes its module file(s) to `course-name/modules/`. Short modules (3 screens, one animation) can be paired — two briefs given to one agent.

**Every agent prompt must state the output format**, since agents never see SKILL.md: write `course-name/modules/0N-slug.html` containing only the `<section class="module" id="module-N">` block and its contents — no `<html>`, `<head>`, `<body>`, `<style>`, or `<script>` tags. The shell and assets already exist; the agent is writing a fragment, not a page.

**What agents do NOT receive:** the full codebase (snippets are in the brief), SKILL.md, other modules' briefs, or unneeded reference file sections.

After all agents finish, reconcile every module against its brief and the course
contract in the main context. Verify canonical actor names and colors, process
boundaries, example-state handoffs, first-use definitions, feature scope, source
snippets, course-wide tooltip locations, nav dots, transitions, and HTML constraints.
Remove repeated tooltip definitions instead of treating each module as a new glossary.
Recheck any repair that spans
multiple modules instead of assuming independently edited files still agree.

**Step 4: Assemble** — Run `build.sh` from the course directory:
```bash
cd course-name && bash build.sh
```
This produces `index.html`. Open it in the browser.

**Critical rules:**
- **Never regenerate** `styles.css` or `main.js` — always copy from references
- Module files contain only `<section>` content — no boilerplate
- Use CSS `scroll-snap-type: y proximity` (NOT `mandatory`)
- On `.module`, declare `min-height: 100vh` first and `min-height: 100dvh` second — the later declaration wins, so reversing them silently disables `dvh`
- Interactive element JS is in `main.js`; wire up via `data-*` attributes and CSS class names as shown in `references/interactive-elements.md`
- Chat containers need `id` attributes; flow animations need `data-steps='[...]'` JSON on `.flow-animation`

### Phase 4: Review and Open

After running `build.sh`, review the assembled course as both a first-time learner and
a source verifier. Use `review-generated-course` when available. Confirm that important
nouns, units, state transitions, limits, and diagrams remain understandable without
hovering or clicking. Inspect representative desktop and mobile widths, then walk the
user through what was built and ask for feedback on content, design, and interactivity.

---

## Design Identity

The course is **dark-themed** — it should feel like a beautiful developer notebook read at night: warm, inviting, and distinctive, never a cold black terminal. Read `references/design-system.md` for the full token system, but here are the non-negotiable principles:

- **Warm dark palette**: Backgrounds are warm charcoal (`#2B2521`), NOT pure black and NOT cold blue-black. Text is warm off-white, never pure white.
- **Layered by lightness**: Depth comes from surface lightness, not just shadow — code blocks are the darkest layer, then the page, then alternating modules, then cards. Never flatten this order.
- **Bold accent**: One confident accent color (vermillion, coral, teal — NOT purple gradients). On dark, hover goes *lighter* than the base, and the `-light` token is a dark tint used as a background, not a pale wash.
- **Dark text on bright fills**: Filled accent chips (buttons, step numbers, avatars) use the page's dark color as their text, not white — it reads far more crisply on a dark theme.
- **Distinctive typography**: Display font with personality for headings (Bricolage Grotesque, or similar bold geometric face — NEVER Inter, Roboto, Arial, or Space Grotesk). Clean sans-serif for body (DM Sans or similar). JetBrains Mono for code.
- **Generous whitespace**: Modules breathe. Max 3-4 short paragraphs per screen.
- **Alternating backgrounds**: Odd/even modules alternate between two warm dark tones for visual rhythm. This is automatic in `styles.css` — module HTML never sets a background.
- **Dark code blocks**: IDE-style with Catppuccin-inspired syntax highlighting on deep indigo-charcoal (#1E1E2E) — the darkest element on the page, so code reads as inset.
- **Depth without harshness**: Shadows are deep enough to register on a dark page, paired with lighter surfaces to carry elevation.

---

## Reference Files

The `references/` directory contains detailed specs. **Read them only when you reach the relevant phase** — not upfront. This keeps context lean.

- **`references/content-philosophy.md`** — Visual density rules, metaphor guidelines, tooltip rules, code translation guidance. Read during Phase 2.5 (briefs) and Phase 3 (writing modules).
- **`references/gotchas.md`** — Common failure points checklist. Read during Phase 3 and Phase 4 (review).
- **`references/module-brief-template.md`** — Template for Phase 2.5 module briefs. Read during Phase 2.5, before dispatching any writing agents.
- **`references/design-system.md`** — Complete CSS custom properties, color palette, typography scale, spacing system, shadows, animations, scrollbar styling. Read during Phase 3 when writing module HTML.
- **`references/interactive-elements.md`** — Implementation patterns for every interactive element: code↔English translations, group chat animations, message flow visualizations, architecture diagrams, layer toggles, pattern cards, callout boxes, glossary tooltips. Read the relevant sections during Phase 3.
