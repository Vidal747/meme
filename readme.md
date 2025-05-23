# Taller de Gramáticas – Solución Completa

## Tabla de Contenidos

1. [Parte 1 – Gramática inicial](#parte1)
2. [Parte 2 – Gramática `S → AabB`](#parte2)
3. [Parte 3 – Gramática `S → xS | Sy | xy`](#parte3)
4. [Resumen global](#resumen)
5. [Errores comunes a evitar](#errores)

---

<a name="parte1"></a>

## 1 · Parte 1 – Gramática inicial

### 1.1 Producciones originales

```text
S → aAb | cHB | CH
A → dBH | eeC
B → ff | D
C → gFB | ah
D → i
E → jF
F → dcGGG | cF
G → kF
H → Hlm
```

| Símbolo             | Tipo                                    | Comentario |
| ------------------- | --------------------------------------- | ---------- |
| **No‑terminales**   | `S, A, B, C, D, E, F, G, H`             |            |
| **Terminales**      | `a, b, c, d, e, f, g, h, i, j, k, l, m` |            |
| **Símbolo inicial** | `S`                                     |            |

### 1.2 Análisis de generatividad

* `D ⇒ i`  ✔️
* `B ⇒ ff` o `B ⇒ i`  ✔️
* `C ⇒ ah` ✔️
* `A ⇒ eeC ⇒ eeah` ✔️
* `S ⇒ aAb ⇒ aeeahb` ✔️

Ninguna producción de `F, G, H, E` puede derivar exclusivamente terminales (ciclos o recursión infinita).
**Generativos:** `S, A, B, C, D`.

### 1.3 Símbolos accesibles

Único camino que genera terminales:

```
S → aAb → eeC → ah
```

Accesibles: `S, A, C`.
`B, D` y el resto (`E, F, G, H`) no aparecen en esa rama.

### 1.4 Eliminación de símbolos inútiles

Gramática resultante (solo símbolos útiles):

```text
S → aAb
A → eeC
C → ah
```

### 1.5 Descripción formal del lenguaje

La derivación es determinista:

```
S ⇒ aAb ⇒ a ee C b ⇒ a ee ah b
```

$$
L(G) = \{\;\texttt{aeeahb}\;\}.
$$

Conjunto finito ⇒ **lenguaje regular**.

### 1.6 Conversión rigurosa a CNF

1. **Variables‑terminal:**

```text
A_a → a    A_b → b    A_e → e    A_h → h
```

2. **Binarización:**

```text
S   → A_a S1
S1  → A A_b
A   → A_e A1
A1  → A_e C
C   → A_a A_h
```

La gramática completa cumple CNF (lado derecho = 2 variables **o** 1 terminal‑variable).

### 1.7 Gramática final en CNF

```text
S   → A_a S1
S1  → A A_b
A   → A_e A1
A1  → A_e C
C   → A_a A_h
A_a → a
A_b → b
A_e → e
A_h → h
```

Emplea **5** auxiliares (`S1, A1, A_a, A_b, A_e, A_h`) – el mínimo para respetar CNF.

---

<a name="parte2"></a>

## 2 · Parte 2 – Gramática `S → AabB`

### 2.1 Producciones

```text
S → AabB
A → aA | bA | ε
B → Bab | Bb | ab | b
```

### 2.2 Lenguajes parciales

* **A**: `(a|b)^*` (iteraciones de `aA`/`bA` hasta ε).
* **B**: base `ab` o `b`; cada paso izquierdo‑recursivo agrega otro `ab` o `b` a la derecha ⇒ `(ab|b)^+`.

### 2.3 Lenguaje completo

$$
L(G) = (a|b)^*\;ab\;(ab|b)^+.
$$

Lenguaje **regular** (expresión regular explícita).

### 2.4 Evaluación de afirmaciones

| Ítem | Afirmación                       | Veredicto | Razón                                          |
| ---- | -------------------------------- | --------- | ---------------------------------------------- |
| (a)  | «CFL no regular»                 | ❌         | Tenemos RE ⇒ regular.                          |
| (b)  | `(a + b)* ab (ab + b) (ab + b)*` | ✔️        | Idéntica a la descripción obtenida.            |
| (c)  | «No existe CNF»                  | ❌         | Todo lenguaje regular ⊆ CFL ⇒ siempre hay CNF. |
| (d)  | «Ninguna anterior»               | ❌         | (b) fue verdadera.                             |

**Respuesta correcta:** **(b)**.

---

<a name="parte3"></a>

## 3 · Parte 3 – Gramática `S → xS | Sy | xy`

### 3.1 Producciones

```text
S → xS | Sy | xy
```

### 3.2 Lenguaje generado

Aplicaciones:

```
S ⇒ x^m S ⇒ x^m S y^n ⇒ x^m x y y^n
```

$$
L(G) = x^{+} y^{+} = \{ x^{p} y^{q} \mid p≥1,\; q≥1 \}.
$$

Lenguaje **regular** (RE `x^+ y^+`).

### 3.3 Propiedades de la gramática

* **¿Regular?** No ✖️ (violación de linealidad: `Sy`).
* **¿En CNF?** No ✖️ (derechos con más de un terminal).
* **¿Lenguaje regular?** Sí ✔️.

### 3.4 Evaluación de afirmaciones

| Ítem | Afirmación                | Veredicto | Razón                    |
| ---- | ------------------------- | --------- | ------------------------ |
| (a)  | «La gramática es regular» | ❌         | `Sy` rompe linealidad.   |
| (b)  | «Está en CNF»             | ❌         | Derechas no cumplen CNF. |
| (c)  | «L(G) es regular»         | ✔️        | RE `x^+y^+`.             |
| (d)  | «Ninguna anterior»        | ❌         | (c) fue verdadera.       |

**Respuesta correcta:** **(c)**.

---

<a name="resumen"></a>

## 4 · Resumen global

| # | Gramática | Lenguaje $L(G)$     | ¿Regular?   | ¿CFG? | ¿En CNF?        | RE equivalente      |
| - | --------- | ------------------- | ----------- | ----- | --------------- | ------------------- |
| 1 | Parte 1   | {`aeeahb`}          | ✔️ (finito) | ✔️    | ✔️ (convertida) | literal `aeeahb`    |
| 2 | Parte 2   | `(a+b)^*ab(ab+b)^+` | ✔️          | ✔️    | Existe          | `(a+b)^*ab(ab+b)^+` |
| 3 | Parte 3   | `x^{+}y^{+}`        | ✔️          | ✔️    | ✖️ (original)   | `x^{+}y^{+}`        |

---

<a name="errores"></a>

## 5 · Errores comunes a evitar

1. **Confundir gramática regular con lenguaje regular.** Una CFG no lineal puede generar lenguaje regular.
2. **Omitir símbolos inútiles antes de CNF.** Conduce a ciclos o variables huérfanas.
3. **Olvidar variables‑terminal en CNF.** Todo terminal aislado en producciones binarias viola la forma.
4. **Aplicar el lema de bombeo prematuramente.** Una expresión regular explícita ya prueba regularidad.

---

### 📝 Licencia

Contenido libre para uso académico. Se agradece citar la referencia a ChatGPT como asistente de elaboración.
