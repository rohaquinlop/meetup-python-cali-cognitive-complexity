---
theme: apple-basic
title: Cognitive Complexity en Python
titleTemplate: '%s - Python Cali'
author: Robin Hafid Quintero Lopez
transition: slide-left
layout: intro
---

# Cognitive Complexity en Python

cómo medir código difícil de entender usando complexipy

<div class="absolute bottom-10">
    <span class="font-700">
        Python Cali - Meetup Junio
    </span>
</div>

---
layout: center
---

<GithubProfile />

---
layout: center
---

# ¿Qué es la Complejidad Cognitiva?

<div style="display: flex; align-items: center; gap: 16px; margin-bottom: 20px;">
  <div style="flex-shrink: 0;">
    <img src="/imgs/OIP-394101286.jpg" style="width: 64px; height: 64px; border-radius: 50%; object-fit: cover; border: 2px solid var(--border-default);" />
  </div>
  <div>
    <div style="font-weight: 700; color: var(--text-primary); font-size: 0.85rem;">G. Ann Campbell</div>
    <div style="color: var(--text-muted); font-size: 0.75rem;">Autora, SonarSource (2017)</div>
  </div>
</div>

Una métrica propuesta por **G. Ann Campbell** (SonarSource) en 2017 que mide **qué tan difícil es entender** un fragmento de código


<div class="text-center text-gray-500 mt-4">
A diferencia de la complejidad ciclomática, penaliza la <b>anidación</b> y los <b>quiebres estructurales</b>
</div>

<div v-click class="mt-3 text-sm" style="color: var(--accent-yellow);">
Su primer principio: <strong>"Ignorar atajos"</strong>, las estructuras que permiten escribir múltiples declaraciones en una sola línea no suman complejidad.
</div>

---
layout: default
---

# Ignorar atajos, Principio fundamental

<div class="mt-2 mb-6 text-sm" style="color: var(--text-muted);">
La regla filosófica detrás de lo que NO suma complejidad
</div>

<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 20px 24px; margin-bottom: 20px;">
<div style="color: var(--text-muted); font-style: italic; font-size: 0.9rem; line-height: 1.6;">
"Ignorar las estructuras que permiten resumir múltiples declaraciones en una de forma legible."
</div>
<div style="color: var(--text-very-muted); font-size: 0.72rem; margin-top: 8px;">
G. Ann Campbell, 2017
</div>
</div>

<div style="display: flex; gap: 12px;">
<div style="flex: 1; background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 14px; text-align: center;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.82rem; margin-bottom: 6px;">Extraer en funciones</div>
<div style="color: var(--text-muted); font-size: 0.72rem; line-height: 1.4;">
Resumir lógica en una función con nombre. El paper no incrementa por métodos<br/>→ 0 puntos
</div>
</div>
<div style="flex: 1; background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 14px; text-align: center;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.82rem; margin-bottom: 6px;">Decoradores</div>
<div style="color: var(--text-muted); font-size: 0.72rem; line-height: 1.4;">
El wrapper que solo envuelve es una excepción del paper, Apéndice A<br/>→ 0 puntos
</div>
</div>
<div style="flex: 1; background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 14px; text-align: center;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.82rem; margin-bottom: 6px;">with / try / finally</div>
<div style="color: var(--text-muted); font-size: 0.72rem; line-height: 1.4;">
No rompen el flujo lineal. Los <code style="color: var(--accent-blue);">except</code> sí suman +1<br/>→ 0 puntos
</div>
</div>
</div>

<div v-click class="mt-6 text-sm" style="color: var(--accent-yellow); text-align: center;">
<strong>No es una lista arbitraria de excepciones. Es una filosofía: si puedes leerlo de un vistazo, no es complejo.</strong>
</div>

---
layout: default
---

# Misma CC, Diferente CogC

<div class="mt-2 mb-3 text-sm" style="color: var(--text-muted);">

Ambas tienen <strong>CC = 5</strong>, pero la complejidad cognitiva es completamente distinta

</div>

<div style="display: flex; gap: 16px;">

<div style="flex: 1; min-width: 0;">

````md magic-move
```python
# CC = 5

def sum_of_primes(max: int) -> int: # base = 1
    total = 0
    for i in range(1, max+1):    # +1
        should_add = True
        for j in range(2, i):    # +1
            if i%j == 0:         # +1
                should_add = False
                break
        if should_add:           # +1
            total += i
    return total                 # = 5
```

```python
# CogC = 8

def sum_of_primes(max: int) -> int:
    total = 0
    for i in range(1, max+1):    # +1 (nivel 0)
        should_add = True
        for j in range(2, i):    # +2 (nivel 1)
            if i%j == 0:         # +3 (nivel 2)
                should_add = False
                break
        if should_add:           # +2 (nivel 1)
            total += i
    return total                 # = 8
```
````

</div>

<div style="flex: 1; min-width: 0;">

````md magic-move
```python
# CC = 5

def process_orders(orders: list) -> int: # base = 1
    total = 0
    for order in orders:                        # +1
        match order.get('status'):
            case 'completed' | 'pending':       # +1
                total += order['amount']
            case 'cancelled' | 'returned':      # +1
                total -= order['amount']
            case _:
                pass
    if total < 0:                               # +1
        total = 0
    return total                                # = 5
```

```python
# CogC = 4

def process_orders(orders: list) -> int:
    total = 0
    for order in orders:                    # +1 (nivel 0)
        match order.get('status'):          # +2 (nivel 1)
            case 'completed' | 'pending':
                total += order['amount']
            case 'cancelled' | 'returned':
                total -= order['amount']
            case _:
                pass
    if total < 0:                           # +1 (nivel 0)
        total = 0
    return total                            # = 4
```
````

</div>

</div>

<div v-click class="mt-3 text-sm" style="color: var(--accent-yellow);">

<strong>8 vs 4</strong>, misma cantidad de caminos, el doble de complejidad cognitiva.<br/>
Los <code>if</code> anidados acumulan anidación, <code>match</code> cobra un solo incremento sin importar cuántos casos.

</div>

---
layout: default
---

# ¿Por qué importa?

<div class="mt-2 mb-4 text-sm" style="color: var(--text-muted);">

La complejidad cognitiva tiene consecuencias reales en el día a día

</div>

<!-- Phase 1: Individual centered cards, one per click -->

<div style="position: relative;">

<div v-click="[1, 2]" style="position: absolute; top: 0; left: 50%; transform: translateX(-50%); width: 100%; display: flex; justify-content: center;">
<GiphyCard
  image="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExY3Roa3RvNXA5MGZkb3kxbmZtYzBlMXFqM2d0c216bGRpZzZsNmR3OCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/FaFu1s2hO1xYHdpk6N/giphy.gif"
  title="Más bugs"
  description="Código difícil de razonar = más errores al modificarlo o extenderlo"
/>
</div>

<div v-click="[2, 3]" style="position: absolute; top: 0; left: 50%; transform: translateX(-50%); width: 100%; display: flex; justify-content: center;">
<GiphyCard
  image="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExamh0MnN5aHoyaHY3Mzg3NnF6Y2IxMGJ4Yml1dm9xZWZwdGFjOWh4ciZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/1xkMJIvxeKiDS/giphy.gif"
  title="Revisiones más lentas"
  description="Un PR con CogC alto toma más tiempo de revisar y aprobar"
/>
</div>

<div v-click="[3, 4]" style="position: absolute; top: 0; left: 50%; transform: translateX(-50%); width: 100%; display: flex; justify-content: center;">
<GiphyCard
  image="https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExODZ4ZXBhc2kzam4xaWozN3k0c2h3enI0bnJqOTloNHptYTJsZzliOSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/Ad91OoLyqki6f0ICEe/giphy.gif"
  title="Onboarding difícil"
  description="Nuevos desarrolladores tardan más en entender código complejo"
/>
</div>

</div>

<!-- Phase 2: All three in row + conclusion -->

<div v-click="4" style="display: flex; gap: 14px; margin-bottom: 16px;">

<div style="flex: 1;">
<GiphyCard
  compact
  image="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExY3Roa3RvNXA5MGZkb3kxbmZtYzBlMXFqM2d0c216bGRpZzZsNmR3OCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/FaFu1s2hO1xYHdpk6N/giphy.gif"
  title="Más bugs"
  description="Código difícil de razonar = más errores al modificarlo o extenderlo"
/>
</div>

<div style="flex: 1;">
<GiphyCard
  compact
  image="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExamh0MnN5aHoyaHY3Mzg3NnF6Y2IxMGJ4Yml1dm9xZWZwdGFjOWh4ciZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/1xkMJIvxeKiDS/giphy.gif"
  title="Revisiones más lentas"
  description="Un PR con CogC alto toma más tiempo de revisar y aprobar"
/>
</div>

<div style="flex: 1;">
<GiphyCard
  compact
  image="https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExODZ4ZXBhc2kzam4xaWozN3k0c2h3enI0bnJqOTloNHptYTJsZzliOSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/Ad91OoLyqki6f0ICEe/giphy.gif"
  title="Onboarding difícil"
  description="Nuevos desarrolladores tardan más en entender código complejo"
/>
</div>

</div>

<div v-click="4" class="text-sm" style="color: var(--accent-yellow); text-align: center;">

<strong>Medir la complejidad no es para juzgar, es para mejorar.</strong><br/>
Saber <em>qué</em> hace el código difícil de leer nos ayuda a escribir mejor.

</div>

---
layout: default
---

# Las 3 reglas de la complejidad cognitiva

<div class="mt-2 mb-4 text-sm" style="color: var(--text-muted);">
La complejidad cognitiva se calcula con 3 componentes
</div>

<div style="display: flex; gap: 12px; margin-bottom: 14px;">

<div v-click style="flex: 1; background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 16px 14px; min-height: 140px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.85rem; margin-bottom: 8px;">Regla #1, Estructuras de control</div>
<div style="color: var(--text-secondary); font-size: 0.78rem; line-height: 1.5;">
<code style="color: var(--accent-blue);">if</code> <code style="color: var(--accent-blue);">elif</code> <code style="color: var(--accent-blue);">else</code>, <strong>+1</strong> cada uno<br/>
<code style="color: var(--accent-blue);">for</code> <code style="color: var(--accent-blue);">while</code>, <strong>+1</strong> cada uno<br/>
<code style="color: var(--accent-blue);">except</code>, <strong>+1</strong> por handler
</div>
</div>

<div v-click style="flex: 1; background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 16px 14px; min-height: 140px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.85rem; margin-bottom: 8px;">Regla #2, Anidación</div>
<div style="color: var(--text-secondary); font-size: 0.78rem; line-height: 1.5;">
Cada nivel de anidación<br/>
<strong>+1 extra</strong> por nivel
</div>
<pre style="background: var(--bg-code); border: 1px solid var(--border-dark); border-radius: 6px; padding: 8px 12px; margin-top: 10px; font-family: monospace; font-size: 0.7rem; line-height: 1.6; color: var(--text-secondary);"><span style="color: var(--accent-keyword);">for</span> ...:          <span style="color: var(--text-muted);"># nivel 0 → +1</span>
    <span style="color: var(--accent-keyword);">if</span> ...:       <span style="color: var(--text-muted);"># nivel 1 → +2</span>
        <span style="color: var(--accent-keyword);">for</span> ...:  <span style="color: var(--text-muted);"># nivel 2 → +3</span></pre>
</div>

<div v-click style="flex: 1; background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 16px 14px; min-height: 140px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.85rem; margin-bottom: 8px;">Regla #3, Booleanos</div>
<div style="color: var(--text-secondary); font-size: 0.78rem; line-height: 1.5;">
<code style="color: var(--accent-blue);">and</code> <code style="color: var(--accent-blue);">or</code>, <strong>+1</strong> por secuencia<br/>
Cambiar <code>and</code> ↔ <code>or</code><br/>
<strong>+1 extra</strong> por cambio
</div>
</div>

</div>

<div v-click style="background: var(--bg-card); border: 1px solid var(--border-green); border-radius: 8px; padding: 16px 14px; margin-bottom: 14px;">
<div style="color: var(--accent-green); font-weight: 600; font-size: 0.82rem; margin-bottom: 4px;">
  Lo que NO suma, "Ignorar atajos"
</div>
<div style="color: var(--text-muted); font-size: 0.76rem; line-height: 1.6;">
  <code style="color: var(--accent-blue);">break</code>, <code style="color: var(--accent-blue);">continue</code>, <code style="color: var(--accent-blue);">return</code>, <code style="color: var(--accent-blue);">with</code>, <code style="color: var(--accent-blue);">try</code> / <code style="color: var(--accent-blue);">finally</code>, extraer funciones, comentarios, todos 0 puntos
</div>
<div style="color: var(--text-very-muted); font-size: 0.7rem; margin-top: 6px;">
  ¿Por qué? Porque son legibles de un vistazo, el Principio #1. Ojo: <code style="color: var(--accent-blue);">match</code> sí suma +1.
</div>
</div>

---
layout: default
---

# Los 4 tipos de incremento

<div class="mt-2 mb-6 text-sm" style="color: var(--text-muted);">
Cómo desglosar la fórmula en componentes precisos
</div>

<div style="display: flex; flex-direction: column; gap: 8px;">
<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 14px 16px; display: flex; align-items: flex-start; gap: 12px;">
<div style="flex-shrink: 0; width: 28px; height: 28px; border-radius: 5px; background: var(--border-default); display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 0.8rem; color: var(--text-primary);">A</div>
<div>
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.85rem; margin-bottom: 4px;">Estructural</div>
<div style="color: var(--text-muted); font-size: 0.72rem; line-height: 1.5;">
<code style="color: var(--accent-blue);">if</code> <code style="color: var(--accent-blue);">for</code> <code style="color: var(--accent-blue);">while</code> <code style="color: var(--accent-blue);">except</code>, +1 por cada uno. Son los que "abren" el flujo.
</div>
</div>
</div>
<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 14px 16px; display: flex; align-items: flex-start; gap: 12px;">
<div style="flex-shrink: 0; width: 28px; height: 28px; border-radius: 5px; background: var(--border-default); display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 0.8rem; color: var(--text-primary);">B</div>
<div>
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.85rem; margin-bottom: 4px;">Anidación</div>
<div style="color: var(--text-muted); font-size: 0.72rem; line-height: 1.5;">
Cuando A está dentro de otro A, +1 por cada nivel extra. El costo acumulativo.
</div>
</div>
</div>
<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 14px 16px; display: flex; align-items: flex-start; gap: 12px;">
<div style="flex-shrink: 0; width: 28px; height: 28px; border-radius: 5px; background: var(--border-default); display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 0.8rem; color: var(--text-primary);">C</div>
<div>
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.85rem; margin-bottom: 4px;">Fundamental</div>
<div style="color: var(--text-muted); font-size: 0.72rem; line-height: 1.5;">
Secuencias booleanas, recursión, goto. No afectan el anidamiento pero sí la complejidad.
</div>
</div>
</div>
<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 14px 16px; display: flex; align-items: flex-start; gap: 12px;">
<div style="flex-shrink: 0; width: 28px; height: 28px; border-radius: 5px; background: var(--border-default); display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 0.8rem; color: var(--text-primary);">D</div>
<div>
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.85rem; margin-bottom: 4px;">Híbrido</div>
<div style="color: var(--text-muted); font-size: 0.72rem; line-height: 1.5;">
<code style="color: var(--accent-blue);">else</code> <code style="color: var(--accent-blue);">elif</code>, no pagan anidación, pero sí aumentan el nivel.
</div>
</div>
</div>
</div>

<div v-click class="mt-5 text-sm" style="color: var(--text-very-muted); text-align: center;">
En Python: A = if/for/while/except, B = anidación, C = secuencias booleanas y recursión, D = elif/else. Goto y labels no existen en Python.
</div>

---
layout: default
---

# Secuencias booleanas

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
Operadores iguales se descuentan, diferentes penalizan
</div>

<div style="display: flex; gap: 16px;">
<div style="flex: 1; min-width: 0;">

```python {2}
# Serie homogénea, una sola secuencia
resultado = a and b and c and d
# and, and, and = 1 incremento
# CogC = 1
```

</div>
<div style="flex: 1; min-width: 0;">

```python {2}
# Serie heterogénea, 3 secuencias
resultado = a and b or c and d
# and, or, and = 1 + 1 + 1
# CogC = 3
```

</div>
</div>

<div v-click class="mt-4" style="border-left: 3px solid var(--accent-blue); padding-left: 14px; background: none;">
<div style="color: var(--text-muted); font-size: 0.76rem; line-height: 1.5;">
Cada transición <code style="color: var(--accent-blue);">and</code> ↔ <code style="color: var(--accent-blue);">or</code> suma +1 extra. La idea: mezclar lógica en la misma línea confunde al lector.
</div>
</div>

<div v-click class="mt-3 text-sm" style="color: var(--accent-yellow); text-align: center;">
<strong>B = 1 + (número de transiciones and↔or)</strong>
</div>

---
layout: default
---

# ¿Por qué switch es más barato?

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
Un vistazo vs. múltiples comparaciones
</div>

<div style="display: flex; gap: 16px;">
<div style="flex: 1; min-width: 0; background: var(--bg-card); border: 1px solid var(--border-green); border-radius: 8px; padding: 14px;">
<div style="color: var(--accent-green); font-weight: 700; font-size: 0.8rem; margin-bottom: 8px;">match / case, un solo +1</div>

```python
match status:                   # +1 (un incremento)
    case "active":
        handle_active()
    case "pending":
        handle_pending()
    case "deleted":
        handle_deleted()
```
<div style="color: var(--text-muted); font-size: 0.7rem; margin-top: 6px;">
Un solo incremento para todo el bloque, sin importar cuántos casos.
</div>
</div>
<div style="flex: 1; min-width: 0; background: var(--bg-card); border: 1px solid var(--border-default); border-top: 2px solid var(--accent-red); border-radius: 8px; padding: 14px;">
<div style="color: var(--accent-red); font-weight: 700; font-size: 0.8rem; margin-bottom: 8px;">if / elif, +1 por rama</div>

```python
if status == "active":      # +1
    handle_active()
elif status == "pending":   # +1
    handle_pending()
elif status == "deleted":   # +1
    handle_deleted()
```
<div style="color: var(--text-muted); font-size: 0.7rem; margin-top: 6px;">
Cada rama puede comparar cualquier variable. Más carga cognitiva.
</div>
</div>
</div>

<div v-click class="mt-3 text-sm" style="color: var(--accent-yellow); text-align: center;"><strong>match/case: un solo incremento para todo el bloque. if/elif: +1 por cada rama. El switch cobra una vez, sin importar cuántos casos.</strong></div>

---
layout: default
---

# ¿Por qué penalizar el anidamiento?

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
La intuición detrás de la Regla #2
</div>

<div style="border-left: 3px solid var(--accent-blue); padding-left: 16px; margin-bottom: 16px;">
<div style="color: var(--text-muted); font-style: italic; font-size: 0.85rem; line-height: 1.5;">
"Una serie lineal de cinco estructuras if y for sería más fácil de entender que esas mismas cinco estructuras anidadas, sin importar el número de rutas de ejecución."
</div>
<div style="color: var(--text-very-muted); font-size: 0.7rem; margin-top: 6px;">
G. Ann Campbell
</div>
</div>

<div style="display: flex; gap: 16px;">
<div style="flex: 1; min-width: 0;">

```python
# Lineal, CogC = 5
def process(items):
    for i in items:       # +1
        check(i)
    for j in items:       # +1
        if j.valid:       # +1
            use(j)
    if items:             # +1
        init()
```

</div>
<div style="flex: 1; min-width: 0;">

```python
# Anidado, CogC = 11
def process(items):
    for i in items:       # +1
      → for j in items:   # +2
        → → if j.valid:   # +3
          → → → if j.active:  # +4
                    use(j)
    if items:             # +1
        init()
```

</div>
</div>

<div v-click class="mt-3 text-sm" style="color: var(--accent-yellow); text-align: center;">
<strong>5 caminos en ambos casos. Pero 11 vs 5: el anidamiento multiplica el costo.</strong>
</div>

---
layout: default
---

# La fórmula en acción

<div class="mt-2 mb-4 text-sm" style="color: var(--text-muted);">
Cada estructura suma: 1 + nivel de anidación + operadores booleanos
</div>

<div style="display: flex; gap: 16px;">

<div style="flex: 1; min-width: 0;">

```python
def check_nested(items):       # CogC = 6
    for item in items:         # 1 + 0 = 1
        if item.active:        # 1 + 1 = 2
            if item.age:       # 1 + 2 = 3
                return True
    return False
```

</div>

<div style="flex: 1; min-width: 0;">

```python
def check_flat(items):         # CogC = 4
    for item in items:         # 1 + 0 = 1
        if item.active and item.age:  # 1 + 1 + 1 = 3
            return True
    return False
```

</div>

</div>

<div v-click class="mt-4 text-sm" style="color: var(--accent-yellow); text-align: center;">
<strong>Fórmula:</strong> complejidad = 1 (estructural) + N (anidación) + B (booleanos), por cada estructura
</div>

<div style="color: var(--text-very-muted); font-size: 0.68rem; margin-top: 6px; text-align: center;">
  Los incrementos fundamentales (C) aparecen en secuencias booleanas y en recursión. Goto no existe en Python.
</div>

---
layout: statement
---

# 6 vs 4.

<div class="mt-6 text-2xl" style="color: var(--text-muted);">
Misma lógica. 33% menos complejidad cognitiva.
</div>

---
layout: default
---

# Decoradores Python, La excepción especial

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
Del paper de Campbell: funciones que solo envuelven otra función no suman
</div>

<div style="color: var(--text-muted); font-style: italic; font-size: 0.85rem; line-height: 1.5; margin-bottom: 16px;">
"Para calificar para la excepción, una función puede contener solo una función anidada y una declaración return. El nivel de anidación de la función interna se mantiene en 0."
<span style="color: var(--text-very-muted); font-size: 0.7rem; display: block; margin-top: 4px;">G. Ann Campbell, Apéndice A (Compensating Usages)</span>
</div>

<div v-click class="mt-4" style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 14px;">

```python
def mi_decorador(arg):      # ← complexipy IGNORA esto
    def inner(func):         # ← nesting = 0 (no nivel 1)
        if arg:              # +1
            print("debug")
        return func()
    return inner             # CogC total = 1
```

</div>

<div v-click class="mt-3 text-sm" style="color: var(--accent-yellow); text-align: center;">
<strong>El decorador es patrón estructural, no lógica. No debería inflar la métrica.</strong>
</div>

---
layout: default
---

# Decorador, una excepción estrecha

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
La excepción se definió a propósito de forma estrecha, un statement de más y deja de aplicar
</div>

<div style="display: flex; gap: 16px;">
<div style="flex: 1; min-width: 0; background: var(--bg-card); border: 1px solid var(--border-default); border-top: 2px solid var(--accent-red); border-radius: 8px; padding: 14px;">
<div style="color: var(--accent-red); font-weight: 700; font-size: 0.8rem; margin-bottom: 8px;">No califica, CogC = 2</div>

```python
def log_calls(func):
    contador = 0              # un statement extra
    def wrapper(*args):       # nivel 1 (ya no es decorador)
        if args:              # +2 (1 + nivel 1)
            print("con args")
        return func(*args)
    return wrapper
```

</div>
<div style="flex: 1; min-width: 0; background: var(--bg-card); border: 1px solid var(--border-green); border-radius: 8px; padding: 14px;">
<div style="color: var(--accent-green); font-weight: 700; font-size: 0.8rem; margin-bottom: 8px;">Califica como decorador, CogC = 1</div>

```python
def log_calls(func):
    def wrapper(*args):       # nivel 0 (excepción)
        if args:              # +1 (nivel 0)
            print("con args")
        return func(*args)
    return wrapper
```

</div>
</div>

<div v-click class="mt-4 text-sm" style="color: var(--accent-yellow); text-align: center;">
<strong>1 vs 2: solo una función anidada y un return califican. Un statement extra y el wrapper sube el nivel de anidación.</strong>
</div>
