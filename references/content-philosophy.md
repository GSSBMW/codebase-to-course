# Content Philosophy

> **When to read this:** During Phase 2.5 (writing module briefs) and Phase 3 (writing module HTML). These principles guide every content decision — what to show, how to explain it, and how to test understanding.

These principles are what separate a great course from a generic tutorial. They should guide every content decision:

### Show, Don't Tell — Aggressively Visual
People's eyes glaze over text blocks. The course should feel closer to an infographic than a textbook. Follow these hard rules:

**Text limits:**
- Max **2-3 sentences** per text block. If you're writing a fourth sentence, stop and convert it into a visual instead.
- No text block should ever be wider than the content width AND taller than ~4 lines. If it is, break it up with a visual element.
- Every screen must be **at least 50% visual** (diagrams, code blocks, cards, animations, badges — anything that isn't a paragraph).

**Convert text to visuals:**
- A list of 3+ items → **cards with icons** (pattern cards, feature cards)
- A sequence of steps → **flow diagram with arrows** or **numbered step cards**
- "Component A talks to Component B" → **data flow drawn through the owning
  components**; animate only when motion adds meaning
- "This file does X, that file does Y" → **visual file tree with annotations** or **icon + one-liner badges**
- Explaining what code does → **code↔English translation block** (not a paragraph *about* the code)
- Comparing two approaches → **side-by-side columns** with visual contrast

**Visual breathing room:**
- Use generous spacing between elements (`--space-8` to `--space-12` between sections)
- Alternate between full-width visuals and narrow text blocks to create rhythm
- Every module should have at least one "hero visual" — a diagram, animation, or interactive element that dominates the screen and teaches the core concept at a glance

### Code ↔ English Translations
Every code snippet gets a side-by-side plain English translation. Left panel: real code from the project with syntax highlighting. Right panel: line-by-line plain English explaining what each line does. This is the single most valuable teaching tool for non-technical learners.

**Critical: No horizontal scrollbars on code.** All code must use `white-space: pre-wrap` so it wraps instead of scrolling. This is a course for non-technical people, not an IDE — readability beats preserving indentation structure.

**Critical: Use original code exactly as-is.** Never modify, simplify, or trim code snippets from the codebase. The learner should be able to open the real file and see the exact same code they learned from — that builds trust. Instead of editing code to make it shorter, *choose* naturally short, punchy snippets (5-10 lines) from the codebase that illustrate the concept well. Every codebase has compact, self-contained moments — find those rather than butchering longer functions.

Translate a condition as the concrete question the program asks, then translate the
next line as the action taken when the answer is yes. Define what a counter counts,
where it accumulates, and when it resets; “the interval counter is above zero” is not
plain English. For a comparison or assertion between measurements, name both readings,
when and where each was taken, the expected relationship, and what a failed comparison
means. Lead error-class explanations with the operational outcome—one request fails,
one worker fails, or the whole engine becomes unusable—before discussing class
hierarchy or source-file organization.

### Scope Runtime Paths and Observable Strings
For every cited implementation, say whether it is the default path at the pinned
snapshot, a configuration-selected alternative, a fallback, legacy, test-only, or
unreachable. Trace a user-facing default through its declared value, overrides or
automatic selection, and the value actually consumed. File proximity is not evidence
that a path runs.

Copy log lines, errors, status fields, flag values, environment variables, and API or
UI literals exactly, including spelling, capitalization, punctuation, and field order.
If displayed text is reconstructed after formatting, shortened, or invented for
teaching, label it as example output, abridged, or paraphrased rather than presenting
it as a literal emitted by the project.

### One Concept Per Screen
No walls of text. Each screen within a module teaches exactly one idea. If you need more space, add another screen — don't cram.

### Mechanism First; Metaphor Only When It Helps
State the real actors, action, object, and result first. Add an everyday metaphor only
when it makes that mechanism easier to understand in fewer words. If the learner must
decode the metaphor before understanding the system, remove it. Never recycle a
metaphor merely to satisfy a format requirement.

### Make State and Units Explicit
For every central example, state what the learner supplied, what has been converted,
what has already been computed or cached, what is scheduled now, and what remains.
Distinguish nearby units such as text, tokens, token IDs, token positions, requests,
sequences, batches, blocks, and bytes.

For every consequential number or limit, say:
- what one unit counts;
- which component owns or enforces it;
- whether it is a configured limit, current runtime state, capacity estimate, or
  diagnostic value;
- when it is evaluated; and
- whether the runtime actually consumes the value to make a decision.

For pools and caches, separate three ideas: memory reserved for the shared pool,
blocks assigned to a request as scheduled work needs them, and a hypothetical
maximum future size. Do not imply that dynamic assignment allocates fresh device
memory for every token or reserves every request's maximum context up front.

For iterative or pipelined systems, trace one representative value through adjacent
steps. State what step `N` consumes, what it stores, what it merely selects or emits,
and when that output first becomes input or stored state in step `N+1`. A value can be
committed or returned before its own derived state has been computed; make that lag
visible instead of compressing both events into “computed.”

For an optional optimization, separate three questions: which setting or default
permits it, which runtime predicate exercises it, and which workload or resource
condition makes it beneficial. Enabled does not mean used, and used does not guarantee
better latency, throughput, memory use, or quality.

For a resource-control flag, trace the actual formula and allocation order before
giving tuning advice. Separate fixed costs such as model weights from the adjustable
remainder such as cache capacity. Never claim that lowering a total budget “leaves
more room” for a fixed allocation unless the source shows that allocation is governed
by the setting; name the failure stage and the trade-off instead.

### Make Labels Stand Alone
Write headings, captions, legends, branch labels, table headers, and card titles so
they still make sense when read by themselves. Name the exact subject instead of using
an idiom, vague comparison, or context-dependent word such as "it," "same," "group,"
"read," or "spread thin." A short label may omit detail, but it must not omit the noun
that tells the learner what is being counted, moved, compared, or repeated.

Scan visible content in document order. Define every central noun visibly at or before
its first occurrence; headings, captions, tables, translations, and badges all count
as first use. A later tooltip does not repair an earlier undefined use.

Use one canonical noun for each important object or state. Do not switch between an
exact term and friendly synonyms such as “opening,” “room,” “slot,” or “window” when
the synonym could be mistaken for a different unit or lifecycle state. Before the
technical term is introduced, spell out the mechanism directly; after it is defined,
use that term consistently.

Read the module title, subtitle, screen heading, introductory copy, and first visual
title in order. Each layer must add scope, carried state, a question, or a learner
action. Remove a layer that only repeats the same promise.

### Explain Structures and Calculations by Tracing One Value
For every table or tensor, define both axes and say what one cell stores, including its
unit or identifier domain. Then trace one concrete value from its source, through the
lookup or transformation, to its destination. Render table data with real row and
column headers, aligned cells, and visible boundaries rather than card-like boxes.

For every formula, define each operand, the result, and their units or identifier
domains; explain why the conversion exists; then substitute one small numeric example.
When two kinds of blocks, addresses, counters, or IDs coexist, name both domains and
state which domain the result belongs to.

### Qualify Performance Claims
Every claim that something is faster, slower, cheaper, more scalable, or more
memory-efficient must state the comparison baseline and the workload, model,
hardware or backend, and configuration conditions that materially determine the
result. Say whether the evidence is measured, estimated, theoretical, or a causal
mechanism. If those conditions are unavailable, teach what the feature can do under
the stated conditions instead of promising an unconditional outcome.

Do not turn a capacity ratio into a live-concurrency guarantee, a memory saving into a
throughput guarantee, cache occupancy into useful work, or one benchmark into a
universal result.

### Describe Lifecycles and Related Sets Precisely
Use resource verbs deliberately. Reserving a pool, assigning an item from that pool,
releasing an ownership reference, reusing the item, and erasing its old bytes are
different events. Name the trigger, actor, resource, state change, and next action.
For shared resources, state whether ownership is reference-counted and what happens
when only one owner releases it.

Record three locations separately when they differ: which component logically owns
the state, which process executes the operation, and where the underlying bytes or
device resource physically reside.

When metrics overlap, state the whole-set/subset relationship before interpreting the
numbers. Use one arithmetic example, such as "10 requests are waiting; 3 of those 10
are deferred, so the total is still 10, not 13." When several counters can display the
same number, trace their lineage: configured ceiling, resolved runtime limit, and
fresh per-step remaining counter are different values even when all equal 2,048.

### Treat Diagrams as Executable Explanations
Choose one grammar per figure: architecture, data flow, sequence, transformation,
comparison, or decision. When the lesson is about ownership and movement, draw the
route directly through components inside their process boundaries. Show the complete
initial route without clicks; interaction may highlight it. Label arrows with the
payload or transformation, mark repeated loops, and remove guide text, legends, chips,
or decorative arrows that repeat visible information.

When color marks one branch, state, or comparison as the followed example, add a text
label that says what is selected. Remove active styling when no real alternatives are
present; color alone must not invent or hide meaning.

### Learn by Tracing
Follow what actually happens when the learner does something they already do every day in the app — trace the data flow end-to-end. "You know that button you click? Here's the journey your data takes after you click it..." This works because the learner has *already experienced the result* — now they're seeing the machinery behind it. It's like watching a behind-the-scenes documentary of a movie you loved.

### Make It Memorable
Use "aha!" callout boxes for universal CS insights. Use humor where natural (not forced). Give components personality — they're "characters" in a story, not abstract boxes on a diagram.

### Glossary Support Without Tooltip Noise
Define central vocabulary in visible prose at the moment the concept is introduced.
Use a dashed-underline tooltip only when a genuinely technical definition is useful
but not essential to reading the sentence. Do not tooltip basic words, repeated terms,
or labels whose meaning the surrounding figure already states. A learner must not need
hover or tap to understand the lesson.

Maintain one course-wide tooltip registry. A technical term may have one canonical
tooltip at its first useful course-wide occurrence; later modules use the term without
another tooltip unless the meaning changes and that change is explained visibly. Do
not treat each module as a fresh glossary.

Tooltips should contain a short, usable definition. Acronyms still need a visible
expansion or inline definition on first use when they are central to the lesson.

**Cursor:** Use `cursor: pointer` on terms (not `cursor: help`). The question-mark cursor feels clinical — a pointer feels clickable and inviting.

**Tooltip overflow fix:** Translation blocks and other containers with `overflow: hidden` will clip tooltips. To fix this, the tooltip JS must use `position: fixed` and calculate coordinates from `getBoundingClientRect()` instead of relying on CSS `position: absolute` within the container. Append tooltips to `document.body` rather than inside the term element. This ensures tooltips are never clipped by any ancestor's overflow.
