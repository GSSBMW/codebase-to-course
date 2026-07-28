# Design System Reference

Reference for the course's CSS design tokens — what each one means and when to reach for it while writing module HTML.

> **Do not copy any CSS from this file into a course.** Every token and rule below already ships in `references/styles.css`, which is copied verbatim into the course directory. The only per-course customization is the four `ACCENT_*` values in `_base.html`. Use the tokens by name (`var(--space-6)`, `var(--color-accent)`) in module HTML; never redeclare them, and never add `<style>` blocks.

## Table of Contents
1. [Color Palette](#color-palette)
2. [Typography](#typography)
3. [Spacing & Layout](#spacing--layout)
4. [Shadows & Depth](#shadows--depth)
5. [Animations & Transitions](#animations--transitions)
6. [Navigation & Progress](#navigation--progress)
7. [Module Structure](#module-structure)
8. [Responsive Breakpoints](#responsive-breakpoints)
9. [Scrollbar & Background](#scrollbar--background)

---

## Color Palette

```css
:root {
  /* --- BACKGROUNDS — warm dark, layered darkest→lightest:
         bg-code (inset panels) < bg < bg-warm < surface < surface-warm --- */
  --color-bg:             #2B2521;       /* warm dark charcoal — the page */
  --color-bg-warm:        #332C27;       /* lighter warm, for alternating modules */
  --color-bg-code:        #1E1E2E;       /* deep indigo — darkest, so code reads as inset */
  --color-text:           #EFE9E2;       /* warm off-white, never pure white */
  --color-text-secondary: #BCB2A8;       /* warm gray for secondary text */
  --color-text-muted:     #A0968C;       /* muted for timestamps, labels */
  --color-border:         #473F38;       /* subtle warm border */
  --color-border-light:   #3A332D;       /* quieter border */
  --color-surface:        #39322C;       /* card surfaces, elevated above the page */
  --color-surface-warm:   #403830;       /* warmest card surface */

  /* --- ACCENT (adapt per project — pick ONE bold color) ---
     Default: vermillion. Alternatives: coral (#E87A64), teal (#4BA3C7),
     amber (#E0B851), forest (#4FB37A). Avoid purple gradients.
     Note two dark-theme inversions: hover is LIGHTER than the base, and
     -light is a dark tint used as a background, not a pale wash. */
  --color-accent:         #E85E3D;
  --color-accent-hover:   #F2795A;
  --color-accent-light:   #3D231B;
  --color-accent-muted:   #E8836C;

  /* --- SEMANTIC — brightened for legibility on dark;
         each -light pair is a dark tint for use as a background --- */
  --color-success:        #6FD39A;
  --color-success-light:  #1E3328;
  --color-error:          #F0908C;
  --color-error-light:    #3A2220;
  --color-info:           #6FC5E3;
  --color-info-light:     #172C36;

  /* --- ACTOR COLORS (assign to main components) ---
     Each major "character" in the codebase gets a distinct color
     for chat bubbles, diagrams, and highlights. Brightened for dark —
     these are used as small text (chat sender names), so light-theme
     versions fail contrast. */
  --color-actor-1:        #F0714E;       /* vermillion */
  --color-actor-2:        #56B6D6;       /* teal */
  --color-actor-3:        #A99AD8;       /* plum */
  --color-actor-4:        #E3BC5C;       /* golden */
  --color-actor-5:        #5FC088;       /* forest */
}
```

**Rules:**
- Alternating module backgrounds (`--color-bg` / `--color-bg-warm`) create visual rhythm. This is automatic — `styles.css` alternates them with `.module:nth-of-type(odd|even)`. Never set a `background` on a `.module` in module HTML; an inline background breaks the alternation for every module after it.
- Actor colors should be visually distinct from each other and from the accent
- Code blocks always use `--color-bg-code` with light text — it is the darkest layer, so code recedes into the page rather than floating on it
- Text on a filled accent or actor chip (buttons, step numbers, avatars) is `var(--color-bg)`, not white — dark-on-bright is what reads on a dark theme
- Never introduce a pure white (`#FFF`) or pure black (`#000`) surface; both break the warm dark palette

---

## Typography

```css
:root {
  /* --- FONTS ---
     Display: bold, geometric, personality-driven. NOT Inter/Roboto/Arial.
     Body: readable with character. NOT system fonts.
     Mono: developer-friendly with clear character distinction. */
  --font-display:  'Bricolage Grotesque', Georgia, serif;
  --font-body:     'DM Sans', -apple-system, sans-serif;
  --font-mono:     'JetBrains Mono', 'Fira Code', 'Consolas', monospace;

  /* --- TYPE SCALE (1.25 ratio) --- */
  --text-xs:   0.75rem;    /* 12px — labels, badges */
  --text-sm:   0.875rem;   /* 14px — secondary text, code */
  --text-base: 1rem;       /* 16px — body text */
  --text-lg:   1.125rem;   /* 18px — lead paragraphs */
  --text-xl:   1.25rem;    /* 20px — screen headings */
  --text-2xl:  1.5rem;     /* 24px — sub-module titles */
  --text-3xl:  1.875rem;   /* 30px — module subtitles */
  --text-4xl:  2.25rem;    /* 36px — module titles */
  --text-5xl:  3rem;       /* 48px — hero text */
  --text-6xl:  3.75rem;    /* 60px — module numbers */

  /* --- LINE HEIGHTS --- */
  --leading-tight:  1.15;  /* headings */
  --leading-snug:   1.3;   /* subheadings */
  --leading-normal: 1.6;   /* body text */
  --leading-loose:  1.8;   /* relaxed reading */
}
```

**Google Fonts link (put in `<head>`):**
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,400;12..96,600;12..96,700;12..96,800&family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;0,9..40,700;1,9..40,400;1,9..40,500&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet">
```

**Rules:**
- Module numbers: `--text-6xl`, font-display, weight 800, `--color-accent` with 15% opacity
- Module titles: `--text-4xl`, font-display, weight 700
- Screen headings: `--text-xl` or `--text-2xl`, font-display, weight 600
- Body text: `--text-base` or `--text-lg`, font-body, `--leading-normal`
- Code: `--text-sm`, font-mono
- Labels/badges: `--text-xs`, font-mono, uppercase, letter-spacing 0.05em

---

## Spacing & Layout

```css
:root {
  --space-1:  0.25rem;   /* 4px */
  --space-2:  0.5rem;    /* 8px */
  --space-3:  0.75rem;   /* 12px */
  --space-4:  1rem;      /* 16px */
  --space-5:  1.25rem;   /* 20px */
  --space-6:  1.5rem;    /* 24px */
  --space-8:  2rem;      /* 32px */
  --space-10: 2.5rem;    /* 40px */
  --space-12: 3rem;      /* 48px */
  --space-16: 4rem;      /* 64px */
  --space-20: 5rem;      /* 80px */
  --space-24: 6rem;      /* 96px */

  --content-width:     800px;   /* standard reading width */
  --content-width-wide: 1000px; /* for side-by-side layouts */
  --nav-height:        50px;
  --radius-sm:  8px;
  --radius-md:  12px;
  --radius-lg:  16px;
  --radius-full: 9999px;
}
```

**Module layout:**
```css
.module {
  min-height: 100vh;        /* fallback first... */
  min-height: 100dvh;       /* ...dvh second, so it wins where supported */
  scroll-snap-align: start;
  padding: var(--space-16) var(--space-6);
  padding-top: calc(var(--nav-height) + var(--space-12));
}
/* Alternation is automatic — module HTML sets no background */
.module:nth-of-type(odd)  { background: var(--color-bg); }
.module:nth-of-type(even) { background: var(--color-bg-warm); }

.module-content {
  max-width: var(--content-width);
  margin: 0 auto;
}
```

---

## Shadows & Depth

```css
:root {
  --shadow-sm:  0 1px 2px rgba(0, 0, 0, 0.30);
  --shadow-md:  0 4px 12px rgba(0, 0, 0, 0.38);
  --shadow-lg:  0 8px 24px rgba(0, 0, 0, 0.48);
  --shadow-xl:  0 16px 48px rgba(0, 0, 0, 0.58);
}
```

On a dark page a lightly-tinted shadow is invisible, so these are deep. Elevation is carried as much by surface lightness (`--color-surface` sits above `--color-bg`) as by the shadow itself — use both together.

---

## Animations & Transitions

```css
:root {
  --ease-out:    cubic-bezier(0.16, 1, 0.3, 1);
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
  --duration-fast:   150ms;
  --duration-normal: 300ms;
  --duration-slow:   500ms;
  --stagger-delay:   120ms;
}
```

**Scroll-triggered reveal pattern:**
```css
.animate-in {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity var(--duration-slow) var(--ease-out),
              transform var(--duration-slow) var(--ease-out);
}
.animate-in.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Stagger children */
.stagger-children > .animate-in {
  transition-delay: calc(var(--stagger-index, 0) * var(--stagger-delay));
}
```

**JS setup for stagger:**
```javascript
document.querySelectorAll('.stagger-children').forEach(parent => {
  Array.from(parent.children).forEach((child, i) => {
    child.style.setProperty('--stagger-index', i);
  });
});
```

**Intersection Observer (trigger reveals):**
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      observer.unobserve(entry.target); // animate only once
    }
  });
}, { rootMargin: '0px 0px -10% 0px', threshold: 0.1 });

document.querySelectorAll('.animate-in').forEach(el => observer.observe(el));
```

---

## Navigation & Progress

**HTML structure:**
```html
<nav class="nav">
  <div class="progress-bar" role="progressbar" aria-valuenow="0"></div>
  <div class="nav-inner">
    <span class="nav-title">Course Title</span>
    <div class="nav-dots">
      <button class="nav-dot" data-target="module-1" data-tooltip="Module 1 Name"
              role="tab" aria-label="Module 1"></button>
      <!-- one per module -->
    </div>
  </div>
</nav>
```

**Progress bar (CSS-only where possible, JS fallback):**
```javascript
function updateProgressBar() {
  const scrollTop = window.scrollY;
  const scrollHeight = document.documentElement.scrollHeight - window.innerHeight;
  const progress = (scrollTop / scrollHeight) * 100;
  progressBar.style.width = progress + '%';
}
window.addEventListener('scroll', () => {
  requestAnimationFrame(updateProgressBar);
}, { passive: true });
```

**Nav dot states:**
- Default: `border: 2px solid var(--color-text-muted)`, empty center
- Current: `border-color: var(--color-accent)`, filled center, subtle glow shadow
- Visited: `background: var(--color-accent)`, filled solid

**Keyboard navigation:**
```javascript
document.addEventListener('keydown', (e) => {
  if (['INPUT', 'TEXTAREA'].includes(e.target.tagName)) return;
  if (e.key === 'ArrowDown' || e.key === 'ArrowRight') { nextModule(); e.preventDefault(); }
  if (e.key === 'ArrowUp' || e.key === 'ArrowLeft') { prevModule(); e.preventDefault(); }
});
```

---

## Module Structure

**HTML template for each module:**
```html
<section class="module" id="module-N">
  <div class="module-content">
    <header class="module-header animate-in">
      <span class="module-number">0N</span>
      <h1 class="module-title">Module Title</h1>
      <p class="module-subtitle">One-line description of what this module teaches</p>
    </header>

    <div class="module-body">
      <section class="screen animate-in">
        <h2 class="screen-heading">Screen Title</h2>
        <p>Content...</p>
        <!-- Interactive elements, code translations, etc. -->
      </section>

      <section class="screen animate-in">
        <!-- Next screen -->
      </section>
    </div>
  </div>
</section>
```

---

## Responsive Breakpoints

```css
/* Tablet */
@media (max-width: 768px) {
  :root {
    --text-4xl: 1.875rem;
    --text-5xl: 2.25rem;
    --text-6xl: 3rem;
  }
  .translation-block { grid-template-columns: 1fr; } /* stack code/english */
  .pattern-cards { grid-template-columns: 1fr 1fr; }
}

/* Mobile */
@media (max-width: 480px) {
  :root {
    --text-4xl: 1.5rem;
    --text-5xl: 1.875rem;
    --text-6xl: 2.25rem;
  }
  .module { padding: var(--space-8) var(--space-4); }
  .pattern-cards { grid-template-columns: 1fr; }
  .flow-steps { flex-direction: column; }
  .flow-arrow { transform: rotate(90deg); }
}
```

---

## Scrollbar & Background

```css
/* Custom scrollbar */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb {
  background: var(--color-border);
  border-radius: var(--radius-full);
}

/* Subtle atmospheric background */
body {
  background: var(--color-bg);
  background-image: radial-gradient(
    ellipse at 20% 50%,
    rgba(232, 94, 61, 0.07) 0%,
    transparent 50%
  );
}

/* Page scroll setup */
html {
  scroll-snap-type: y proximity;
  scroll-behavior: smooth;
}
```

---

## Code Block Globals

All code blocks in the course — whether inside translation blocks or standalone snippets — must wrap text and never show a horizontal scrollbar. This is a teaching tool, not an IDE.

```css
pre, code {
  white-space: pre-wrap;       /* wrap long lines */
  word-break: break-word;      /* break mid-word if absolutely needed */
  overflow-x: hidden;          /* no horizontal scrollbar — ever */
}
/* Hide scrollbars on code containers */
.translation-code::-webkit-scrollbar,
pre::-webkit-scrollbar {
  display: none;
}
```

Code snippets must be **exact copies** from the real codebase — never modified, trimmed, or simplified. Instead, choose naturally short (5-10 line) sections from the code that illustrate the concept well. If a longer block is needed, show it all — the wrapping CSS will handle readability.

---

## Syntax Highlighting (Catppuccin-inspired)

For code blocks on the dark `--color-bg-code` background:

```css
.code-keyword  { color: #CBA6F7; }  /* purple — if, else, return, function */
.code-string   { color: #A6E3A1; }  /* green — "strings" */
.code-function { color: #89B4FA; }  /* blue — function names */
.code-comment  { color: #6C7086; }  /* muted gray — // comments */
.code-number   { color: #FAB387; }  /* peach — numbers */
.code-property { color: #F9E2AF; }  /* yellow — object keys */
.code-operator { color: #94E2D5; }  /* teal — =, =>, +, etc. */
.code-tag      { color: #F38BA8; }  /* pink — HTML tags */
.code-attr     { color: #F9E2AF; }  /* yellow — HTML attributes */
.code-value    { color: #A6E3A1; }  /* green — attribute values */
```
