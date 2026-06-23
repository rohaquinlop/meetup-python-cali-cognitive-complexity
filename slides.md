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

<div class="mt-2 mb-3 text-sm" style="color: #8b949e;">

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

<div v-click class="mt-3 text-sm" style="color: #58a6ff;">

<strong>8 vs 2</strong> &mdash; misma cantidad de caminos, diferencia de 4× en complejidad cognitiva.<br/>
Los <code>if</code> anidados penalizan, <code>match</code> con patrones no tiene costo estructural.

</div>

---
layout: default
---

# ¿Por qué importa?

<div class="mt-2 mb-4 text-sm" style="color: #8b949e;">

La complejidad cognitiva tiene consecuencias reales en el día a día

</div>

<!-- Phase 1: Individual centered cards, one per click -->

<div style="position: relative;">

<div v-click="[1, 2]" style="position: absolute; top: 0; left: 50%; transform: translateX(-50%); width: 100%; display: flex; justify-content: center;">
<div style="background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 18px 24px; text-align: center; max-width: 380px; overflow: hidden;">
<img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExY3Roa3RvNXA5MGZkb3kxbmZtYzBlMXFqM2d0c216bGRpZzZsNmR3OCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/FaFu1s2hO1xYHdpk6N/giphy.gif" style="width: 100%; height: 140px; object-fit: contain; border-radius: 14px; margin-bottom: 12px;" />
<div style="color: #e6edf3; font-weight: 600; font-size: 1.1rem; margin-bottom: 6px;">Más bugs</div>
<div style="color: #8b949e; font-size: 0.78rem; line-height: 1.4;">Código difícil de razonar = más errores al modificarlo o extenderlo</div>
</div>
</div>

<div v-click="[2, 3]" style="position: absolute; top: 0; left: 50%; transform: translateX(-50%); width: 100%; display: flex; justify-content: center;">
<div style="background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 18px 24px; text-align: center; max-width: 380px; overflow: hidden;">
<img src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExamh0MnN5aHoyaHY3Mzg3NnF6Y2IxMGJ4Yml1dm9xZWZwdGFjOWh4ciZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/1xkMJIvxeKiDS/giphy.gif" style="width: 100%; height: 140px; object-fit: contain; border-radius: 14px; margin-bottom: 12px;" />
<div style="color: #e6edf3; font-weight: 600; font-size: 1.1rem; margin-bottom: 6px;">Revisiones más lentas</div>
<div style="color: #8b949e; font-size: 0.78rem; line-height: 1.4;">Un PR con CogC alto toma más tiempo de revisar y aprobar</div>
</div>
</div>

<div v-click="[3, 4]" style="position: absolute; top: 0; left: 50%; transform: translateX(-50%); width: 100%; display: flex; justify-content: center;">
<div style="background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 18px 24px; text-align: center; max-width: 380px; overflow: hidden;">
<img src="https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExODZ4ZXBhc2kzam4xaWozN3k0c2h3enI0bnJqOTloNHptYTJsZzliOSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/Ad91OoLyqki6f0ICEe/giphy.gif" style="width: 100%; height: 140px; object-fit: contain; border-radius: 14px; margin-bottom: 12px;" />
<div style="color: #e6edf3; font-weight: 600; font-size: 1.1rem; margin-bottom: 6px;">Onboarding difícil</div>
<div style="color: #8b949e; font-size: 0.78rem; line-height: 1.4;">Nuevos desarrolladores tardan más en entender código complejo</div>
</div>
</div>

</div>

<!-- Phase 2: All three in row + conclusion -->

<div v-click="4" style="display: flex; gap: 14px; margin-bottom: 16px;">

<div style="flex: 1; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 16px 14px; text-align: center; overflow: hidden;">
<img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExY3Roa3RvNXA5MGZkb3kxbmZtYzBlMXFqM2d0c216bGRpZzZsNmR3OCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/FaFu1s2hO1xYHdpk6N/giphy.gif" style="width: 100%; height: 140px; object-fit: contain; border-radius: 14px; margin-bottom: 10px;" />
<div style="color: #e6edf3; font-weight: 600; font-size: 1rem; margin-bottom: 6px;">Más bugs</div>
<div style="color: #8b949e; font-size: 0.76rem; line-height: 1.4;">Código difícil de razonar = más errores al modificarlo o extenderlo</div>
</div>

<div style="flex: 1; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 16px 14px; text-align: center; overflow: hidden;">
<img src="https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExamh0MnN5aHoyaHY3Mzg3NnF6Y2IxMGJ4Yml1dm9xZWZwdGFjOWh4ciZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/1xkMJIvxeKiDS/giphy.gif" style="width: 100%; height: 140px; object-fit: contain; border-radius: 14px; margin-bottom: 10px;" />
<div style="color: #e6edf3; font-weight: 600; font-size: 1rem; margin-bottom: 6px;">Revisiones más lentas</div>
<div style="color: #8b949e; font-size: 0.76rem; line-height: 1.4;">Un PR con CogC alto toma más tiempo de revisar y aprobar</div>
</div>

<div style="flex: 1; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 16px 14px; text-align: center; overflow: hidden;">
<img src="https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExODZ4ZXBhc2kzam4xaWozN3k0c2h3enI0bnJqOTloNHptYTJsZzliOSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/Ad91OoLyqki6f0ICEe/giphy.gif" style="width: 100%; height: 140px; object-fit: contain; border-radius: 14px; margin-bottom: 10px;" />
<div style="color: #e6edf3; font-weight: 600; font-size: 1rem; margin-bottom: 6px;">Onboarding difícil</div>
<div style="color: #8b949e; font-size: 0.76rem; line-height: 1.4;">Nuevos desarrolladores tardan más en entender código complejo</div>
</div>

</div>

<div v-click="4" class="text-sm" style="color: #58a6ff; text-align: center;">

<strong>Medir la complejidad no es para juzgar — es para mejorar.</strong><br/>
Saber <em>qué</em> hace el código difícil de leer nos ayuda a escribir mejor.

</div>

---
layout: default
---

# ¿Cómo se mide?

<div class="mt-2 mb-4 text-sm" style="color: #8b949e;">

La complejidad cognitiva se calcula analizando el AST del código

</div>

<div style="display: flex; gap: 12px; margin-bottom: 12px;">

<div style="flex: 1; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 16px 14px;">
<div style="color: #f85149; font-weight: 600; font-size: 0.85rem; margin-bottom: 8px;">+ Estructuras de control</div>
<div style="color: #c9d1d9; font-size: 0.78rem; line-height: 1.5;">
<code style="color: #58a6ff;">if</code> <code style="color: #58a6ff;">elif</code> <code style="color: #58a6ff;">else</code> · <strong>+1</strong> cada uno<br/>
<code style="color: #58a6ff;">for</code> <code style="color: #58a6ff;">while</code> · <strong>+1</strong> cada uno<br/>
<code style="color: #58a6ff;">except</code> · <strong>+1</strong> por handler
</div>
</div>

<div style="flex: 1; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 16px 14px;">
<div style="color: #f85149; font-weight: 600; font-size: 0.85rem; margin-bottom: 8px;">+ Anidación</div>
<div style="color: #c9d1d9; font-size: 0.78rem; line-height: 1.5;">
Cada nivel de anidación<br/>
<strong>+1 extra</strong> por nivel<br/><br/>
<span style="color: #8b949e;">nivel 0 → +0<br/>nivel 1 → +1<br/>nivel 2 → +2</span>
</div>
</div>

<div style="flex: 1; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 16px 14px;">
<div style="color: #f85149; font-weight: 600; font-size: 0.85rem; margin-bottom: 8px;">+ Booleanos</div>
<div style="color: #c9d1d9; font-size: 0.78rem; line-height: 1.5;">
<code style="color: #58a6ff;">and</code> <code style="color: #58a6ff;">or</code> · <strong>+1</strong> por operador<br/>
Cambiar <code>and</code> ↔ <code>or</code><br/>
<strong>+1 extra</strong> por cambio
</div>
</div>

</div>

<div v-click style="display: flex; gap: 12px;">

<div style="flex: 1; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 16px 14px;">
<div style="color: #3fb950; font-weight: 600; font-size: 0.85rem; margin-bottom: 8px;">✓ No suma</div>
<div style="color: #c9d1d9; font-size: 0.78rem; line-height: 1.5;">
<code style="color: #58a6ff;">match</code> / <code style="color: #58a6ff;">case</code><br/>
Comentarios y docstrings<br/>
Declaraciones, asignaciones
</div>
</div>

<div style="flex: 1; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 16px 14px;">
<div style="color: #3fb950; font-weight: 600; font-size: 0.85rem; margin-bottom: 8px;">✓ No suma</div>
<div style="color: #c9d1d9; font-size: 0.78rem; line-height: 1.5;">
<code style="color: #58a6ff;">break</code> <code style="color: #58a6ff;">continue</code><br/>
<code style="color: #58a6ff;">return</code> temprano<br/>
<code style="color: #58a6ff;">for...else</code> <code style="color: #58a6ff;">try...else</code>
</div>
</div>

<div style="flex: 1; background: #0d1117; border: 1px solid #30363d; border-radius: 8px; padding: 16px 14px;">
<div style="color: #3fb950; font-weight: 600; font-size: 0.85rem; margin-bottom: 8px;">✓ No suma</div>
<div style="color: #c9d1d9; font-size: 0.78rem; line-height: 1.5;">
<code style="color: #58a6ff;">with</code> (solo nesting)<br/>
<code style="color: #58a6ff;">try</code> (solo nesting)<br/>
Funciones anidadas
</div>
</div>

</div>

<div v-click class="mt-5 text-sm" style="color: #58a6ff; text-align: center;">

<strong>Fórmula:</strong> complejidad = 1 (estructural) + N (nivel de anidación) + B (operadores booleanos)

</div>
