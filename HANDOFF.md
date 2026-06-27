# Handoff — Remaining slides (refactoring Magic Move + closing)

> For the next working session. Picks up after the tool section + "back to the past" growth story were added.
> **Language: Spanish. No emojis. No em dashes / double hyphens in prose.**

## TL;DR

The deck is **26 / 30 slides done**. Four remain, and the most important one is the
**refactoring Magic Move** section, which `CLAUDE.md` calls the *key goal of the talk*
(showing high-complexity code morphing into clean, low-complexity code). All the hard
part, picking examples and verifying their scores, is **already done below**. Your job is
to turn the verified pairs into slides that follow the deck's design system.

## Read these first (mandatory)

1. `CLAUDE.md` — project rules, especially: **validate every CogC score by running `complexipy` before claiming it.**
2. `DESIGN.md` — full token + component reference (colors, card styling, slide-ending variety).
3. `STRUCTURE.md` — current slide inventory and the **ordering note**.
4. Skim the existing refactoring-adjacent slides in `slides.md` to match tone: slide "Misma CC, Diferente CogC" (two side-by-side `magic-move` blocks) and slide "La fórmula en acción".

## Where these slides go (ordering is decided)

The two growth-story slides ("Volvamos dos años atrás." + the `<DownloadsChart>`) are the
**closing crescendo** and sit physically last in `slides.md`. The remaining technical
slides must be **inserted BEFORE them**.

**Insertion anchor** in `slides.md` — insert immediately before this block:

```md
---
layout: statement
---

# Volvamos dos años atrás.
```

Target final order:

```
... tool section (Uso básico … La API de Python)
→ Buenas prácticas              (TODO, slide 25)
→ Refactoring en acción          (TODO, slide 26 — section, then the Magic Move examples)
→ Resumen                        (TODO, slide 27)
→ Volvamos dos años atrás        (DONE, statement)
→ El mismo proyecto, 2 años después  (DONE, chart)
→ ¡Gracias!                      (TODO, slide 30 — goes AFTER the chart)
```

Note: "Gracias" is the only TODO that goes *after* the growth chart. Everything else goes before it.

## Environment facts

- `complexipy` source + CLI: `/Users/rhafid/opensource-projects/complexipy` (currently **v6.0.0**, paper-conformant).
- Run it: `cd /Users/rhafid/opensource-projects/complexipy && uv run complexipy <file> --plain`
  (`--plain` prints `<path> <function> <complexity>`, perfect for confirming a score).
- The user fixed the algorithm in v6.0.0 to match the SonarSource white paper (v1.7).
  Every score below was verified on v6.0.0, **re-verify if you change a single character of the code.**
- Build check after editing: `bun run build` (≈2s; the `@vueuse/core` `#__PURE__` warnings are
  pre-existing and harmless, look for `✓ built`).

---

## Verified example material (the gold, do not re-invent)

These before/after pairs are scored on complexipy 6.0.0. Use the strong trio for the Magic
Move section; use the boolean gotcha in "Buenas prácticas".

### Example A — Condicionales anidados → guard clauses (11 → 4, −64%)

```python
# Antes, CogC = 11
def get_discount(user, cart):
    if user is not None:
        if user.is_active:
            if cart.total > 100:
                if user.is_premium:
                    return 0.2
                else:
                    return 0.1
    return 0.0
```

```python
# Después, CogC = 4
def get_discount(user, cart):
    if user is None or not user.is_active:
        return 0.0
    if cart.total <= 100:
        return 0.0
    return 0.2 if user.is_premium else 0.1
```

Lesson: invertir la condición y salir temprano (guard clauses) elimina la anidación, que es lo que multiplica el costo.

### Example B — Acumulación anidada → comprehension (10 → 3, −70%)

```python
# Antes, CogC = 10
def active_premium_emails(users):
    result = []
    for u in users:
        if u.is_active:
            if u.is_premium:
                if u.email:
                    result.append(u.email.lower())
    return result
```

```python
# Después, CogC = 3
def active_premium_emails(users):
    return [
        u.email.lower()
        for u in users
        if u.is_active and u.is_premium and u.email
    ]
```

Lesson: una comprehension aplana tres niveles de anidación en un solo nivel; los `and` cuestan solo +1 cada uno, sin penalización de anidamiento.

### Example C — Loop con bandera + break → any() (6 → 2, −67%)

```python
# Antes, CogC = 6
def has_expired_item(items):
    found = False
    for item in items:
        if item.status == "active":
            if item.days_left < 0:
                found = True
                break
    return found
```

```python
# Después, CogC = 2
def has_expired_item(items):
    return any(i.days_left < 0 for i in items if i.status == "active")
```

Lesson: `any()` / `all()` expresan "existe / para todo" sin la bandera ni el `break` ni la anidación.

### Example D (gotcha, for "Buenas prácticas") — nombrar sub-expresiones NO baja la CogC

```python
# CogC = 3
def can_checkout(user, cart):
    if user.active and user.verified and not user.banned and cart.items and cart.total > 0 or user.is_admin:
        return True
    return False
```

```python
# CogC = 4  (¡subió!)  -> extraer a variables mejora la lectura pero la métrica cuenta cada secuencia booleana
def can_checkout(user, cart):
    valid_user = user.active and user.verified and not user.banned
    has_items = bool(cart.items) and cart.total > 0
    return (valid_user and has_items) or user.is_admin
```

```python
# CogC = 2  -> sacar el caso admin con un return temprano sí baja la métrica
def can_checkout(user, cart):
    if user.is_admin:
        return True
    return (
        user.active and user.verified and not user.banned
        and bool(cart.items) and cart.total > 0
    )
```

Lesson (honest, and a great talk moment): la complejidad cognitiva mide una cosa concreta.
Refactorizar por legibilidad no siempre baja el número, y eso está bien. La gran reducción
casi siempre viene de **quitar anidación**, no de renombrar.

---

## Per-slide spec

### Slide 25 — "Buenas prácticas"

- `layout: default`. Title `# Buenas prácticas` (no emoji).
- Content: a short vertical list (NOT a 2×2 colored grid) of the patterns that actually move
  the number, each with white/700 heading + muted description:
  - Guard clauses / return temprano, quita anidación
  - Comprehensions en vez de loops que solo acumulan
  - `any()` / `all()` en vez de bandera + `break`
  - Extraer funciones con nombre (la anidación interna se mide aparte)
- Optionally fold in the **Example D gotcha** as the closer / pull-quote: "renombrar mejora la
  lectura, pero la gran reducción viene de quitar anidación." Use a teal left-border pull-quote.
- Keep to ≤3 visual elements.

### Slide 26 — "Refactoring en acción" (the key demo)

This is the heart of the talk. Two ways to structure it; pick one:

**Option 1 (recommended): one section divider + 3 Magic Move slides.**
- A `layout: section` or `layout: statement` divider: "Refactoring en acción".
- Then one slide per example (A, B, C), each a single `magic-move` block that morphs
  Antes → Después, followed by a `<CogCVersus>` reveal on `v-click`.

**Option 2: 3 compact slides, no divider** if you want to keep slide count down.

Magic Move + CogCVersus pattern for each (the deck's signature resolution):

````md
````md magic-move {lines: true}
```python {*|2-9}
# Antes, CogC = 11
def get_discount(user, cart):
    if user is not None:
        if user.is_active:
            if cart.total > 100:
                if user.is_premium:
                    return 0.2
                else:
                    return 0.1
    return 0.0
```

```python
# Después, CogC = 4
def get_discount(user, cart):
    if user is None or not user.is_active:
        return 0.0
    if cart.total <= 100:
        return 0.0
    return 0.2 if user.is_premium else 0.1
```
````

<div v-click class="mt-4">
<CogCVersus :a="11" :b="4" mode="arrow" labelA="anidado" labelB="guard clauses"
  note="Quitar la anidación, no acortar el código, es lo que baja la métrica." />
</div>
````

`<CogCVersus>` numbers auto-color via the severity ramp; the `mode="arrow"` shows the `−%`
pill. Use it for A (11→4) and B (10→3) where the heat contrast is real (red→green). For
C (6→2) it also reads well (mid→low). **Do not** invent extra colored takeaway lines, the
device IS the resolution.

### Slide 27 — "Resumen"

- `layout: default`. 3-4 key takeaways as a clean list (white headings, muted body), e.g.:
  - La complejidad cognitiva mide qué tan difícil es **entender** el código, no cuántos caminos tiene.
  - La anidación es lo que más pesa.
  - complexipy lo mide por ti, en la terminal, el editor y CI.
  - Refactoriza para quitar anidación, no solo para acortar.
- Resolve with a confident white typographic line, not amber.

### Slide 30 — "¡Gracias!" (goes AFTER the chart)

- `layout: end` or `statement`. Thanks + links: GitHub `rohaquinlop/complexipy`,
  PyPI, docs `rohaquinlop.github.io/complexipy`, the speaker's handle (see `<GithubProfile />`).
- Keep it minimal. `title-tick` is appropriate here.

---

## Design guardrails (the easy ones to violate)

- **1 token = 1 purpose.** `--accent-blue` = inline `<code>` only. `--accent-yellow` =
  formulas only (probably none here). `--accent-teal` = signature / pull-quotes / `<CogCVersus>` chrome.
  `--accent-green` = positive/low, `--accent-red` = penalty/high. Severity ramp (`--cogc-*`)
  = `<CogCVersus>` hero numbers ONLY.
- **Card headings** are always `color: var(--text-primary); font-weight: 700`, never colored.
- **No 2×2 colored card grid.** Use vertical lists with badges/dots.
- **Vary slide endings.** Rotate: `<CogCVersus>` device, a white typographic line, a teal
  pull-quote, or no closer. Never end every slide with a centered amber line.
- **Spanish prose:** commas, not em dashes or `--`. CLI flags like `--max-complexity-allowed`
  are fine inside `<code>`/code blocks (they are not prose).
- **No emojis.** When recreating CLI output, drop the octopus/checkmark, use colored
  text (`PASSED` green, `FAILED` red), as the tool slides already do.
- **Magic Move syntax:** open with 4 backticks (` ````md magic-move `), nested ```python blocks inside.
- **Breathing rhythm:** insert a `layout: statement` breather after 3-4 heavy slides.

## Definition of done

- [ ] Slides 25, 26 (+ its Magic Move examples), 27, 30 written and inserted in the right place.
- [ ] Refactoring examples use the verified pairs above; any new code re-scored with `uv run complexipy ... --plain`.
- [ ] `bun run build` shows `✓ built`.
- [ ] `STRUCTURE.md` updated: mark 25/26/27/30 DONE, refresh the Summary counts (→ 30/30).
