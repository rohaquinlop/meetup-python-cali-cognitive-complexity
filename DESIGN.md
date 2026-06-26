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
| `--accent-red` | `#f85149` | Penalty/cost markers (higher complexity patterns) |
| `--accent-green` | `#3fb950` | "No suma" / positive / exception markers |
| `--accent-yellow` | `#d29922` | Formula results, key takeaways |
| `--accent-keyword` | `#ff7b72` | Syntax-highlighted keywords (for, if, while) |
| `--lang-python` | `#3572A5` | Python language badge color |

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

- **Headings**: `color: var(--text-primary); font-weight: 700`
- **Body**: `color: var(--text-secondary); font-size: 0.78rem; line-height: 1.5`
- **Subtitles**: `color: var(--text-muted); font-size: 0.78rem`
- **Code references inline**: `<code style="color: var(--accent-blue);">keyword</code>`
- **Syntax-highlighted keywords** (inside pre/code blocks): `<span style="color: var(--accent-keyword);">for</span>`

---

## Slide Structure Rules

- **Titles**: Use `# Título` Markdown. No emojis. Use commas (`,`) not middle dots (`·`) in prose.
- **Subtitles**: `<div class="mt-2 mb-4 text-sm" style="color: var(--text-muted);">`
- **Callouts/takeaways**: `<div v-click class="mt-3 text-sm" style="color: var(--accent-yellow);">`
- **Formula results**: `color: var(--accent-yellow)`

---

## Component Usage

### `<GiphyCard>`
Props: `image`, `title`, `description`, `compact?` (boolean)

### `<GithubProfile>`
Self-contained, no props. Uses scoped CSS from `styles/github-card.css`.

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
