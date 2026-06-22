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
