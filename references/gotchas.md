# Gotchas — Common Failure Points

> **When to read this:** During Phase 3 (writing module HTML) and Phase 4 (review). Check every one of these before considering a course complete.

These are real problems encountered when building courses. Check every one before considering a course complete.

### Tooltip Clipping
Translation blocks use `overflow: hidden` for code wrapping. If tooltips use `position: absolute` inside the term element, they get clipped by the container. **Fix:** Tooltips must use `position: fixed` and be appended to `document.body`. Calculate position from `getBoundingClientRect()`. This is already handled by `main.js` but is the #1 bug that appears in every build.

### Tooltip Noise or Hidden Definitions
Do not scatter tooltips over basic or repeated words. Define central terms visibly;
reserve tooltips for optional technical detail. If removing a tooltip makes the main
sentence incomprehensible, the definition belongs in the sentence or nearby figure.
Keep a course-wide registry so a later module does not repeat a tooltip already used
earlier in the assembled page.

### Implementation-Centered Code Translations
“This branch checks an interval counter” describes syntax, not behavior. Translate a
condition as the concrete question it asks and the following line as the action taken.
For assertions, name both values, when each was measured, the expected relationship,
and why violating it invalidates the operation. For errors, state the runtime scope
and consequence before naming the exception hierarchy.

### Reversed or Causally Unsafe Knob Advice
A flag may set a total budget while only the remainder is adjustable. Trace the
formula, allocation order, fixed costs, failure stage, and trade-off before recommending
that a user raise or lower it. Lowering a budget does not create space for a fixed cost;
it usually shrinks the adjustable allocation or leaves headroom outside the budget.

### Walls of Text
The course looks like a textbook instead of an infographic. This happens when you write more than 2-3 sentences in a row without a visual break. Every screen must be at least 50% visual. Convert any list of 3+ items into cards, any sequence into step cards or flow diagrams, any code explanation into a code↔English translation block.

### Forced Metaphors
A metaphor that needs its own explanation makes the lesson harder. Start with the
literal mechanism and keep a metaphor only when it shortens the explanation.

### Code Modifications
Trimming, simplifying, or "cleaning up" code snippets from the codebase. The learner should be able to open the real file and see the exact same code. Instead of editing code to be shorter, *choose* naturally short snippets (5-10 lines) from the codebase that illustrate the point.

### Scroll-Snap Mandatory
Using `scroll-snap-type: y mandatory` traps users inside long modules. Always use `proximity`.

### Module Quality Degradation
Trying to write all modules in one pass causes later modules to be thin and rushed. Give each module its own brief and its own writing agent, so every module gets full attention, and verify each module file before assembling.

### Interaction Hides the Lesson
Do not make clicks reveal the only architecture, route, or meaningful state. Render a
complete static explanation first; use interaction only to highlight, filter, compare,
or expand it.

### Detached Architecture and Flow
An inventory of components followed by a separate route forces the learner to merge
two maps mentally. When ownership and movement are the same lesson, draw the route
through the components inside their process boundaries.

### Unclassified Numbers
Counts and limits are misleading when their unit, owner, or runtime role is omitted.
State whether each value is a configured ceiling, per-step budget, current usage,
capacity estimate, or diagnostic output, and whether any scheduler or allocator reads
it later.

### Adjacent Steps Collapsed
A pass can emit or select a value that is not processed or stored until the next
iteration. Trace one value through steps `N` and `N+1`; never use “computed” to merge
selection, commitment, processing, persistence, and return into one event.

### Enabled Means Faster
Configuration only permits a feature. State the separate runtime condition that uses
it and the workload or resource condition required for a benefit. Optional paths can
be enabled yet unused, or used while making latency worse.

### Context-Dependent Labels
Headings such as "spread thin," captions such as "four values can read 2,048," and
labels such as "the group" force the learner to reconstruct missing context. Make every
standalone label name the exact object and action.

### Module Drift
Parallel writing agents can assign different names, colors, boundaries, or states to
the same actor and example. Give every agent the shared course contract, then reconcile
each module against both its brief and that contract before assembly.

### Opaque Tables and Formulas
A grid is not self-explanatory. Define both axes, the meaning of one cell, and one
end-to-end lookup. For a formula, define every operand and the result, including units
or ID domains, explain why the conversion is needed, and work one numeric example.

### Collapsed Resource Lifecycles and Overlapping Metrics
Do not use "allocate," "free," "evict," "erase," and "reuse" as synonyms. Distinguish
pool reservation, per-owner assignment, reference release, and physical reuse. If one
metric includes another, state the subset relationship and show the arithmetic instead
of telling the learner only to compare the two counters.
