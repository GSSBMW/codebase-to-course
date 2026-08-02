# Module Brief Template

> **When to read this:** During Phase 2.5 (planning checkpoint), before dispatching any writing agents. Fill in one brief per module, save to `course-name/briefs/0N-slug.md`. Each brief gives a parallel agent everything it needs to write one module without reading the codebase or SKILL.md.

---

## Module N: [Title]

### Teaching Arc
- **Opening hook:** [1 sentence that connects to something the learner already knows from using the app]
- **Key insight:** [The one thing the learner should walk away understanding]
- **"Why should I care?":** [How this helps them steer AI / debug / make decisions]
- **Optional metaphor:** [Only include one if it is simpler than the literal mechanism]

### Learner Contract

- **Central nouns:** [Define request, sequence, batch, block, worker, etc. before labels]
- **Example state:** [What the user supplied; what is tokenized/computed/cached;
  what is scheduled now; what remains]
- **Adjacent-step transitions:** [For steps N and N+1, what enters N; what N reads,
  computes, stores, samples, or emits; and when that output first becomes input or
  stored state]
- **Units:** [For each important value, what exactly does one unit count?]
- **Scope and cadence:** [Default/backend/configuration and whether each action occurs
  per request, per step, per token, at startup, or only on an optional path]
- **Ownership and lifecycle:** [Who reserves, assigns, owns, releases, reuses, or only
  reports each resource? Which component owns it, which process performs the action,
  and where do the bytes or device resource physically reside? What exact state
  transition does the example show?]
- **Feature applicability:** [Configuration or default → runtime eligibility or
  selection → workload or resource condition required for benefit]
- **Related metrics:** [For overlapping sets or counters with similar values, define
  whole/subset relationships and configured ceiling → resolved value → mutable counter]
- **Tables, tensors, and formulas:** [Define both axes and one cell for each structure;
  define every operand, unit/ID domain, result, necessity, and one numeric substitution]
- **Standalone language:** [Write literal, self-contained headings, captions, legends,
  branch labels, and table headers; avoid idioms and missing nouns]
- **First-use map:** [term → exact first visible sentence/cell → visible definition;
  list optional tooltip-only terms separately]
- **Operation ledger:** [actor/owner → action → object → before/after state → trigger,
  cadence, and any process boundary]
- **Resource verbs:** [For reserve, assign, claim, release, free, evict, reclaim, and
  reuse, state the exact transition and what does not happen]

### Consequential Claim Ledger

| Claim | Source evidence | Configuration and runtime path | Applicability or performance conditions | Runtime role or evidence kind |
|---|---|---|---|---|
| [claim] | [path:symbol or lines] | [entrypoint; default, conditional, fallback, legacy, test-only, or unreachable; selection conditions] | [workload, model, hardware/backend, metric window, or N/A] | [controls behavior, current state, capacity estimate, diagnostic, subset relationship; measured, estimated, theoretical, or mechanistic] |

### Observable String Ledger

| Displayed text | Source literal or formatter | Emission conditions and path | Presentation status |
|---|---|---|---|
| [text] | [path:symbol or lines] | [when and whether this path runs] | [exact, formatted example, abridged, or paraphrased] |

### Code Snippets (pre-extracted)

Include the actual code the module will use in code↔English translation blocks. Copy-paste from the codebase with file path and line numbers. The writing agent will use these verbatim — it will NOT re-read the codebase. For each snippet, state the runtime path and selection conditions that make it execute; file and line evidence alone is insufficient when old and new implementations coexist.

File: src/example/file.ts (lines 12-24)
[paste actual code here]

File: src/another/file.ts (lines 45-52)
[paste actual code here]

### Interactive Elements

Check which elements this module needs. Include enough detail for the writing agent to build them.

- [ ] **Code↔English translation** — which snippet(s) from above
- [ ] **Static course diagram** — grammar, visible title, complete initial state,
      labelled route or axes, one worked value, and takeaway
- [ ] **Architecture + data flow** — ownership zones, components, route, arrow
      payloads, numbered stages, repeat cadence, and complete initial state
- [ ] **Data flow animation** — optional highlights over the complete static route
- [ ] **Group chat animation** — only if the mechanism is genuinely message-like
- [ ] **Other** — [architecture diagram, layer toggle, pattern cards, etc.]

### Reference Files to Read

List only the sections the writing agent needs — not the whole file.

- `references/interactive-elements.md` → [section names, e.g., "Static Course Diagrams", "Message Flow / Data Flow Animation"]
- `references/design-system.md` → [only if needed for specific tokens not in the brief]
- `references/content-philosophy.md` → [always include — agent needs content rules]
- `references/gotchas.md` → [always include — agent needs the checklist]

### Connections

- **Previous module:** [Title — what it covered, so this module can build on it]
- **Next module:** [Title — what it will cover, so this module can set it up]
- **Tone/style notes:** [Any course-wide consistency notes: accent color name, actor naming convention, etc.]
