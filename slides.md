---
theme: apple-basic
title: Cognitive Complexity en Python
titleTemplate: '%s - Python Cali'
author: Robin Hafid Quintero Lopez
transition: slide-left
layout: intro
fonts:
  sans: 'Space Grotesk'
  mono: 'JetBrains Mono'
  weights: '300,400,500,600,700'
  italic: false
---

# Cognitive Complexity en Python

<div class="title-tick"></div>

<div style="font-size: 1.05rem; color: var(--text-secondary);">
cómo medir código difícil de entender con <span style="color: var(--accent-teal); font-weight: 600;">complexipy</span>
</div>

<div class="absolute bottom-10" style="display: flex; align-items: center; gap: 9px;">
    <span style="width: 7px; height: 7px; border-radius: 50%; background: var(--accent-teal); display: inline-block;"></span>
    <span class="font-700">Python Cali, Meetup Junio</span>
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


<div class="text-center mt-4" style="color: var(--text-muted);">
A diferencia de la complejidad ciclomática, penaliza la <b>anidación</b> y los <b>quiebres estructurales</b>
</div>

<div v-click class="mt-5" style="border-left: 3px solid var(--accent-teal); padding-left: 14px; text-align: left;">
<div style="color: var(--text-secondary); font-size: 0.84rem; line-height: 1.5;">
Su primer principio, <strong style="color: var(--text-primary);">"Ignorar atajos"</strong>: las estructuras que permiten escribir múltiples declaraciones en una sola línea no suman complejidad.
</div>
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

<div v-click class="mt-6" style="text-align: center;">
<div style="color: var(--text-primary); font-size: 0.98rem; font-weight: 500; line-height: 1.45;">
No es una lista de excepciones, es una filosofía:<br/>si puedes leerlo de un vistazo, no es complejo.
</div>
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

def sum_of_primes(max: int) -> int:
    total = 0
    for i in range(1, max + 1):    # +1
        should_add = True
        for j in range(2, i):      # +1
            if i % j == 0:         # +1
                should_add = False
                break
        if should_add:             # +1
            total += i
    return total
```

```python
# CogC = 8

def sum_of_primes(max: int) -> int:
    total = 0
    for i in range(1, max + 1):    # +1 (nivel 0)
        should_add = True
        for j in range(2, i):      # +2 (nivel 1)
            if i % j == 0:         # +3 (nivel 2)
                should_add = False
                break
        if should_add:             # +2 (nivel 1)
            total += i
    return total
```
````

</div>

<div style="flex: 1; min-width: 0;">

````md magic-move
```python
# CC = 5

def process_orders(orders: list) -> int:
    total = 0
    for order in orders:               # +1
        match order.get('status'):
            case 'completed' | 'pending':   # +1
                total += order['amount']
            case 'cancelled' | 'returned':  # +1
                total -= order['amount']
    if total < 0:                      # +1
        total = 0
    return total
```

```python
# CogC = 4

def process_orders(orders: list) -> int:
    total = 0
    for order in orders:               # +1 (nivel 0)
        match order.get('status'):     # +2 (nivel 1)
            case 'completed' | 'pending':
                total += order['amount']
            case 'cancelled' | 'returned':
                total -= order['amount']
    if total < 0:                      # +1 (nivel 0)
        total = 0
    return total
```
````

</div>

</div>

<div v-click class="mt-4">
<CogCVersus
  :a="8" :b="4" mode="vs"
  labelA="if anidados" labelB="match / case"
  note="Misma cantidad de caminos (CC = 5), el doble de complejidad cognitiva. Los if anidados acumulan anidación; match cobra un solo incremento sin importar cuántos casos."
/>
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

<div v-click="4" style="text-align: center;">
<div style="color: var(--text-primary); font-size: 0.96rem; font-weight: 500; line-height: 1.45;">
Medir la complejidad no es para juzgar, es para mejorar.
</div>
<div style="color: var(--text-muted); font-size: 0.8rem; margin-top: 5px;">
Saber <em>qué</em> hace el código difícil de leer nos ayuda a escribir mejor.
</div>
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
<pre style="background: var(--bg-code); border: 1px solid var(--border-dark); border-radius: 6px; padding: 8px 12px; margin-top: 10px; font-family: var(--slidev-font-mono), monospace; font-size: 0.7rem; line-height: 1.6; color: var(--text-secondary);"><span style="color: var(--accent-keyword);">for</span> ...:          <span style="color: var(--text-muted);"># nivel 0 → +1</span>
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

<div v-click style="background: var(--bg-card); border: 1px solid var(--border-green); border-radius: 8px; padding: 14px 16px; margin-bottom: 14px;">
<div style="color: var(--accent-green); font-weight: 600; font-size: 0.82rem; margin-bottom: 5px;">
  Lo que NO suma, "Ignorar atajos"
</div>
<div style="color: var(--text-muted); font-size: 0.76rem; line-height: 1.6;">
  <code style="color: var(--accent-blue);">break</code>, <code style="color: var(--accent-blue);">continue</code>, <code style="color: var(--accent-blue);">return</code>, <code style="color: var(--accent-blue);">with</code>, <code style="color: var(--accent-blue);">try</code> / <code style="color: var(--accent-blue);">finally</code>, extraer funciones, comentarios. Legibles de un vistazo, <strong style="color: var(--text-secondary);">0 puntos</strong>. Ojo: <code style="color: var(--accent-blue);">match</code> sí suma +1.
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
<div style="flex-shrink: 0; width: 28px; height: 28px; border-radius: 6px; background: var(--accent-teal-dim); border: 1px solid var(--accent-teal); display: flex; align-items: center; justify-content: center; font-weight: 600; font-size: 0.8rem; color: var(--accent-teal); font-family: 'JetBrains Mono', monospace;">A</div>
<div>
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.85rem; margin-bottom: 4px;">Estructural</div>
<div style="color: var(--text-muted); font-size: 0.72rem; line-height: 1.5;">
<code style="color: var(--accent-blue);">if</code> <code style="color: var(--accent-blue);">for</code> <code style="color: var(--accent-blue);">while</code> <code style="color: var(--accent-blue);">except</code>, +1 por cada uno. Son los que "abren" el flujo.
</div>
</div>
</div>
<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 14px 16px; display: flex; align-items: flex-start; gap: 12px;">
<div style="flex-shrink: 0; width: 28px; height: 28px; border-radius: 6px; background: var(--accent-teal-dim); border: 1px solid var(--accent-teal); display: flex; align-items: center; justify-content: center; font-weight: 600; font-size: 0.8rem; color: var(--accent-teal); font-family: 'JetBrains Mono', monospace;">B</div>
<div>
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.85rem; margin-bottom: 4px;">Anidación</div>
<div style="color: var(--text-muted); font-size: 0.72rem; line-height: 1.5;">
Cuando A está dentro de otro A, +1 por cada nivel extra. El costo acumulativo.
</div>
</div>
</div>
<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 14px 16px; display: flex; align-items: flex-start; gap: 12px;">
<div style="flex-shrink: 0; width: 28px; height: 28px; border-radius: 6px; background: var(--accent-teal-dim); border: 1px solid var(--accent-teal); display: flex; align-items: center; justify-content: center; font-weight: 600; font-size: 0.8rem; color: var(--accent-teal); font-family: 'JetBrains Mono', monospace;">C</div>
<div>
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.85rem; margin-bottom: 4px;">Fundamental</div>
<div style="color: var(--text-muted); font-size: 0.72rem; line-height: 1.5;">
Secuencias booleanas, recursión, goto. No afectan el anidamiento pero sí la complejidad.
</div>
</div>
</div>
<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 14px 16px; display: flex; align-items: flex-start; gap: 12px;">
<div style="flex-shrink: 0; width: 28px; height: 28px; border-radius: 6px; background: var(--accent-teal-dim); border: 1px solid var(--accent-teal); display: flex; align-items: center; justify-content: center; font-weight: 600; font-size: 0.8rem; color: var(--accent-teal); font-family: 'JetBrains Mono', monospace;">D</div>
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

<div v-click class="mt-4" style="border-left: 3px solid var(--accent-teal); padding-left: 14px; background: none;">
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

<div v-click class="mt-4" style="text-align: center; color: var(--text-secondary); font-size: 0.85rem;">
El <span style="color: var(--accent-teal); font-weight: 600;">switch</span> cobra una vez, sin importar cuántos casos.
</div>

---
layout: default
---

# ¿Por qué penalizar el anidamiento?

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
La intuición detrás de la Regla #2
</div>

<div style="border-left: 3px solid var(--accent-teal); padding-left: 16px; margin-bottom: 16px;">
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
    for i in items:            # +1
        for j in items:        # +2
            if j.valid:        # +3
                if j.active:   # +4
                    use(j)
    if items:                  # +1
        init()
```

</div>
</div>

<div v-click class="mt-4">
<CogCVersus
  :a="11" :b="5" mode="vs"
  labelA="anidado" labelB="lineal"
  note="Cinco caminos en ambos casos. Cada nivel de anidación añade un recargo, y se acumulan: 11 contra 5."
/>
</div>

---
layout: default
---

# La fórmula en acción

<div class="mt-2 mb-4 text-sm" style="color: var(--text-muted);">
Apliquemos la fórmula, línea por línea
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

# Misma lógica.

<div class="title-tick" style="margin: 16px auto 30px;"></div>

<CogCVersus
  :a="6" :b="4" mode="arrow"
  labelA="check_nested" labelB="check_flat"
  big
  note="Una sola línea menos de anidación, un tercio menos de carga cognitiva."
/>

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

<div v-click class="mt-4" style="text-align: center; color: var(--text-secondary); font-size: 0.88rem;">
El decorador es <span style="color: var(--accent-teal); font-weight: 600;">patrón estructural</span>, no lógica. No debería inflar la métrica.
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

<div v-click class="mt-4" style="border-left: 3px solid var(--accent-teal); padding-left: 14px;">
<div style="color: var(--text-secondary); font-size: 0.82rem; line-height: 1.5;">
El mismo <code style="color: var(--accent-blue);">if</code>, el mismo wrapper. El score cambia solo porque una de las dos pierde la excepción del decorador.
</div>
</div>

---
layout: statement
---

# Ya sabemos qué medir.

<div class="title-tick" style="margin: 16px auto 24px;"></div>

<div style="color: var(--text-secondary); font-size: 1.05rem;">
Ahora, ¿cómo lo medimos sin contar a mano?
</div>

---
layout: default
---

# complexipy

<div class="mt-2 mb-6 text-sm" style="color: var(--text-muted);">
Análisis de complejidad cognitiva para Python, escrito en Rust
</div>

<div style="display: flex; flex-direction: column; gap: 11px; margin-bottom: 22px;">

<div style="display: flex; gap: 12px; align-items: flex-start;">
<span style="flex-shrink: 0; width: 8px; height: 8px; border-radius: 2px; background: var(--accent-teal); margin-top: 6px;"></span>
<div style="color: var(--text-secondary); font-size: 0.84rem; line-height: 1.5;">
Implementa el paper de Campbell, los mismos incrementos que acabamos de ver.
</div>
</div>

<div style="display: flex; gap: 12px; align-items: flex-start;">
<span style="flex-shrink: 0; width: 8px; height: 8px; border-radius: 2px; background: var(--accent-teal); margin-top: 6px;"></span>
<div style="color: var(--text-secondary); font-size: 0.84rem; line-height: 1.5;">
El motor está en Rust, analiza miles de funciones en milisegundos.
</div>
</div>

<div style="display: flex; gap: 12px; align-items: flex-start;">
<span style="flex-shrink: 0; width: 8px; height: 8px; border-radius: 2px; background: var(--accent-teal); margin-top: 6px;"></span>
<div style="color: var(--text-secondary); font-size: 0.84rem; line-height: 1.5;">
Un score por función, listo para la terminal, el editor, pre-commit y CI.
</div>
</div>

</div>

```bash
pip install complexipy
# o con uv
uv add complexipy
```

---
layout: default
---

# Uso básico

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
Apunta complexipy a un archivo, una carpeta o un repositorio de git
</div>

<pre style="background: var(--bg-code); border: 1px solid var(--border-dark); border-radius: 8px; padding: 18px 22px; font-family: var(--slidev-font-mono), monospace; font-size: 0.8rem; line-height: 1.6; color: var(--text-secondary); overflow-x: auto;"><span style="color: var(--text-very-muted);">$</span> <span style="color: var(--text-primary);">complexipy .</span>

<span style="color: var(--text-very-muted);">──────────────── 🐙 complexipy ────────────────</span>
orders.py
    is_valid 1  ✅ <span style="color: var(--accent-green);">PASSED</span>
    process_orders 9  ✅ <span style="color: var(--accent-green);">PASSED</span>

<span style="color: var(--text-muted);">All functions are within the allowed complexity.</span>
<span style="color: var(--text-very-muted);">──────────── 🎉 Analysis completed! 🎉 ────────────</span></pre>

<div v-click class="mt-5" style="border-left: 3px solid var(--accent-teal); padding-left: 14px;">
<div style="color: var(--text-secondary); font-size: 0.82rem; line-height: 1.5;">
Sin configuración. El número junto a cada función es su complejidad cognitiva, el mismo que calculamos a mano.
</div>
</div>

---
layout: default
---

# El umbral, tu compuerta en CI

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
Define un máximo por función con <code style="color: var(--accent-blue);">--max-complexity-allowed</code>, por defecto 15
</div>

<pre style="background: var(--bg-code); border: 1px solid var(--border-dark); border-radius: 8px; padding: 16px 22px; font-family: var(--slidev-font-mono), monospace; font-size: 0.78rem; line-height: 1.55; color: var(--text-secondary); overflow-x: auto;"><span style="color: var(--text-very-muted);">$</span> <span style="color: var(--text-primary);">complexipy . --max-complexity-allowed 5</span>

<span style="color: var(--text-very-muted);">──────────────── 🐙 complexipy ────────────────</span>
orders.py
    is_valid 1  ✅ <span style="color: var(--accent-green);">PASSED</span>
    process_orders 9  ❌ <span style="color: var(--accent-red);">FAILED</span>

<span style="color: var(--text-muted);">Failed functions:</span>
 <span style="color: var(--accent-red);">- orders.py: process_orders</span>
<span style="color: var(--text-very-muted);">──────────── 🎉 Analysis completed! 🎉 ────────────</span>

<span style="color: var(--text-very-muted);">$</span> <span style="color: var(--text-primary);">echo $?</span>
<span style="color: var(--accent-red);">1</span></pre>

<div v-click class="mt-4" style="text-align: center; color: var(--text-secondary); font-size: 0.86rem;">
Ese <code style="color: var(--accent-blue);">exit 1</code> es lo que detiene el pipeline cuando una función se vuelve difícil de leer.
</div>

---
layout: default
---

# No solo el número, también el plan

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
<code style="color: var(--accent-blue);">--suggest-refactors</code> propone cómo bajar la complejidad
</div>

<pre style="background: var(--bg-code); border: 1px solid var(--border-dark); border-radius: 8px; padding: 14px 22px; font-family: var(--slidev-font-mono), monospace; font-size: 0.74rem; line-height: 1.5; color: var(--text-secondary); overflow-x: auto;"><span style="color: var(--text-very-muted);">$</span> <span style="color: var(--text-primary);">complexipy . --suggest-refactors -mx 5</span>

<span style="color: var(--text-very-muted);">──────────────── 🐙 complexipy ────────────────</span>
orders.py
    is_valid 1  ✅ <span style="color: var(--accent-green);">PASSED</span>
    process_orders 9  ❌ <span style="color: var(--accent-red);">FAILED</span>

      Refactor plans:
        <span style="color: var(--text-primary);">1.</span> Extract complex block into helper function, lines 7-16
           estimated: 9 -> 4 (<span style="color: var(--accent-green);">-5</span>)
        <span style="color: var(--text-primary);">2.</span> Split conditional dispatcher into handlers, lines 9-16
           estimated: 9 -> 7 (<span style="color: var(--accent-green);">-2</span>)
<span style="color: var(--text-very-muted);">──────────── 🎉 Analysis completed! 🎉 ────────────</span></pre>

<div v-click class="mt-4" style="text-align: center;">
<div style="color: var(--text-primary); font-size: 0.92rem; font-weight: 500; line-height: 1.45;">
Planes deterministas desde el AST en Rust. Sin IA, y nunca reescribe tu código.
</div>
</div>

---
layout: statement
---

# No te dice que tu código es malo.

<div class="title-tick" style="margin: 16px auto 24px;"></div>

<div style="color: var(--text-secondary); font-size: 1.05rem;">
Te dice dónde mirar primero.
</div>

---
layout: default
---

# Seguimiento en el tiempo

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
Adopta complexipy en un proyecto grande sin tener que arreglarlo todo hoy
</div>

<div style="display: flex; flex-direction: column; gap: 9px; margin-bottom: 18px;">

<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 12px 16px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.82rem; margin-bottom: 3px;">Snapshot, <code style="color: var(--accent-blue);">--snapshot-create</code></div>
<div style="color: var(--text-muted); font-size: 0.74rem; line-height: 1.45;">Congela la deuda actual como línea base. Solo falla si algo nuevo la supera.</div>
</div>

<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 12px 16px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.82rem; margin-bottom: 3px;">Diff, <code style="color: var(--accent-blue);">--diff main</code></div>
<div style="color: var(--text-muted); font-size: 0.74rem; line-height: 1.45;">Compara contra cualquier referencia de git y muestra qué mejoró y qué empeoró.</div>
</div>

<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 12px 16px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.82rem; margin-bottom: 3px;">Ratchet, <code style="color: var(--accent-blue);">--diff main --ratchet</code></div>
<div style="color: var(--text-muted); font-size: 0.74rem; line-height: 1.45;">Sobre el diff, solo falla si una función cruza el umbral o empeora estando por encima.</div>
</div>

</div>

<pre v-click style="background: var(--bg-code); border: 1px solid var(--border-dark); border-radius: 8px; padding: 12px 18px; font-family: var(--slidev-font-mono), monospace; font-size: 0.72rem; line-height: 1.6; color: var(--text-secondary); overflow-x: auto;"><span style="color: var(--text-muted);">Complexity diff  (vs HEAD~1)</span>
<span style="color: var(--text-very-muted);">────────────────────────────────────────────────────────</span>
  <span style="color: var(--accent-red);">REGRESSED</span>   api.py::handle_request     12 → 19  (+7)
  <span style="color: var(--accent-green);">IMPROVED</span>    utils.py::flatten_tree     22 → 14  (-8)
  NEW         auth.py::validate_token    17  (new)
<span style="color: var(--text-very-muted);">────────────────────────────────────────────────────────</span>
<span style="color: var(--text-muted);">Net: 1 regressed, 1 improved, 1 new</span></pre>

---
layout: default
---

# Se conecta donde ya trabajas

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
La misma medición, en el editor, en el commit y en el pull request
</div>

<div style="display: flex; gap: 18px;">

<div style="flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 9px;">

<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 11px 15px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.8rem;">Pre-commit</div>
<div style="color: var(--text-muted); font-size: 0.73rem; line-height: 1.4;">Bloquea el commit antes de que la complejidad llegue al repo.</div>
</div>

<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 11px 15px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.8rem;">VS Code</div>
<div style="color: var(--text-muted); font-size: 0.73rem; line-height: 1.4;">Indicadores de complejidad en tiempo real mientras escribes.</div>
</div>

<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 11px 15px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.8rem;">SARIF, GitLab</div>
<div style="color: var(--text-muted); font-size: 0.73rem; line-height: 1.4;">Anotaciones inline en el PR, vía Code Scanning o Code Quality.</div>
</div>

</div>

<div style="flex: 1; min-width: 0;">

```yaml
# GitHub Actions
- uses: rohaquinlop/complexipy-action@v2
  with:
    paths: .
    max_complexity_allowed: 10
    output_format: json
```

<div style="color: var(--text-very-muted); font-size: 0.7rem; margin-top: 10px; line-height: 1.5;">
Formatos de salida: <code style="color: var(--accent-blue);">csv</code>, <code style="color: var(--accent-blue);">json</code>, <code style="color: var(--accent-blue);">gitlab</code>, <code style="color: var(--accent-blue);">sarif</code>.
</div>

</div>

</div>

---
layout: default
---

# La API de Python

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
El mismo motor en Rust, desde tu propio código
</div>

```python
from complexipy import code_complexity, file_complexity

result = file_complexity("app.py")
print(result.complexity)              # score total del archivo

for fn in result.functions:
    print(fn.name, fn.complexity)     # score por función
    for plan in fn.refactor_plans:    # planes de refactor
        print(plan.title)
```

<div v-click class="mt-5" style="border-left: 3px solid var(--accent-teal); padding-left: 14px;">
<div style="color: var(--text-secondary); font-size: 0.82rem; line-height: 1.5;">
Devuelve objetos con el detalle línea por línea, ideal para construir tus propios linters, dashboards o reglas a medida.
</div>
</div>

---
layout: default
---

# Buenas prácticas

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
Lo que de verdad mueve el número es quitar anidación, no acortar el código
</div>

<div style="display: flex; flex-direction: column; gap: 9px; margin-bottom: 16px;">

<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 12px 16px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.82rem; margin-bottom: 3px;">Guard clauses, salir temprano</div>
<div style="color: var(--text-muted); font-size: 0.74rem; line-height: 1.45;">Invierte la condición y retorna pronto. Cada nivel de anidación que eliminas baja el costo de todo lo que tenía dentro.</div>
</div>

<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 12px 16px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.82rem; margin-bottom: 3px;">Comprehensions en vez de loops que solo acumulan</div>
<div style="color: var(--text-muted); font-size: 0.74rem; line-height: 1.45;">Aplanan varios niveles en uno. Los <code style="color: var(--accent-blue);">and</code> cuestan +1 cada uno, sin penalización de anidamiento.</div>
</div>

<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 12px 16px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.82rem; margin-bottom: 3px;"><code style="color: var(--accent-blue);">any()</code> / <code style="color: var(--accent-blue);">all()</code> en vez de bandera y <code style="color: var(--accent-blue);">break</code></div>
<div style="color: var(--text-muted); font-size: 0.74rem; line-height: 1.45;">Expresan "existe" o "para todo" en una línea, sin la variable de estado ni el corte del bucle.</div>
</div>

<div style="background: var(--bg-card); border: 1px solid var(--border-default); border-radius: 8px; padding: 12px 16px;">
<div style="color: var(--text-primary); font-weight: 700; font-size: 0.82rem; margin-bottom: 3px;">Extraer funciones con nombre</div>
<div style="color: var(--text-muted); font-size: 0.74rem; line-height: 1.45;">La anidación interna de cada función se mide aparte, así que partir un bloque grande reparte la carga.</div>
</div>

</div>

<div v-click style="border-left: 3px solid var(--accent-teal); padding-left: 14px;">
<div style="color: var(--text-secondary); font-size: 0.82rem; line-height: 1.5;">
Cuidado, renombrar sub-expresiones mejora la lectura, pero la gran reducción casi siempre viene de quitar anidación, no de renombrar.
</div>
</div>

---
layout: statement
---

# Refactoring en acción.

<div class="title-tick" style="margin: 16px auto 24px;"></div>

<div style="color: var(--text-secondary); font-size: 1.05rem;">
El mismo comportamiento, mucha menos carga cognitiva.
</div>

---
layout: default
---

# Condicionales anidados a guard clauses

<div class="mt-2 mb-4 text-sm" style="color: var(--text-muted);">
Cuatro <code style="color: var(--accent-blue);">if</code> anidados, cada nivel suma su profundidad al costo del siguiente
</div>

````md magic-move {lines: true}
```python {*|3-8}
# Antes
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
# Después
def get_discount(user, cart):
    if user is None or not user.is_active:
        return 0.0
    if cart.total <= 100:
        return 0.0
    return 0.2 if user.is_premium else 0.1
```
````

<div v-click class="mt-4">
<CogCVersus
  :a="11" :b="4" mode="arrow"
  labelA="anidado" labelB="guard clauses"
  note="Quitar la anidación, no acortar el código, es lo que baja la métrica."
/>
</div>

---
layout: default
---

# Acumulación anidada a comprehension

<div class="mt-2 mb-4 text-sm" style="color: var(--text-muted);">
Tres niveles de <code style="color: var(--accent-blue);">if</code> dentro de un <code style="color: var(--accent-blue);">for</code>, solo para llenar una lista
</div>

````md magic-move {lines: true}
```python {*|3-7}
# Antes
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
# Después
def active_premium_emails(users):
    return [
        u.email.lower()
        for u in users
        if u.is_active and u.is_premium and u.email
    ]
```
````

<div v-click class="mt-4">
<CogCVersus
  :a="10" :b="3" mode="arrow"
  labelA="loop anidado" labelB="comprehension"
  note="La comprehension aplana tres niveles a uno. Los and cuestan solo +1 cada uno."
/>
</div>

---
layout: default
---

# Bandera y break a any()

<div class="mt-2 mb-4 text-sm" style="color: var(--text-muted);">
Una variable de estado, un <code style="color: var(--accent-blue);">break</code> y dos niveles, para responder una sola pregunta
</div>

````md magic-move {lines: true}
```python {*|3-8}
# Antes
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
# Después
def has_expired_item(items):
    return any(i.days_left < 0 for i in items if i.status == "active")
```
````

<div v-click class="mt-4">
<CogCVersus
  :a="6" :b="2" mode="arrow"
  labelA="bandera + break" labelB="any()"
  note="any() y all() expresan existe o para todo, sin bandera, sin break, sin anidación."
/>
</div>

---
layout: default
---

# Resumen

<div class="mt-2 mb-5 text-sm" style="color: var(--text-muted);">
Cuatro ideas para llevarte a casa
</div>

<div style="display: flex; flex-direction: column; gap: 11px; margin-bottom: 20px;">

<div style="display: flex; gap: 14px; align-items: baseline;">
<div style="color: var(--accent-teal); font-family: var(--slidev-font-mono), monospace; font-weight: 700; font-size: 0.9rem; min-width: 22px;">01</div>
<div style="color: var(--text-secondary); font-size: 0.92rem; line-height: 1.45;">La complejidad cognitiva mide qué tan difícil es <strong style="color: var(--text-primary);">entender</strong> el código, no cuántos caminos tiene.</div>
</div>

<div style="display: flex; gap: 14px; align-items: baseline;">
<div style="color: var(--accent-teal); font-family: var(--slidev-font-mono), monospace; font-weight: 700; font-size: 0.9rem; min-width: 22px;">02</div>
<div style="color: var(--text-secondary); font-size: 0.92rem; line-height: 1.45;">La <strong style="color: var(--text-primary);">anidación</strong> es lo que más pesa, cada nivel añade un recargo a todo lo que tiene dentro.</div>
</div>

<div style="display: flex; gap: 14px; align-items: baseline;">
<div style="color: var(--accent-teal); font-family: var(--slidev-font-mono), monospace; font-weight: 700; font-size: 0.9rem; min-width: 22px;">03</div>
<div style="color: var(--text-secondary); font-size: 0.92rem; line-height: 1.45;"><strong style="color: var(--text-primary);">complexipy</strong> lo mide por ti, en la terminal, en el editor y en CI.</div>
</div>

<div style="display: flex; gap: 14px; align-items: baseline;">
<div style="color: var(--accent-teal); font-family: var(--slidev-font-mono), monospace; font-weight: 700; font-size: 0.9rem; min-width: 22px;">04</div>
<div style="color: var(--text-secondary); font-size: 0.92rem; line-height: 1.45;">Refactoriza para <strong style="color: var(--text-primary);">quitar anidación</strong>, no solo para acortar.</div>
</div>

</div>

<div v-click style="color: var(--text-primary); font-weight: 500; font-size: 1rem;">
Código simple no es código corto, es código que se entiende a la primera.
</div>

---
layout: statement
---

# Volvamos dos años atrás.

<div class="title-tick" style="margin: 16px auto 24px;"></div>

<div style="color: var(--text-secondary); font-size: 1.05rem;">
Marzo de 2024, la primera vez que hablé de complexipy aquí.
</div>

---
layout: default
---

# El mismo proyecto, dos años después

<div class="mt-2 mb-3 text-sm" style="color: var(--text-muted);">
Descargas semanales en PyPI, desde aquella primera charla hasta hoy
</div>

<DownloadsChart />

<div class="mt-3" style="display: flex; justify-content: center; gap: 40px;">
<div style="text-align: center;">
<div style="font-family: 'JetBrains Mono', monospace; font-size: 1.5rem; font-weight: 600; color: var(--accent-teal);">21×</div>
<div style="color: var(--text-muted); font-size: 0.72rem;">más descargas por semana</div>
</div>
<div style="text-align: center;">
<div style="font-family: 'JetBrains Mono', monospace; font-size: 1.5rem; font-weight: 600; color: var(--text-primary);">2.83M</div>
<div style="color: var(--text-muted); font-size: 0.72rem;">descargas acumuladas</div>
</div>
<div style="text-align: center;">
<div style="font-family: 'JetBrains Mono', monospace; font-size: 1.5rem; font-weight: 600; color: var(--text-primary);">122k</div>
<div style="color: var(--text-muted); font-size: 0.72rem;">por semana hoy</div>
</div>
</div>
