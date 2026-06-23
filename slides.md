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

Una métrica que mide **qué tan difícil es entender** un fragmento de código


<div class="text-center text-gray-500 mt-4">
A diferencia de la complejidad ciclomática, penaliza la <b>anidación</b> y los <b>quiebres estructurales</b>
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
# CogC = 2

def process_orders(orders: list) -> int:
    total = 0
    for order in orders:                    # +1 (nivel 0)
        match order.get('status'):          # +0
            case 'completed' | 'pending':   # +0
                total += order['amount']
            case 'cancelled' | 'returned':  # +0
                total -= order['amount']
            case _:                         # +0
                pass
    if total < 0:                           # +1 (nivel 0)
        total = 0
    return total                            # = 2
```
````

</div>

</div>

<div v-click class="mt-3 text-sm" style="color: var(--accent-blue);">

<strong>8 vs 2</strong> &mdash; misma cantidad de caminos, diferencia de 4× en complejidad cognitiva.<br/>
Los <code>if</code> anidados penalizan, <code>match</code> con patrones no tiene costo estructural.

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

<div v-click="4" class="text-sm" style="color: var(--accent-blue); text-align: center;">

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
<code style="color: var(--accent-blue);">if</code> <code style="color: var(--accent-blue);">elif</code> <code style="color: var(--accent-blue);">else</code> · <strong>+1</strong> cada uno<br/>
<code style="color: var(--accent-blue);">for</code> <code style="color: var(--accent-blue);">while</code> · <strong>+1</strong> cada uno<br/>
<code style="color: var(--accent-blue);">except</code> · <strong>+1</strong> por handler
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
<code style="color: var(--accent-blue);">and</code> <code style="color: var(--accent-blue);">or</code> · <strong>+1</strong> por operador<br/>
Cambiar <code>and</code> ↔ <code>or</code><br/>
<strong>+1 extra</strong> por cambio
</div>
</div>

</div>

<div v-click style="background: var(--bg-card); border: 1px solid var(--border-green); border-radius: 8px; padding: 16px 14px; margin-bottom: 14px;">
<div style="color: var(--accent-green); font-weight: 600; font-size: 0.82rem; margin-bottom: 4px;">✓ Lo que NO suma complejidad</div>
<div style="color: var(--text-muted); font-size: 0.76rem;">
<code style="color: var(--accent-blue);">match/case</code> · <code style="color: var(--accent-blue);">break</code> · <code style="color: var(--accent-blue);">continue</code> · <code style="color: var(--accent-blue);">return</code> · <code style="color: var(--accent-blue);">with</code> · <code style="color: var(--accent-blue);">try</code> · funciones anidadas · comentarios → 0 puntos de complejidad
</div>
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

<div v-click class="mt-3 text-sm" style="color: var(--text-muted); text-align: center;">
6 vs 4, misma lógica, 33% menos complejidad cognitiva. El <code>and</code> aplana la anidación y reduce el costo.
</div>
