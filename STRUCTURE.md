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
| 13 | 6 vs 4. Misma lógica. 33% menos. | ✅ DONE | Breathing statement slide (statement layout). No cards, no code. Lets the audience absorb the formula-in-action takeaway. |
| 14 | Decoradores Python, La excepción especial | ✅ DONE | **Paper integration.** Campbell Appendix A quote about decorator pattern. Code example showing `complexipy` ignores the outer function, sets inner nesting to 0. v-click takeaway: "El decorador es patrón estructural, no lógica." |
| 15 | Decorador, una excepción estrecha | ✅ DONE | **Paper integration.** Side-by-side, same wrapper logic: no califica (un statement extra hace que la función anidada suba el nivel, CogC=2) vs califica como decorador (CogC=1). Mirrors the paper's `a_decorator`/`not_a_decorator` examples (Apéndice A). Both values verified on complexipy 5.6.1. |
| 16 | ~~Introduciendo complexipy~~ | ❌ TODO | *(Planned: Tool intro — not yet created)* |
| 17 | ~~complexipy — Uso básico~~ | ❌ TODO | *(Planned: CLI usage — not yet created)* |
| 18 | ~~Detección en código real (1)~~ | ❌ TODO | *(Planned: Show high-CC code with Monaco — not yet created)* |
| 19 | ~~Detección en código real (2)~~ | ❌ TODO | *(Planned: Problem analysis — not yet created)* |
| 20 | ~~Refactoring en acción~~ | ❌ TODO | *(Planned: Section intro — not yet created)* |
| 21 | ~~Ejemplo 1: Condicionales anidados~~ | ❌ TODO | *(Planned: Magic Move animation — not yet created)* |
| 22 | ~~Ejemplo 2: Loop con lógica compleja~~ | ❌ TODO | *(Planned: Magic Move animation — not yet created)* |
| 23 | ~~Ejemplo 3: Expresiones booleanas complejas~~ | ❌ TODO | *(Planned: Magic Move animation — not yet created)* |
| 24 | ~~Ejemplo 4: Match/Case con lógica anidada~~ | ❌ TODO | *(Planned: Magic Move animation — not yet created)* |
| 25 | ~~Resumen de los 4 ejemplos~~ | ❌ TODO | *(Planned: Stats cards — not yet created)* |
| 26 | ~~Buenas Prácticas~~ | ❌ TODO | *(Planned: Patterns for reducing CC — not yet created)* |
| 27 | ~~Integración con CI/CD~~ | ❌ TODO | *(Planned: GitHub Actions example — not yet created)* |
| 28 | ~~¿Qué más ofrece complexipy?~~ | ❌ TODO | *(Planned: Feature list + links — not yet created)* |
| 29 | ~~Resumen~~ | ❌ TODO | *(Planned: Key takeaways — not yet created)* |
| 30 | ~~¡Gracias!~~ | ❌ TODO | *(Planned: Q&A + links — not yet created)* |

## Summary

- **Done:** 15 / 30 slides
- **Remaining:** 15 slides
- **Progress:** 50%
- **Current coverage:** Title → Concept → Ignorar atajos → CogC vs CC demo → Why it matters → Rules → 4 types of increment → Boolean sequences → Why switch is cheaper → Why penalize nesting → Formula in action → Breathing statement → Decorator exception

## Structural Notes

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
