# Presentation Structure

> Cognitive Complexity en Python: cómo medir código difícil de entender usando complexipy

## Decisions

- **Duration:** 45 min
- **Audience:** Beginner/Intermediate
- **Demo:** No live demo — screenshots/code only
- **Examples:** 3-4 Magic Move refactoring animations
- **Style:** Dark, code-focused
- **Language:** Spanish

## Slide Outline

| # | Slide | Status | Notes |
|---|---|---|---|
| 1 | Cognitive Complexity en Python | ✅ DONE | Cover slide (intro layout). Title + subtitle: "cómo medir código difícil de entender usando complexipy". Python Cali Meetup Junio badge. |
| 2 | Sobre mí | ✅ DONE | Center layout. Uses `<GithubProfile />` component for speaker intro. |
| 3 | ¿Qué es la Complejidad Cognitiva? | ✅ DONE | Center layout. Defines CogC with Campbell portrait + attribution (SonarSource, 2017). Key definition + contrast vs ciclomática. v-click reveals "Ignorar atajos" principle teaser. |
| 4 | Ignorar atajos, Principio fundamental | ✅ DONE | **Paper integration.** Default layout. Campbell blockquote: "Ignore structures that allow multiple statements to be readably shorthanded into one." Three cards: extraer en funciones, decoradores (Apéndice A), with/try/finally — all 0 pts. (`match` removed: it costs +1, not a shortcut.) v-click conclusion about philosophy behind the exceptions. |
| 5 | Misma CC, Diferente CogC | ✅ DONE | Two side-by-side `<magic-move>` comparisons. Left: `sum_of_primes` (CC=5, CogC=8). Right: `process_orders` (CC=5, CogC=4 under paper rules: `match` = 1+nesting). Shows `if` nesting accumulates while `match` pays a single increment. Key takeaway: "8 vs 4, misma cantidad de caminos, el doble de complejidad." |
| 6 | ¿Por qué importa? | ✅ DONE | Animated `<GiphyCard>` cards with click animations. Three consequences shown one-by-one then together in compact row: (1) Más bugs, (2) Revisiones más lentas, (3) Onboarding difícil. Conclusion: "Medir la complejidad no es para juzgar, es para mejorar." |
| 7 | Las 3 reglas de la complejidad cognitiva | ✅ DONE | Three click-revealed cards: (1) Estructuras de control (+1 each), (2) Anidación (+1 extra per level, with nested `for/if/for` example), (3) Booleanos (+1 **per sequence** of like operators, +1 extra for and↔or switches). Plus green "Lo que NO suma" banner: break, continue, return, with, try/finally, extraer funciones, comentarios = 0 pts (match removed: it costs +1). |
| 8 | Los 4 tipos de incremento | ✅ DONE | **Paper integration.** Four labeled cards (A-D): Estructural (if/for/while/except), Anidación (nested levels), Fundamental (boolean sequences, recursion, goto), Híbrido (else/elif — no nesting cost but raise level). Footer note about Python-specifics. |
| 9 | Secuencias booleanas | ✅ DONE | **Paper integration.** Two side-by-side code blocks in assignment form (so the printed CogC equals the boolean cost, no `if` +1 muddying it): homogeneous `resultado = a and b and c and d` → 1 vs heterogeneous `a and b or c and d` → 3. Both verified on complexipy 5.6.1. Left-border pull-quote about transitions confusing readers. Formula: "B = 1 + (número de transiciones and↔or)". |
| 10 | ¿Por qué switch es más barato? | ✅ DONE | **Paper integration.** Two-column comparison: match/case (green border, **un solo +1** para todo el bloque) vs if/elif (red border, +1 per branch = 3). Matches the paper's `getWords` switch = 1. The point: switch pays one increment regardless of how many cases. |
| 11 | ¿Por qué penalizar el anidamiento? | ✅ DONE | **Paper integration.** Campbell quote about linear vs nested being easier regardless of execution paths. Two code blocks: linear (CogC=5) vs nested (CogC=11) with same 5 execution paths. Key: "el anidamiento multiplica el costo." |
| 12 | La fórmula en acción | ✅ DONE | Two side-by-side examples: `check_nested` (CogC=6) vs `check_flat` (CogC=4). Annotated with per-line breakdowns. Formula: "1 (estructural) + N (anidación) + B (booleanos)". Footer note about fundamental increments (C). |
| 13 | Misma lógica. (6 → 4) | ✅ DONE | Breathing statement slide (statement layout). Hero `<CogCVersus :a="6" :b="4" big>` device with the `−33%` pill, plus the `title-tick` brand mark. The signature visual moment of the deck. |
| 14 | Decoradores Python, La excepción especial | ✅ DONE | **Paper integration.** Campbell Appendix A quote about decorator pattern. Code example showing `complexipy` ignores the outer function, sets inner nesting to 0. v-click takeaway: "El decorador es patrón estructural, no lógica." |
| 15 | Decorador, una excepción estrecha | ✅ DONE | **Paper integration.** Side-by-side, same wrapper logic: no califica (un statement extra hace que la función anidada suba el nivel, CogC=2) vs califica como decorador (CogC=1). Mirrors the paper's `a_decorator`/`not_a_decorator` examples (Apéndice A). Both values verified on complexipy 5.6.1. |
| 16 | Ya sabemos qué medir. | ✅ DONE | Breathing statement. Bridge from the algorithm to the tool: "¿cómo lo medimos sin contar a mano?" `title-tick`. |
| 17 | complexipy | ✅ DONE | **Tool intro.** Default layout. Subtitle "análisis de complejidad cognitiva para Python, escrito en Rust". Three teal-dot points (implementa el paper de Campbell, motor en Rust, score por función listo para CI). Install code block (`pip install` / `uv add`). No closer (install lands it). |
| 18 | Uso básico | ✅ DONE | **CLI usage.** Recreated terminal (`bg-code` `<pre>`, no emojis): `complexipy .` → `is_valid 1 PASSED`, `process_orders 9 PASSED` (real output, verified on complexipy 6.0.0). Teal pull-quote closer. |
| 19 | El umbral, tu compuerta en CI | ✅ DONE | **CI gate.** `--max-complexity-allowed 5` → `process_orders 9 FAILED` (red), `Failed functions`, `echo $?` → `1`. Default threshold 15. White-text closer about `exit 1` stopping the pipeline. |
| 20 | No solo el número, también el plan | ✅ DONE | **Refactor plans.** `--suggest-refactors -mx 5` → real deterministic plan output (Extract helper 9→3, Flatten guard clauses 9→7). Closer: "Planes deterministas desde el AST en Rust. Sin IA, y nunca reescribe tu código." |
| 21 | No te dice que tu código es malo. | ✅ DONE | Breathing statement. "Te dice dónde mirar primero." `title-tick`. |
| 22 | Seguimiento en el tiempo | ✅ DONE | **Tracking.** Vertical list (no grid): Snapshot (`--snapshot-create`), Diff (`--diff main`), Ratchet (`--ratchet`). v-click reveals a recreated diff sample (REGRESSED red / IMPROVED green / NEW). |
| 23 | Se conecta donde ya trabajas | ✅ DONE | **Integrations.** Two-column: left list (Pre-commit, VS Code, SARIF/GitLab) + right GitHub Action YAML (`complexipy-action@v2`) and output formats (csv/json/gitlab/sarif). |
| 24 | La API de Python | ✅ DONE | **Python API.** `code_complexity` / `file_complexity`, iterate `result.functions`, `fn.refactor_plans`. Teal pull-quote: build your own linters/dashboards. |
| 25 | Buenas prácticas | ✅ DONE | Default layout. Vertical list (no grid) of the four patterns that move the number: guard clauses, comprehensions, `any()`/`all()` vs bandera+break, extraer funciones con nombre. Teal pull-quote closer (the Example D gotcha): renombrar mejora la lectura, pero la gran reducción viene de quitar anidación. |
| 26 | Refactoring en acción. | ✅ DONE | Breathing statement / section divider. "El mismo comportamiento, mucha menos carga cognitiva." `title-tick`. Heads the talk's key Magic Move demo trio (26a-c). |
| 26a | Condicionales anidados a guard clauses | ✅ DONE | **Key demo.** Magic Move `get_discount` Antes→Después, `<CogCVersus :a="11" :b="4" mode="arrow">`. Verified 11→4 on complexipy 6.0.0. |
| 26b | Acumulación anidada a comprehension | ✅ DONE | **Key demo.** Magic Move `active_premium_emails`, `<CogCVersus :a="10" :b="3" mode="arrow">`. Verified 10→3 on complexipy 6.0.0. |
| 26c | Bandera y break a any() | ✅ DONE | **Key demo.** Magic Move `has_expired_item`, `<CogCVersus :a="6" :b="2" mode="arrow">`. Verified 6→2 on complexipy 6.0.0. |
| 27 | Resumen | ✅ DONE | Default layout. Four numbered takeaways (teal `01-04` badges, white key terms): mide entendimiento, anidación es lo que más pesa, complexipy lo mide por ti, refactoriza para quitar anidación. White typographic closer: "Código simple no es código corto, es código que se entiende a la primera." |
| 28 | Volvamos dos años atrás. | ✅ DONE | Breathing statement. Bridge into the closing growth story: "Marzo de 2024, la primera vez que hablé de complexipy aquí." `title-tick`. |
| 29 | El mismo proyecto, dos años después | ✅ DONE | **Back to the past (closing crescendo).** `<DownloadsChart>` (custom SVG, 123 weekly PyPI points) annotated Marzo 2024 (5.8k/sem) → Junio 2026 (122k/sem). Stat trio: 21×, 2.83M acumuladas, 122k/sem. |
| 30 | ¡Gracias! | ✅ DONE | **Closing.** `layout: center`. `title-tick`, one-line thesis recap, mono link grid (GitHub `rohaquinlop/complexipy`, docs `rohaquinlop.github.io/complexipy`, `pip install complexipy`) in teal, speaker line "Robin Quintero, rohaquinlop". |

> **Ordering note:** the two growth-story slides (28-29) currently sit **physically last** in `slides.md`. They are intentionally the closing crescendo, right before Gracias. When the remaining technical TODO slides (buenas prácticas, refactoring Magic Move, resumen) are built, **insert them BEFORE the "Volvamos dos años atrás" statement**, not after it. Gracias goes after the growth chart.

## Summary

- **Done:** 32 / 32 slides
- **Remaining:** none, deck complete
- **Progress:** 100%
- **Current coverage:** Title → Concept → Ignorar atajos → CogC vs CC demo → Why it matters → Rules → 4 types of increment → Boolean sequences → Why switch is cheaper → Why penalize nesting → Formula in action → Breathing statement → Decorator exception → **Tool: intro + install → CLI basics → CI gate → refactor plans → tracking (snapshot/diff/ratchet) → integrations → Python API → buenas prácticas → refactoring en acción (Magic Move trio: 11→4, 10→3, 6→2) → resumen → "back to the past" downloads growth**

## complexipy facts (verified, v6.0.0)

- `process_orders` example (nested `if`/`elif` + boolean) = **9**; `is_valid` (`and`) = **1**. Boolean `a and b or c` = **3**. All verified by running `complexipy` 6.0.0.
- v6.0.0 brought the algorithm into **full SonarSource white-paper (v1.7) conformance** (the user's fix): `match` now `1 + nesting`, `except` now `1 + nesting`, `with`/`try`/`finally` no longer over-nest, direct recursion `+1`, comprehensions/lambdas/nested-ternaries counted. The deck's paper-aligned scores match this release.
- Download data: `complexipy-downloads-overtime.csv`, 123 weekly PyPI points (2024-02-11 → 2026-06-14), embedded in `components/DownloadsChart.vue`. Anchor week of the March 2024 talk = 2024-03-17 (5,814/wk). Total downloads = **2,826,553** (authoritative ClickHouse count; the weekly-CSV sum of 2,756,717 runs ~70k low, so the slide shows 2.83M).

## Structural Notes

### Visual Identity (personality pass)

The deck was given a distinct visual identity to avoid the generic "AI template" feel:

- **Type:** Space Grotesk (display/UI) + JetBrains Mono (code + hero numbers), set in the `slides.md` headmatter `fonts` block.
- **Signature accent:** teal `--accent-teal` (#2dd4bf) for brand chrome, the `title-tick` mark, the `<CogCVersus>` device, and insight/transition callouts.
- **Severity ramp:** the thesis made visual, low complexity cools (green), high burns (red). Used only on `<CogCVersus>` hero numbers via `--cogc-low/mid/high`.
- **`<CogCVersus>` component:** the number contrast is now the hero device on slides 5 (8 vs 4), 11 (11 vs 5) and 13 (6 → 4), replacing the repeated amber takeaway lines.
- **Rhythm:** the uniform "centered bold amber takeaway on every slide" was broken up, amber is now reserved for formulas only (slides 9, 12); other slides resolve with the device, a white typographic line, or a teal pull-quote.
- **Craft fixes:** slide 11 nested code now uses real indentation (was faked with `→`); slide 5 columns equalised and `base = 1` noise removed; slide 7 banner de-densified; slide 8 badges adopt the teal signature.

See `DESIGN.md` for the full token + component reference.

### Accuracy Audit (paper-aligned, not 5.6.1)

All scores were validated against (a) Campbell's white paper v1.7 and (b) the `complexipy`
Rust source. **The slides target paper-correct values**, which is what the upcoming
complexipy release will compute. The currently-installed 5.6.1 still *deviates* from the
paper, so a couple of on-screen numbers intentionally differ from what 5.6.1 prints today:

- **`match`/`switch` = `1 + nesting`** (paper) — 5.6.1 scores it `0`. → slide 5 `process_orders`
  is **4** (not 2); slide 10 `match` is **un solo +1** (not 0); `match` removed from the
  "ignore shortcuts" lists (slides 4, 7).
- **Recursion = +1** (paper) — 5.6.1 scores it `0`. → slides 8 and 12 footers updated.
- **`with`/`try`/`finally` do not raise nesting; `except` = `1 + nesting`** (paper) — 5.6.1
  over-nests them. → slide 4 card reworded ("except sí suma +1").
- **Booleans = +1 per *sequence*** of like operators (not per operator). → slide 7 Regla #3 fixed.
- **Slide 9** uses assignment form so the printed CogC equals the pure boolean cost (1 / 3).
- **Slide 15** rebuilt: the old example had no control flow (scored 0 both ways). Now mirrors
  the paper's `a_decorator` (1) vs `not_a_decorator` (2). Decorator-exception slides (14, 15)
  confirmed against paper Appendix A "Compensating Usages → Python: Decorators" (added v1.3).

Decorator handling and boolean-sequence counting are *already* paper-correct in 5.6.1, so
slides 9, 14, 15 were verified by running `complexipy` directly.

### Paper-Integration Slides (7 new from Campbell's 2017 paper)

Slides 4, 8, 9, 10, 11, 14, 15 were added by directly translating concepts from G. Ann Campbell's original Cognitive Complexity paper into slide format:

- **Slide 4** (Ignorar atajos): The foundational "Ignore shortcuts" principle with Campbell's original quote and three categorized exceptions.
- **Slide 8** (Los 4 tipos de incremento): The paper's four-component framework (A: Structural, B: Nesting, C: Fundamental, D: Hybrid) presented as labeled cards.
- **Slide 9** (Secuencias booleanas): Homogeneous vs heterogeneous boolean series with the formula "B = 1 + transitions".
- **Slide 10** (¿Por qué switch es más barato?): The paper's rationale for why switch/match/case doesn't incur structural cost.
- **Slide 11** (¿Por qué penalizar el anidamiento?): Campbell's key insight that nesting multiplies cognitive load regardless of execution paths, with a linear vs nested comparison.
- **Slide 14** (Decoradores Python): The paper's Appendix A decorator exception — nesting level reset to 0 for decorator patterns.
- **Slide 15** (Decorador, Sin penalización): Side-by-side demonstrating the decorator exception in practice (CogC=3 vs CogC=1).

### Other Structural Changes

- **Slide 5** (Misma CC, Diferente CogC): Originally planned as a simple "¿Cómo se mide?" slide. Replaced with a powerful CogC vs CC comparison using two parallel magic-move blocks.
- **Slide 7** (Las 3 reglas): Expanded from the original single "¿Cómo se mide?" slide (planned as slide 5) into a detailed breakdown of all three rules plus the "what doesn't count" list.
- **Slide 13** (Breathing statement): New structural addition — a minimal statement slide after the formula-in-action slide to let the audience absorb the key insight before moving to decorators.
- **Slides 12-13** (Formula + breathing): The original "La fórmula en acción" slide was split — the formula content stays as slide 12, and a breathing statement "6 vs 4. Misma lógica. 33% menos." was added as slide 13 for pacing.
