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
| 2 | Sobre mí | ✅ DONE | Center layout. Uses `<GithubProfile />` component for speaker intro. *(Planned as: "¿Quién soy?" — speaker intro with github profile style)* |
| 3 | ¿Qué es la Complejidad Cognitiva? | ✅ DONE | Center layout. Defines CogC as "una métrica que mide qué tan difícil es entender código". Notes difference vs ciclomática: penaliza anidación y quiebres estructurales. |
| 4 | Misma CC, Diferente CogC | ✅ DONE | **NEW — not in original plan.** Two side-by-side `<magic-move>` comparisons. Left: `sum_of_primes` (CC=5, CogC=8). Right: `process_orders` (CC=5, CogC=2). Shows that `if` nesting penalizes heavily while `match/case` doesn't. Key takeaway: "8 vs 2, misma cantidad de caminos, diferencia de 4× en complejidad cognitiva." |
| 5 | ¿Por qué importa? | ✅ DONE | Animated `<GiphyCard>` cards with click animations. Three consequences shown one-by-one then together in compact row: (1) Más bugs, (2) Revisiones más lentas, (3) Onboarding difícil. Conclusion: "Medir la complejidad no es para juzgar, es para mejorar." *(Planned as slide 4: "Impact on code quality")* |
| 6 | Las 3 reglas de la complejidad cognitiva | ✅ DONE | Three click-revealed cards with code snippets: (1) Estructuras de control (+1 each), (2) Anidación (+1 extra per level, with nested `for/if/for` example), (3) Booleanos (+1 per operator, +1 extra for and↔or switches). Plus green "Lo que NO suma" banner: match/case, break, continue, return, with, try, funciones anidadas, comentarios = 0 pts. *(Planned as slide 5: "¿Cómo se mide?" — expanded into two detailed slides)* |
| 7 | La fórmula en acción | ✅ DONE | Two side-by-side Python examples with annotation: `check_nested` (CogC=6) vs `check_flat` (CogC=4). Shows formula: "1 (estructural) + N (anidación) + B (booleanos)". Key takeaway: "6 vs 4, misma lógica, 33% menos complejidad cognitiva." |
| 8 | ~~Introduciendo complexipy~~ | ❌ TODO | *(Planned: Tool intro — not yet created)* |
| 9 | ~~complexipy — Uso básico~~ | ❌ TODO | *(Planned: CLI usage — not yet created)* |
| 10 | ~~Detección en código real (1)~~ | ❌ TODO | *(Planned: Show high-CC code with Monaco — not yet created)* |
| 11 | ~~Detección en código real (2)~~ | ❌ TODO | *(Planned: Problem analysis — not yet created)* |
| 12 | ~~Refactoring en acción~~ | ❌ TODO | *(Planned: Section intro — not yet created)* |
| 13 | ~~Ejemplo 1: Condicionales anidados~~ | ❌ TODO | *(Planned: Magic Move animation — not yet created)* |
| 14 | ~~Ejemplo 2: Loop con lógica compleja~~ | ❌ TODO | *(Planned: Magic Move animation — not yet created)* |
| 15 | ~~Ejemplo 3: Expresiones booleanas complejas~~ | ❌ TODO | *(Planned: Magic Move animation — not yet created)* |
| 16 | ~~Ejemplo 4: Match/Case con lógica anidada~~ | ❌ TODO | *(Planned: Magic Move animation — not yet created)* |
| 17 | ~~Resumen de los 4 ejemplos~~ | ❌ TODO | *(Planned: Stats cards — not yet created)* |
| 18 | ~~Buenas Prácticas~~ | ❌ TODO | *(Planned: Patterns for reducing CC — not yet created)* |
| 19 | ~~Integración con CI/CD~~ | ❌ TODO | *(Planned: GitHub Actions example — not yet created)* |
| 20 | ~~¿Qué más ofrece complexipy?~~ | ❌ TODO | *(Planned: Feature list + links — not yet created)* |
| 21 | ~~Resumen~~ | ❌ TODO | *(Planned: Key takeaways — not yet created)* |
| 22 | ~~¡Gracias!~~ | ❌ TODO | *(Planned: Q&A + links — not yet created)* |

## Summary

- **Done:** 7 / 22 slides
- **Remaining:** 15 slides
- **Progress:** ~32%
- **Current coverage:** Title → Concept → CogC vs CC demo → Why it matters → Rules → Formula in action

## Structural Notes

- Slides 1-3 match the original plan (title, intro, definition).
- Slide 4 (Misma CC, Diferente CogC) is an **addition** not in the original plan — it's a powerful CogC vs CC comparison using two parallel magic-move blocks. Replaces what was planned as a simple "¿Cómo se mide?" slide with a concrete, visual demonstration.
- Slide 5 was originally planned as slide 4; moved down to accommodate the new slide 4.
- Slides 6-7 replace the original single "¿Cómo se mide?" slide (planned as slide 5) with two detailed slides covering all three rules + the formula with worked examples.
- The remaining slides 8-22 follow the original plan order with minor number adjustments.
