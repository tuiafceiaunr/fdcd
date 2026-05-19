---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Métodos Avanzados: medidas de similaridad y distancia - Resolución

### Ejercicio N°1

#### MARIANA vs. MERIANNA

- $s_1$: `MARIANA`

- $s_2$: `MERIANNA`

Las longitudes de estas cadenas son $|s_1| = 7$ y $|s_2| = 8$. 

**Paso 1: determinar el umbral de coincidencia**

Dos caracteres se consideran coincidentes si son iguales y la distancia entre sus posiciones no supera:

$$\big[\frac{max(|s_1|, |s_2|)}{2}\big] - 1 = 4 - 1 = 3$$

**Paso 2: identificar los caracteres coincidentes y calcular $m$**

Alineamos las cadenas posición a posición para facilitar la visualización:

<table style="border-collapse: collapse; text-align: center; font-family: monospace; font-size: 1em; margin-bottom: 20px;">
  <thead>
    <tr style="background-color: #e8e8e8;">
      <th style="border: 1px solid #ccc; padding: 8px 14px;">Pos.</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">1</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">2</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">3</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">4</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">5</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">6</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">7</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">8</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">s<sub>1</sub></td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">M</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">R</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">I</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">N</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;"></td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">s<sub>2</sub></td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">M</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">E</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">R</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">I</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">N</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">N</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
    </tr>
  </tbody>
</table>

Recorremos $s_1$ caracter por caracter y buscamos, dentro del umbral de 3 posiciones, si ese carácter aparece en $s_2$ (sin reutilizar caracteres ya emparejados):

- M (pos. 1): coincide con la M en pos. 1 de $s_2$ (distancia 0).

- A (pos. 2): coincide con la A en pos. 5 de $s_2$ (distancia = 3). 

- R (pos. 3): coincide con la R en pos. 3 de $s_2$ (distancia 0).

- I (pos. 4): coincide con la I en pos. 4 de $s_2$ (distancia 0). 

- A (pos. 5): coincide con la A en pos. 8 de $s_2$ (distancia = 3).

- N (pos. 6): coincide con la N en pos. 6 de $s_2$ (distancia 0).

- A (pos. 7): no hay ninguna A restante en $s_2$.

Por lo tanto, $m = 6$.

**Paso 3: identificar las transposiciones y calcular $t$**

Para calcular las transposiciones, construimos una nueva tabla listando únicamente los caracteres coincidentes. Tanto para $s_1$ como para $s_2$ los tomamos en el orden en que aparecen en las cadenas originales:

<table style="border-collapse: collapse; text-align: center; font-family: monospace; font-size: 1em; margin-bottom: 20px;">
  <thead>
    <tr style="background-color: #e8e8e8;">
      <th style="border: 1px solid #ccc; padding: 8px 14px;">Par</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">1</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">2</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">3</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">4</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">5</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">Coincidentes en $s_1$</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">M</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px; background-color: #ffd6d6;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px; background-color: #ffd6d6;">R</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px; background-color: #ffd6d6;">I</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px; background-color: #ffd6d6;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px; background-color: #ffd6d6;">N</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">Coincidentes en $s_2$</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">M</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px; background-color: #ffd6d6;">R</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px; background-color: #ffd6d6;">I</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px; background-color: #ffd6d6;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px; background-color: #ffd6d6;">N</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px; background-color: #ffd6d6;">A</td>
    </tr>
  </tbody>
</table>

El primer par coincide en posición. En los últimos cinco, en cambio, el carácter de $s_1$ y el de $s_2$ son distintos. Estos son los 5 caracteres transpuestos (resaltados en rojo). Según la definición de Jaro, $t$ es la mitad de ese conteo:  

$$t = \frac{5}{2} = 2.5 \approx 2$$

**Paso 4: calcular la similaridad de Jaro**

Con $m = 6$, $|s_1| = 7$, $|s_2| = 8$ y $t = 2$:

$$sim_J = \frac{1}{3}\big(\frac{6}{7} + \frac{6}{8} + \frac{6 - 2}{6}\big) = 0.7579$$

**Cálculo de la similaridad de Jaro-Winkler**

La única coincidencia al inicio de ambas cadenas está dada por la letra `M`. Con el valor estándar $p = 0.1$:

$$sim_{JW} = 0.7579 + 1~0.1(1 − 0.7579) = 0.7821$$

**Verificación de los resultados obtenidos:**

```{code-cell} python
from rapidfuzz.distance import Jaro, JaroWinkler
```

```{code-cell} python
# Similaridad de Jaro (redondeada a 4 decimales)
sim_j = round(Jaro.similarity('MARIANA', 'MERIANNA'), 4)
print(sim_j)
```

```{code-cell} python
# Similaridad de Jaro-Winkler (redondeada a 4 decimales)
sim_jw = round(JaroWinkler.similarity('MARIANA', 'MERIANNA'), 4)
print(sim_jw)
```

#### DELLA CECA vs. DELLACECCA

- $s_1$: `DELLA CECA`

- $s_2$: `DELLACECCA`

Las longitudes de estas cadenas son $|s_1| = 10$ y $|s_2| = 10$. 

**Paso 1: determinar el umbral de coincidencia**

Dos caracteres se consideran coincidentes si son iguales y la distancia entre sus posiciones no supera:

$$\big[\frac{max(|s_1|, |s_2|)}{2}\big] - 1 = 5 - 1 = 4$$

**Paso 2: identificar los caracteres coincidentes y calcular $m$**

Alineamos las cadenas posición a posición para facilitar la visualización:

<table style="border-collapse: collapse; text-align: center; font-family: monospace; font-size: 1em; margin-bottom: 20px;">
  <thead>
    <tr style="background-color: #e8e8e8;">
      <th style="border: 1px solid #ccc; padding: 8px 14px;">Pos.</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">1</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">2</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">3</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">4</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">5</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">6</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">7</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">8</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">9</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">10</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">s<sub>1</sub></td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">D</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">E</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">L</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">L</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">_</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">E</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">s<sub>2</sub></td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">D</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">E</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">L</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">L</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">E</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
    </tr>
  </tbody>
</table>

Recorremos $s_1$ caracter por caracter y buscamos, dentro del umbral de 3 posiciones, si ese carácter aparece en $s_2$ (sin reutilizar caracteres ya emparejados):

- D (pos. 1): coincide con la D en pos. 1 de $s_2$ (distancia 0).

- E (pos. 2): coincide con la E en pos. 2 de $s_2$ (distancia 0). 

- L (pos. 3): coincide con la R en pos. 3 de $s_2$ (distancia 0).

- L (pos. 4): coincide con la I en pos. 4 de $s_2$ (distancia 0). 

- A (pos. 5): coincide con la A en pos. 5 de $s_2$ (distancia 0).

- _ (pos. 6): no hay ningún espacio en $s_2$.

- C (pos. 7): coincide con la C en pos. 6 de $s_2$ (distancia = 1).

- E (pos. 8): coincide con la E en pos. 7 de $s_2$ (distancia = 1). 

- C (pos. 9): coincide con la C en pos. 9 de $s_2$ (distancia 0).

- A (pos. 10): coincide con la A en pos. 10 de $s_2$ (distancia 0). 

Por lo tanto, $m = 9$.

**Paso 3: identificar las transposiciones y calcular $t$**

Para calcular las transposiciones, construimos una nueva tabla listando únicamente los caracteres coincidentes. Tanto para $s_1$ como para $s_2$ los tomamos en el orden en que aparecen en las cadenas originales:

<table style="border-collapse: collapse; text-align: center; font-family: monospace; font-size: 1em; margin-bottom: 20px;">
  <thead>
    <tr style="background-color: #e8e8e8;">
      <th style="border: 1px solid #ccc; padding: 8px 14px;">Par</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">1</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">2</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">3</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">4</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">5</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">6</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">7</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">8</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">9</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">Coincidentes en $s_1$</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">D</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">E</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">L</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">L</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">E</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">Coincidentes en $s_2$</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">D</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">E</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">L</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">L</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">E</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
    </tr>
  </tbody>
</table>

Todos los pares coinciden en posición.

$$t = \frac{0}{2} = 0$$

**Paso 4: calcular la similaridad de Jaro**

Con $m = 9$, $|s_1| = 10$, $|s_2| = 10$ y $t = 0$:

$$sim_J = \frac{1}{3}\big(\frac{9}{10} + \frac{9}{10} + \frac{9}{9}\big) = 0.9333$$

**Cálculo de la similaridad de Jaro-Winkler**

Ambas cadenas comparten el prefijo `DELLA` (5 caracteres), pero Jaro-Winkler considera hasta un máximo de 4, por lo que $l = 4$. Con el valor estándar $p = 0.1$:

$$sim_{JW} = 0.9333 + 4~0.1(1 − 0.9333) = 0.9600$$

**Verificación de los resultados obtenidos:**

```{code-cell} python
from rapidfuzz.distance import Jaro, JaroWinkler
```

```{code-cell} python
# Similaridad de Jaro (redondeada a 4 decimales)
sim_j = round(Jaro.similarity('DELLA CECA', 'DELLACECCA'), 4)
print(sim_j)
```

```{code-cell} python
# Similaridad de Jaro-Winkler (redondeada a 4 decimales)
sim_jw = round(JaroWinkler.similarity('DELLA CECA', 'DELLACECCA'), 4)
print(sim_jw)
```

#### CÓRDOBA 2568 vs. CORDOBA 2478

- $s_1$: `CÓRDOBA 2568`

- $s_2$: `CORDOBA 2478`

Las longitudes de estas cadenas son $|s_1| = 12$ y $|s_2| = 12$. 

**Paso 1: determinar el umbral de coincidencia**

Dos caracteres se consideran coincidentes si son iguales y la distancia entre sus posiciones no supera:

$$\big[\frac{max(|s_1|, |s_2|)}{2}\big] - 1 = 6 - 1 = 5$$

**Paso 2: identificar los caracteres coincidentes y calcular $m$**

Alineamos las cadenas posición a posición para facilitar la visualización:

<table style="border-collapse: collapse; text-align: center; font-family: monospace; font-size: 1em; margin-bottom: 20px;">
  <thead>
    <tr style="background-color: #e8e8e8;">
      <th style="border: 1px solid #ccc; padding: 8px 14px;">Pos.</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">1</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">2</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">3</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">4</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">5</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">6</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">7</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">8</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">9</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">10</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">11</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">12</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">s<sub>1</sub></td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">Ó</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">R</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">D</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">O</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">B</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">_</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">2</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">5</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">6</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">8</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">s<sub>2</sub></td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">O</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">R</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">D</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">O</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">B</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">_</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">2</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">4</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">7</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">8</td>
    </tr>
  </tbody>
</table>

Recorremos $s_1$ caracter por caracter y buscamos, dentro del umbral de 3 posiciones, si ese carácter aparece en $s_2$ (sin reutilizar caracteres ya emparejados):

- C (pos. 1): coincide con la C en pos. 1 de $s_2$ (distancia 0).

- Ó (pos. 2): no hay ninguna Ó (con tilde) en $s_2$. 

- R (pos. 3): coincide con la R en pos. 3 de $s_2$ (distancia 0).

- D (pos. 4): coincide con la D en pos. 4 de $s_2$ (distancia 0). 

- O (pos. 5): coincide con la 0 en pos. 5 de $s_2$ (distancia 0).

- B (pos. 6): coincide con la B en pos. 6 de $s_2$ (distancia 0).

- A (pos. 7): coincide con la A en pos. 7 de $s_2$ (distancia 0).

- _ (pos. 8): coincide con el espacio en pos. 8 de $s_2$ (distancia 0). 

- 2 (pos. 9): coincide con el 2 en pos. 9 de $s_2$ (distancia 0).

- 5 (pos. 10): no hay ningún 5 en $s_2$. 

- 6 (pos. 11): no hay ningún 6 en $s_2$.

- 8 (pos. 12): coincide con el 8 en pos. 12 de $s_2$ (distancia 0). 

Por lo tanto, $m = 9$.

**Paso 3: identificar las transposiciones y calcular $t$**

Para calcular las transposiciones, construimos una nueva tabla listando únicamente los caracteres coincidentes. Tanto para $s_1$ como para $s_2$ los tomamos en el orden en que aparecen en las cadenas originales:

<table style="border-collapse: collapse; text-align: center; font-family: monospace; font-size: 1em; margin-bottom: 20px;">
  <thead>
    <tr style="background-color: #e8e8e8;">
      <th style="border: 1px solid #ccc; padding: 8px 14px;">Par</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">1</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">2</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">3</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">4</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">5</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">6</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">7</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">8</th>
      <th style="border: 1px solid #ccc; padding: 8px 14px;">9</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">Coincidentes en $s_1$</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">R</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">D</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">O</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">B</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">_</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">2</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">8</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ccc; padding: 8px 14px; font-weight: bold;">Coincidentes en $s_2$</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">C</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">R</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">D</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">O</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">B</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">A</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">_</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">2</td>
      <td style="border: 1px solid #ccc; padding: 8px 14px;">8</td>
    </tr>
  </tbody>
</table>

Todos los pares coinciden en posición.

$$t = \frac{0}{2} = 0$$

**Paso 4: calcular la similaridad de Jaro**

Con $m = 9$, $|s_1| = 12$, $|s_2| = 12$ y $t = 0$:

$$sim_J = \frac{1}{3}\big(\frac{9}{12} + \frac{9}{12} + \frac{9}{9}\big) = 0.8333$$

**Cálculo de la similaridad de Jaro-Winkler**

La única coincidencia al inicio de ambas cadenas está dada por la letra `C`. Con el valor estándar $p = 0.1$:

$$sim_{JW} = 0.8333 + 0.1(1 − 0.8333) = 0.8500$$

**Verificación de los resultados obtenidos:**

```{code-cell} python
from rapidfuzz.distance import Jaro, JaroWinkler
```

```{code-cell} python
# Similaridad de Jaro (redondeada a 4 decimales)
sim_j = round(Jaro.similarity('CÓRDOBA 2568', 'CORDOBA 2478'), 4)
print(sim_j)
```

```{code-cell} python
# Similaridad de Jaro-Winkler (redondeada a 4 decimales)
sim_jw = round(JaroWinkler.similarity('CÓRDOBA 2568', 'CORDOBA 2478'), 4)
print(sim_jw)
```



Para cada uno de los siguientes pares de cadenas, calcule la similaridad de Jaro y Jaro-Winkler y la distancia de Levenshtein. Realice primero el cálculo en forma manual y luego verifique los resultados obtenidos utilizando herramientas de la librería `rapidfuzz`.

| **cadena 1** | **cadena 2** |
| --- | --- |
| Mariana | Merianna |
| Della Ceca | Dellacecca |
| Córdoba 2568 | Cordoba 2478 |
| San Martín | AsnMartín |

Analice por qué Jaro-Winkler da mayor similaridad que Jaro en ciertos pares y no en otros.

### Ejercicio N°2

El dataset `ventas.xlsx` contiene los registros de una serie de ventas realizadas en el último tiempo en un local de productos electrónicos. Por otra parte, cuenta con el dataset `clientes_base.xlsx`, el cual contiene información sobre los clientes registrados en dicho establecimiento. 

1. ¿Cuál fue el monto total de venta de productos *iPad* y *MacBook*?

2. Realice la unión de ambos DataFrames utilizando la operación que considere más adecuada y la columna `nombre_cliente` como *key*. ¿Qué observa en el DataFrame resultante?

3. Considerando que en `clientes_base.xlsx` los nombres de los clientes se encuentran exentos de errores ortográficos y tipográficos, ¿en qué porcentaje de los registros que conforman el dataset `ventas.xlsx` el nombre del cliente coincide con el de un cliente registrado?

4. Teniendo en cuenta lo observado en los ítems anteriores, utilice herramientas de *fuzzy joins* para realizar la unión de ambos datasets. ¿De qué ciudad es el cliente que más compras realizó en el local?

### Ejercicio N°3

La siguiente tabla representa cinco canciones con sus vectores de frecuencia de términos, construidos a partir de las letras de cada una (vocabulario simplificado de 6 palabras):

|  | **"amor"* | **"noche"** | **"sol"** | **"lluvia"** | **"tiempo"** | **"vida"** |
| --- | --- | --- | --- | --- | --- | --- |
| cancion_1 | 8 | 1 | 0 | 0 | 2 | 5 |
| cancion_2 | 6 | 0 | 0 | 1 | 3 | 4 |
| cancion_3 | 0 | 7 | 3 | 5 | 1 | 0 |
| cancion_4 | 1 | 6 | 2 | 4 | 0 | 0 |
| cancion_5 | 5 | 0 | 8 | 0 | 1 | 3 |

1. ¿Qué medida propondría para evaluar la similaridad de las canciones, basada en el conjunto de términos que componen el vocabulario simplificado? 

2. Calcule manualmente dicha medida considerando `cancion_1` y `cancion_2` y verifique el resultado utilizando herramientas de la librería `SciPy`.

3. Calcule la matriz completa de similaridades entre las cinco canciones y represéntela visualmente mediante un gráfico apropiado. ¿Qué pares de canciones resultan más similares entre sí? 

### Ejercicio N°4

Una consultora evaluó a un conjunto de candidatos en tres pruebas de selección, todas puntuadas en la misma escala de 0 a 100. Los datos se encuentran en el archivo `candidatos.xlsx`.

1. Explore brevemente el conjunto de datos y realice un análisis exploratorio de la información que el mismo contiene.

2. Se propone utilizar tres variables numéricas para calcular distancias entre candidatos. ¿Considera necesario estandarizarlas antes de calcular distancias? Justifique.

3. Calcule las matrices de distancias euclídeas y Manhattan entre candidatos y visualícelas mediante un gráfico apropiado. 

- Identifique pares de candidatos que sean muy similares o muy diferentes. ¿Observa grupos o patrones?

- Según ambas métricas: ¿qué candidatos resultan más similares? ¿Qué candidatos resultan más diferentes? ¿Hay pares cuya similitud dependa de la métrica utilizada?


