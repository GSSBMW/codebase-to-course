# Codebase to Course

A Claude Code skill that turns any codebase into a beautiful, interactive single-page HTML course.

Point it at a repo. Get back a stunning, self-contained course that teaches how the code works — with scroll-based navigation, animated visualizations, and code-with-plain-English side-by-side translations.

## Who is this for?

**"Vibe coders"** — people who build software by instructing AI coding tools in natural language, without a traditional CS education.

You've built something (or found something cool on GitHub). It works. But you don't really understand *how* it works under the hood. This skill generates a course that teaches you — not by lecturing, but by tracing what happens when you actually use the app.

**Your goals are practical, not academic:**
- Steer AI coding tools better (make smarter architectural decisions)
- Detect when AI is wrong (spot hallucinations, catch bad patterns)
- Debug when AI gets stuck (break out of bug loops)
- Talk to engineers without feeling lost

You're not trying to become a software engineer. You want coding as a superpower.

## What the course looks like

The output is a **folder you open in the browser** — no build step, no install, no server:

```
my-course/
├── index.html      # the course — open this
├── styles.css
└── main.js
```

The folder also keeps the per-module HTML sources and the `build.sh` that assembled them, so you can edit one module and rebuild without regenerating the course. The only external request is Google Fonts — offline it falls back to system fonts and everything else still works. The course includes:

- **Scroll-based modules** with progress tracking and keyboard navigation
- **Code ↔ Plain English translations** — real code on the left, what it means on the right
<img width="720" alt="Code translation block" src="https://github.com/user-attachments/assets/fb9e7fac-05c1-4f98-b80c-46543ef81afc" />

- **Explanatory visualizations** — static architecture and data-flow diagrams first,
  with animation or group chat when interaction genuinely helps
<img width="720" alt="Animated data flow" src="https://github.com/user-attachments/assets/20fb403e-7dfd-4a47-989b-bbae86ca8041" />

- **Plain-language vocabulary** — central terms are defined in visible text; optional
  technical detail can appear in a tooltip
<img width="720" alt="Glossary tooltip" src="https://github.com/user-attachments/assets/ac2f160a-d73f-4779-97b2-a06fdb5f3227" />

  
- **Warm dark design** — a developer notebook read at night, not a cold black terminal or the typical purple-gradient AI look

## How to use

### As a Claude Code skill

1. Copy the `codebase-to-course` folder into `~/.claude/skills/`
2. Open any project in Claude Code
3. Say: *"Turn this codebase into an interactive course"*

### Trigger phrases

- "Turn this into a course"
- "Explain this codebase interactively"
- "Make a course from this project"
- "Teach me how this code works"
- "Interactive tutorial from this code"

## Design philosophy

### Build first, understand later

This inverts traditional CS education. The old way: memorize concepts for years → eventually build something → finally see the point (most people quit before step 3). This way: **build something → experience it working → now understand how it works.**

### Show, don't tell

Every screen is at least 50% visual. Max 2-3 sentences per text block. If something can be a diagram, animation, or interactive element — it shouldn't be a paragraph.

### Mechanism before metaphor

Name the real actors, action, data, and result first. Add a metaphor only when it is
shorter and clearer than the mechanism itself.

### State, units, and limits are explicit

Examples say what has been supplied, computed, cached, scheduled, and returned. Every
important count names its unit and whether it configures behavior, describes current
state, estimates capacity, or is diagnostic only.

### Original code only

Code snippets are exact copies from the real codebase — never modified or simplified. The learner should be able to open the actual file and see the same code they learned from.

### One brief, one agent, one module

Writing a whole course in a single pass makes the last modules thin and rushed. So every module gets a written brief first — teaching arc, central noun definitions, example-state and claim ledgers, optional metaphor, and pre-extracted code snippets — and then its own writing agent turns that brief into HTML. Modules are written in parallel, and each agent starts with a small, focused context: the brief carries the verified source evidence and code snippets, so writing agents never re-read the codebase.

## Skill structure

```
codebase-to-course/
├── SKILL.md                          # Main skill instructions
└── references/
    ├── content-philosophy.md         # Visual density, metaphors, tooltips, code translations
    ├── design-system.md              # CSS tokens, typography, colors, layout
    ├── gotchas.md                    # Common failure points checklist
    ├── interactive-elements.md       # Animation and visualization patterns
    ├── module-brief-template.md      # Template for the per-module briefs
    ├── _base.html                    # Page shell — title, accent color, nav dots
    ├── _footer.html                  # Closing markup
    ├── build.sh                      # Concatenates the parts into index.html
    ├── main.js                       # All interactivity — copied verbatim, never regenerated
    └── styles.css                    # All styling — copied verbatim, never regenerated
```


---

Built by [Zara](https://x.com/zarazhangrui) with Claude Code.
