# Design Style Guide

> Cognitive Complexity & `complexipy` — Python Cali Meetup
>
> All agents MUST follow these design rules when creating or editing slides.

## Color Palette

All colors are CSS custom properties defined in `style.css` (project root, auto-loaded globally by Slidev). Never use hardcoded hex values — always reference `var(--token)`.

### Backgrounds
| Token | Value | Usage |
|-------|-------|-------|
| `--bg-card` | `#0d1117` | Card backgrounds |
| `--bg-code` | `#161b22` | Code block backgrounds |

### Borders
| Token | Value | Usage |
|-------|-------|-------|
| `--border-default` | `#30363d` | Standard card borders |
| `--border-dark` | `#21262d` | Darker separators |
| `--border-green` | `#1a4731` | Green accent borders (exceptions/positive) |

### Text
| Token | Value | Usage |
|-------|-------|-------|
| `--text-primary` | `#e6edf3` | Card headings, primary content |
| `--text-secondary` | `#c9d1d9` | Body text, descriptions |
| `--text-muted` | `#8b949e` | Subtitles, secondary info, comments |
| `--text-very-muted` | `#6e7681` | Very low-emphasis text |
| `--text-subtle` | `#484f58` | Extremely subtle text |

### Accents
| Token | Value | Usage |
|-------|-------|-------|
| `--accent-blue` | `#58a6ff` | Inline code references (`<code>`) only |
| `--accent-red` | `#f85149` | Penalty/cost markers + high-complexity hero numbers |
| `--accent-green` | `#3fb950` | "No suma" / positive / exception markers |
| `--accent-yellow` | `#d29922` | **Formulas only** (e.g. `B = 1 + transiciones`). NOT slide takeaways |
| `--accent-keyword` | `#ff7b72` | Syntax-highlighted keywords (for, if, while) |
| `--lang-python` | `#3572A5` | Python language badge color |

### Signature (brand)
| Token | Value | Usage |
|-------|-------|-------|
| `--accent-teal` | `#2dd4bf` | The signature accent: cover mark, `title-tick`, the `vs`/`→` device chrome, insight pull-quotes and transition callouts |
| `--accent-teal-dim` | `#134e4a` | Teal fills (badge backgrounds, the `−%` pill) |

### Complexity severity ramp (hero numbers only)
The deck's thesis made visible: low complexity cools, high complexity burns. Used **only** by the `<CogCVersus>` hero numbers, never on body text.
| Token | Value | Range |
|-------|-------|-------|
| `--cogc-low` | `#3fb950` | CogC ≤ 5 |
| `--cogc-mid` | `#d29922` | CogC 6–10 |
| `--cogc-high` | `#f85149` | CogC ≥ 11 |

---

## Card Styling

All feature/info cards use this base styling:

```css
background: var(--bg-card);
border: 1px solid var(--border-default);
border-radius: 8px;
padding: 16px 14px;
```

### Variants

- **Rule cards** (informational): `color: var(--text-primary); font-weight: 700` for headings
- **Exception cards** (green): `border: 1px solid var(--border-green)` + `color: var(--accent-green)` for headings
- **Code cards**: `background: var(--bg-code)` instead of `var(--bg-card)`

---

## Typography

Fonts are set in the `slides.md` headmatter and loaded from Google Fonts:
```yaml
fonts:
  sans: 'Space Grotesk'   # display + UI, the deck's personality
  mono: 'JetBrains Mono'   # all code + hero numbers
  weights: '300,400,500,600,700'
```
Headings get tightened tracking (`letter-spacing: -0.02em`) via `style.css`. Never hardcode `font-family` except `'JetBrains Mono', monospace` for inline `<pre>` blocks (or use `var(--slidev-font-mono)`).

- **Headings**: Space Grotesk, `color: var(--text-primary); font-weight: 600`
- **Body**: `color: var(--text-secondary); font-size: 0.78rem; line-height: 1.5`
- **Subtitles**: `color: var(--text-muted); font-size: 0.78rem` — keep to one line, cut if the title already says it
- **Code references inline**: `<code style="color: var(--accent-blue);">keyword</code>`
- **Syntax-highlighted keywords** (inside pre/code blocks): `<span style="color: var(--accent-keyword);">for</span>`

---

## Slide Structure Rules

- **Titles**: Use `# Título` Markdown. No emojis. Use commas (`,`) not middle dots (`·`) in prose.
- **Subtitles**: `<div class="mt-2 mb-4 text-sm" style="color: var(--text-muted);">`
- **Formulas**: `color: var(--accent-yellow)` — this is the ONLY place amber is allowed.
- **Vary how slides resolve.** Do NOT end every slide with a centered bold amber line, that uniform beat is what makes a deck read as machine-generated. Rotate between these forms:
  - the `<CogCVersus>` device (comparison slides),
  - a confident white typographic line (`color: var(--text-primary); font-weight: 500`, no accent),
  - a teal left-border pull-quote (`border-left: 3px solid var(--accent-teal)`),
  - a short teal-highlighted inline line,
  - or **no closer at all** when the body already lands the point.

---

## Component Usage

### `<GiphyCard>`
Props: `image`, `title`, `description`, `compact?` (boolean)

### `<GithubProfile>`
Self-contained, no props. Uses scoped CSS from `styles/github-card.css`.

### `<CogCVersus>` (signature device)
The hero number-contrast. Use it to **resolve** comparison slides instead of a colored takeaway line. Numbers auto-color via the severity ramp.

Props: `a` (number), `b` (number), `labelA?`, `labelB?`, `mode?` (`'vs'` for two different programs | `'arrow'` for a refactor reduction, shows a `−%` pill), `note?` (caption), `big?` (hero size, for statement slides).

```html
<CogCVersus :a="11" :b="5" mode="vs" labelA="anidado" labelB="lineal" note="..." />
<CogCVersus :a="6" :b="4" mode="arrow" big labelA="check_nested" labelB="check_flat" />
```

Reserve it for pairs with a real heat contrast (8 vs 4, 11 vs 5, 6 vs 4). For two low values (3 vs 1) the ramp shows no contrast, so use a text callout instead, that variation is intentional.

### `title-tick` (brand mark)
A short teal rule. Add `<div class="title-tick"></div>` right after an `<h1>` on the cover and statement slides. Do **not** put it on every content slide, it is identity, not decoration.

---

## Animation Rules

- **Sequential card reveals**: Use `v-click` ranges `[start, end]` with `position: absolute; top: 0; left: 50%; transform: translateX(-50%)` inside a `position: relative` container so cards overlap at the same spot
- **Final row reveal**: Use bare `v-click` (no range) on the wrapping container
- **Code side-by-side**: Use `display: flex; gap: 16px` with `flex: 1; min-width: 0` children, no v-clicks

---

## Anti-patterns

- ❌ Hardcoded hex colors in any file — use `var(--token)` always
- ❌ Red headings on informational cards — use `var(--text-primary)` bold
- ❌ `v-click` without `position: absolute` for stacked card reveals — they'll stack vertically
- ❌ Dark card wrappers around code blocks — use Slidev's native Shiki styling
- ❌ Emojis in slide content
- ❌ `·` (middle dot) outside GitHub-profile UI elements — use `,` in prose and titles
- ❌ Adding `1 (base)` to cognitive complexity formulas — CogC has no base, only structure + nesting + booleans
- ❌ `—` (em dash) and `--` (double hyphen) — Spanish uses commas, not dashes
- ❌ Ending every slide with a centered bold amber takeaway — vary the resolution (see Slide Structure Rules)
- ❌ Amber (`--accent-yellow`) on anything but a formula
- ❌ Faking nested indentation with `→` characters inside code blocks — use real indentation and let Shiki render it
- ❌ A muted one-line subtitle under every title when the title already carries the point — cut the filler
