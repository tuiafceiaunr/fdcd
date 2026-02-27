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

# Unidad 3 - Análisis exploratorio de datos: medidas de resumen

## Introducción

Una de las primeras etapas en cualquier trabajo con datos es el **análisis exploratorio de datos** (EDA, por sus siglas en inglés). Antes de calcular indicadores, ajustar modelos o responder preguntas específicas, necesitamos entender con qué información estamos trabajando.

Con frecuencia, cuando el objetivo del análisis ya está definido —por ejemplo, estimar un promedio o construir un modelo predictivo—, tendemos a ir directamente al procedimiento técnico. Sin embargo, detenernos previamente a explorar los datos es fundamental para evitar errores y para interpretar correctamente los resultados.

El análisis exploratorio nos permite:

- Comprender la estructura del dataset.

- Detectar errores, valores faltantes o inconsistencias.

- Identificar patrones preliminares.

- Formular hipótesis.

- Elegir métodos adecuados para el análisis posterior.

En esta unidad estudiaremos medidas de resumen, es decir, herramientas numéricas que permiten describir y sintetizar la información contenida en los datos mediante ciertos valores representativos. Estas medidas constituyen una primera aproximación cuantitativa al conjunto de datos. En la próxima unidad complementaremos este enfoque mediante el uso de visualizaciones, que nos permitirán analizar la información desde una perspectiva gráfica.

Para trabajar los conceptos utilizaremos datos de la Encuesta Nacional de Factores de Riesgo (ENFR 2018, los datos pueden descargarse [acá](https://www.indec.gob.ar/indec/web/Institucional-Indec-BasesDeDatos-2)), una encuesta que releva información sobre condiciones de salud, hábitos y factores de riesgo en la población adulta argentina. 

```{code-cell} python
import pandas as pd

data = pd.read_csv('datasets/enfr2018.txt', delimiter = '|')

data.head()
```

## Algunos conceptos fundamentales

Antes de avanzar con las medidas de resumen, es importante repasar algunos conceptos básicos de estadística.

### Población

En estadística, la población es el conjunto total de individuos, objetos, eventos o mediciones que comparten una característica de interés y sobre los cuales deseamos obtener información.

La población no necesariamente está compuesta por personas: puede estar formada por empresas, países, mediciones ambientales, transacciones financieras, experimentos de laboratorio, entre otros.

Un punto importante es que la población está definida por el objetivo del estudio. Por ejemplo:

- Si queremos estudiar el nivel de actividad física en Argentina, la población podría ser *“todas las personas adultas residentes en Argentina”*.

- Si queremos estudiar el rendimiento académico en una materia, la población podría ser *“todos los estudiantes que cursaron la materia en 2025”*.

La población puede ser muy grande (millones de individuos) o relativamente pequeña (unas pocas decenas).

### Muestra

En la práctica, rara vez es posible estudiar a toda la población. Por eso se selecciona una muestra, es decir, un subconjunto de la población que se utiliza para obtener información e inferir conclusiones sobre el conjunto total. El procedimiento mediante el cual se selecciona una muestra se denomina **muestreo**. Existen distintos tipos de muestreo, que pueden clasificarse, en términos generales, en:

- **Probabilísticos:** interviene el azar en la selección.

- **No probabilísticos:** no todos los elementos de la población tienen la misma probabilidad de ser seleccionados.

Para que los resultados obtenidos a partir de una muestra puedan generalizarse válidamente a la población, es fundamental que la muestra sea **representativa**, lo cual generalmente requiere algún tipo de selección aleatoria.

Por ejemplo, si deseamos estudiar la estatura de las mujeres adultas de un país, podríamos seleccionar una muestra aleatoria de mujeres adultas y medir su estatura. Si la muestra es suficientemente grande y representativa, podremos estimar la estatura promedio poblacional con cierto margen de error.

```{figure} imagenes/poblacion-muestra.png
---
width: 70%
align: center
---
Representación esquemática de una población compuesta por individuos y de una muestra seleccionada a partir de ella.
```

### Variable

Una variable es una característica o propiedad que puede observarse o medirse y que puede tomar distintos valores entre los elementos de la población. Las observaciones registradas de una o más variables conforman un conjunto de datos (**dataset**). En el formato largo presentado en la Unidad 2, cada fila del dataset constituye un registro y cada columna contiene la información correspondiente a una variable.

Según su naturaleza, las variables pueden clasificarse en **cualitativas** o **cuantitativas**.

#### Variables cualitativas (categóricas)

Las variables cualitativas son aquellas cuyos valores representan categorías o atributos, y no cantidades numéricas sobre las que tenga sentido realizar operaciones aritméticas. Aunque a veces se codifiquen con números, esos números funcionan únicamente como etiquetas.

En la ENFR 2018 encontramos múltiples ejemplos de este tipo de variables. Por ejemplo: el máximo nivel educativo alcanzado, el estado civil, la provincia de residencia y la cobertura de salud (sí/no, tipo de cobertura).

Dentro de las variables cualitativas podemos distinguir dos tipos:

- **Nominales:** las categorías no tienen un orden natural. En la ENFR, la provincia de residencia o el estado civil son variables nominales.

- **Ordinales:** las categorías presentan un orden, pero no es posible cuantificar la distancia entre ellas. El máximo nivel educativo alcanzado es un ejemplo claro: sabemos que *universitario* implica un nivel mayor que *secundario*, pero no podemos decir cuánto mayor en términos numéricos.

#### Variables cuantitativas

Son aquellas que toman valores numéricos para los cuales tiene sentido realizar operaciones aritméticas.

Se dividen en:

- **Discretas:** toman una cantidad finita de valores posibles. Ejemplos: número de hijos, número de miembros del hogar, número de intervenciones quirúrgicas.

- **Continuas:** pueden asumir cualquier valor dentro de un intervalo. Ejemplos: estatura, peso, presión arterial, tiempo hasta que llega un colectivo.


### Medición y escalas de medida

La medición es el proceso mediante el cual se asignan números o categorías a objetos, personas o hechos con el propósito de representar atributos de acuerdo con ciertas reglas. Medir no es simplemente “poner números”: implica respetar un sistema que determina qué significan esos números y qué operaciones pueden realizarse con ellos.

El tipo de análisis estadístico que podemos aplicar depende de la escala de medida de la variable. Tradicionalmente se distinguen cuatro niveles: nominal, ordinal, de intervalo y de razón. Cada uno de ellos habilita —y a la vez limita— las operaciones que tienen sentido realizar.

#### Escala nominal

En la escala nominal, los valores funcionan únicamente como etiquetas. No existe un orden inherente entre las categorías y estas son mutuamente excluyentes. Variables como la profesión o el grupo sanguíneo pertenecen a este nivel. Aunque puedan estar codificadas numéricamente en un dataset (por ejemplo, 1 = docente, 2 = comerciante, 3 = estudiante), esos números no representan cantidades. En consecuencia, las operaciones válidas se restringen a conteos, proporciones o la identificación de la categoría más frecuente (la moda). Calcular un promedio no tiene interpretación.

#### Escala ordinal

En la escala ordinal, las categorías presentan un orden natural, pero no es posible cuantificar la distancia entre ellas. Por ejemplo, el nivel educativo (primario, secundario, universitario), el nivel de dolor (leve, moderado, severo) o la posición de llegada en una carrera. Sabemos que una categoría es “mayor” o “más alta” que otra, pero no podemos afirmar cuánto mayor es. En este nivel podemos calcular frecuencias, proporciones, moda y también medidas basadas en el orden, como la mediana o los percentiles. Sin embargo, el promedio puede no ser interpretable, ya que presupone distancias numéricas bien definidas entre valores.

#### Escala de intervalo

La escala de intervalo incorpora un cambio fundamental: las diferencias entre los valores sí tienen significado cuantitativo. Un ejemplo clásico es la temperatura medida en grados Celsius. La diferencia entre 10 °C y 20 °C es la misma que entre 20 °C y 30 °C. No obstante, el cero no representa ausencia absoluta de la magnitud, por lo que las razones no son interpretables. No tiene sentido afirmar que 20 °C es el doble de 10 °C. En este nivel ya es válido calcular media, varianza y desvío estándar, así como realizar operaciones de suma y resta.

#### Escala de razón

Finalmente, la escala de razón posee todas las propiedades anteriores y, además, cuenta con un cero absoluto que indica ausencia real de la magnitud medida. Variables como la estatura, el peso, los ingresos o la edad pertenecen a esta categoría. Aquí tanto las diferencias como las razones tienen significado: decir que una persona de 40 años tiene el doble de edad que una de 20 es una afirmación matemáticamente válida. Esta escala permite la mayor variedad de operaciones estadísticas y es la más informativa desde el punto de vista cuantitativo.

Comprender la escala de medida no es un detalle técnico menor. De ella depende qué medidas de resumen podemos calcular, qué visualizaciones son apropiadas, qué modelos estadísticos resultan válidos y qué interpretaciones tienen sentido. Retomando ejemplos mencionados anteriormente, en una variable nominal podemos hablar de proporciones, pero no de promedios; en una variable ordinal podemos analizar la mediana; y en una variable de razón podemos calcular media, varianza o coeficiente de variación.

Elegir una medida inapropiada para el tipo de variable puede conducir a interpretaciones erróneas. Por eso, antes de calcular cualquier estadístico, es fundamental preguntarse primero: **¿qué estoy midiendo y en qué escala?**

## Medidas de posición

Las medidas de posición brindan información acerca de la localización del conjunto de observaciones de una variable.

### Media aritmética

La media aritmética, comúnmente llamada promedio y denotada como $\bar{X}$, se define como la suma de todos los valores observados de una variable dividida por el número total de observaciones:

$$\bar{X} = \frac{1}{n}\sum_{i=1}^{n} X_i$$

Es una medida de tendencia central que utiliza toda la información disponible en el conjunto de datos.

En Pandas, la media puede calcularse fácilmente con el método **`mean()`**.

```{code-cell} python
data.bhih01.mean()
```

Como parte del análisis descriptivo, no solo debemos calcular medidas numéricas representativas, sino también **interpretarlas en el contexto del problema**. Esto constituye un aspecto central en cualquier análisis de datos. El valor obtenido puede interpretarse de la siguiente manera:

> ***En promedio, el ingreso total mensual de los hogares encuestados es de $22446.65.***

```{admonition} **Importante**
:class: important

Es importante tener presente que la media es sensible a valores extremos. Si en el conjunto de datos existen ingresos muy altos o muy bajos en comparación con la mayoría, estos influirán de manera significativa en el promedio. Esta idea será retomada más adelante.
```

### Mediana ($Q_2$)

La mediana, también llamada segundo cuartil ($Q_2$) o percentil 50, es el valor que ocupa la posición central cuando los datos se ordenan de menor a mayor. Divide al conjunto en dos partes iguales: el 50% de las observaciones son menores o iguales a la mediana y el otro 50% son mayores o iguales.

A diferencia de la media, la mediana no depende de todos los valores en magnitud, sino únicamente del orden. Por ello, es mucho más robusta frente a valores extremos.

En Pandas, la mediana puede calcularse fácilmente con el método **`median()`**.

```{code-cell} python
data.bhih01.median()
```
> ***El 50 % de los hogares encuestados presenta un ingreso mensual total menor o igual a $18000.***

```{admonition} **Media vs. mediana**
:class: important

La comparación entre media y mediana es particularmente relevante en variables económicas.

Dado que la media utiliza todos los valores registrados, la presencia de observaciones anormalmente grandes o pequeñas influye de manera sensible en su valor. En cambio, la mediana solo depende de la posición central.

En distribuciones simétricas, media y mediana suelen ser similares. Pero en distribuciones asimétricas —como suele ocurrir con los ingresos— la media puede diferir considerablemente de la mediana. Cuando la distribución presenta asimetría hacia la derecha, algunos valores elevados tienden a incrementar el promedio.

En nuestro conjunto de datos, la media del ingreso mensual es de \$22446.65, mientras que la mediana es de \$18000. El hecho de que la media sea mayor que la mediana sugiere la presencia de ingresos relativamente altos que elevan el promedio por encima del valor central. En situaciones como esta, la mediana suele considerarse una medida más representativa del “valor típico” o de la tendencia central de los datos. Por este motivo, resultaría más apropiado informar la mediana como resumen del ingreso mensual de estos hogares.
```

### Media truncada (*trimmed mean*)

Una alternativa intermedia entre la media y la mediana es la media truncada. Consiste en calcular el promedio una vez que se ha eliminado un cierto porcentaje $\alpha$ 100 % de las observaciones más pequeñas y más grandes del conjunto de datos. Por ejemplo, una media truncada al 10% elimina el 10% inferior y el 10% superior antes de promediar.

Esta medida reduce la influencia de valores extremos, pero sigue utilizando una gran parte de la información disponible. Conceptualmente, se ubica entre la media (que usa todos los datos) y la mediana (que solo depende del centro).

Un ejemplo conocido de su aplicación es la evaluación de pruebas olímpicas de patinaje artístico sobre hielo y otras disciplinas artísticas y deportivas, donde se descartan puntajes extremos antes de calcular el promedio final.

```{figure} imagenes/patinaje.jpg
---
width: 70%
align: center
---
```

### Fractilas o cuantilos

La mediana es, en realidad, un caso particular de una familia más amplia de medidas llamadas **fractilas o cuantilos**.

En términos generales, la fractila de orden $r$ (con $0 \leq r \leq 1$) es el valor de la variable tal que el $r$ 100 % de las observaciones son menores o iguales a él.

Por ejemplo:

- El percentil 25 deja por debajo al 25% de los datos.

- El percentil 50 coincide con la mediana.

- El percentil 90 deja por debajo al 90% de las observaciones.


#### Cuartilos ($Q_1$, $Q_2$ y $Q_3$)

Los cuartilos son un tipo particular de fractilas que dividen al conjunto de datos ordenados en cuatro partes aproximadamente iguales:

- **$Q_1$ (primer cuartil):** es aquel valor de la variable tal que el 25 % de las observaciones son menores o iguales a él.

- **$Q_2$ (segundo cuartil):** es aquel valor de la variable tal que la mitad de las observaciones son menores o iguales a él (coincide con la mediana).

- **Q3 (tercer cuartil):** es aquel valor de la variable tal que el 75 % de las observaciones son menores o iguales a él.

En Pandas, cualquier cuantilo puede calcularse con el método **`quantile()`**. A continuación, veremos la aplicación de esta función para el cálculo de los tres cuartilos:

```{code-cell} python

q1 = data.bhih01.quantile(0.25)  # Q1
q2 = data.bhih01.quantile(0.50)  # Q2
q3 = data.bhih01.quantile(0.75)  # Q3  

print(f"Q1: {q1}")
print(f"Q2: {q2}")
print(f"Q3: {q3}")
```

**Interpretación de los valores obtenidos:**

> ***El 25% de los hogares encuestados reporta ingresos mensuales menores o iguales a \$10000. Por otro lado, la mitad de los hogares percibe ingresos menores o iguales a  \$18000, mientras que el 75% de los hogares reporta ingresos menores o iguales a $30 000.***


```{dropdown} ¿Cómo calcula Pandas los cuantilos?
:class: seealso

Por defecto, Pandas utiliza el método de interpolación lineal conocido como **método R-7** (también utilizado en R, Excel y NumPy).

Según este método, la posición de la fractila $Q$ se calcula como:

$$Pos = 1 + Q(n-1)$$

Si la posición no es un número entero, se interpola linealmente entre los dos valores adyacentes del conjunto ordenado.

Por ejemplo, para los datos:

$$[2,2,4,5,5,6,7,8]$$

el primer cuartil se ubica en la posición:

$$Pos=1+0.25(8−1)=2.75$$

Como 2.75 se encuentra entre las posiciones 2 y 3, el valor se obtiene interpolando:

$$Q_1 = 2 + 0.75(4-2) = 3.5$$

Cuando la posición calculada para un cuantilo no es un número entero, es necesario decidir cómo obtener el valor correspondiente, lo que se hace modificando el parámetro `interpolation`. Por defecto, Pandas utiliza el método de interpolación `'linear'` (**método R-7**), que interpola linealmente entre los dos valores adyacentes del conjunto ordenado.

Sin embargo, existen otras alternativas que modifican el resultado cuando la posición no coincide exactamente con un dato observado (consultar la [documentación oficial](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.quantile.html#pandas.DataFrame.quantile) para más información):

- **`interpolation = 'lower'`**: toma directamente el valor inferior, es decir, el que se encuentra inmediatamente antes de la posición calculada.

- **`interpolation = 'higher'`**: toma el valor superior, es decir, el que se encuentra inmediatamente después de la posición calculada.

- **`interpolation = 'nearest'`**: selecciona el valor más cercano a la posición estimada.

- **`interpolation = 'midpoint'`**: calcula el punto medio entre los dos valores adyacentes.

Estas opciones pueden producir resultados ligeramente distintos, especialmente en muestras pequeñas. En muestras grandes, las diferencias suelen menores.

Es importante ser conscientes de qué método se está utilizando, sobre todo cuando se comparan resultados obtenidos con distintos programas o paquetes estadísticos, ya que no todos emplean el mismo criterio por defecto.
```

#### Decilos y percentilos

Además de los cuartilos, existen otras fractilas que permiten dividir el conjunto de datos ordenado en partes iguales más pequeñas.

Los **decilos** son valores que dividen a las observaciones en diez partes con aproximadamente el mismo número de datos. Cada decilo deja por debajo al 10 %, 20 %, 30 %, …, 90 % de las observaciones, respectivamente.

Por su parte, los **percentilos** dividen al conjunto en cien partes. Así, por ejemplo, el percentil 90 deja por debajo al 90% de los datos.


### Moda

La moda es el valor de la variable que aparece con mayor frecuencia en el conjunto de datos.

En Pandas, puede calcularse con el método **`mode()`**:

```{code-cell} python

data.bhih01.mode()
```

> ***El ingreso total mensual más frecuente entre los hogares encuestados es $20000.***

A diferencia de la media o la mediana, **la moda puede no ser única**. Es posible que un conjunto de datos presente varias modas (distribución multimodal). En esos casos, el método `mode()` devuelve una Serie con todos los valores que comparten la frecuencia máxima:

```{code-cell} python

serie_ejemplo = pd.Series([2, 3.5, 8, 3.5, 4, 6.5, 8, 6.5])
serie_ejemplo.mode()
```

Aquí observamos que existen tres valores igualmente frecuentes.

Un aspecto importante es que la moda **es la única medida de posición que puede utilizarse con variables cualitativas**. En variables categóricas no tiene sentido hablar de media o mediana, pero sí podemos identificar la categoría más frecuente.

Por ejemplo, en el contexto de la ENFR 2018, si calculamos la moda de la variable *tipo de vivienda* (`bhcv01`), obtenemos el siguiente resultado:

```{code-cell} python

data.bhcv01.mode()
```

Si el resultado es 1, esto no significa que “1” sea el tipo de vivienda más frecuente en sí mismo, sino que **la categoría codificada como 1 es la más frecuente**. En este caso, según el diccionario de variables:

1. Casa  
2. Casilla  
3. Departamento  
4. Pieza de inquilinato  
5. Pieza en hotel o pensión  
6. Local no construido para habitación  
7. Otros  

Por lo tanto, la categoría más frecuente entre los hogares encuestados es **casa**.

Este ejemplo ilustra una cuestión importante en el análisis de datos: muchas variables cualitativas se encuentran codificadas numéricamente. Antes de interpretar los resultados, es indispensable consultar el diccionario de variables para comprender qué representa cada código.

## Medidas de dispersión

Las medidas de tendencia central —como la media y la mediana— nos dan una idea acerca de la localización del conjunto de datos en el eje de variación en el que se mueve la variable de interés. Sin embargo, resumen sólo parte de la información contenida en el conjunto de datos

Dos conjuntos de observaciones pueden tener la misma media y la misma mediana, pero diferir considerablemente en cuánto se "alejan"" los valores individuales respecto del valor central. Surge así la necesidad de incorporar medidas de dispersión, cuyo objetivo es **describir la variabilidad de los datos.**

Para ilustrar esta idea, consideremos los siguientes dos conjuntos de datos:

**Conjunto I:** 40, 38, 42, 40, 39, 39, 43, 40, 39, 40

**Conjunto II:** 46, 37, 40, 33, 42, 36, 40, 47, 34, 45

Vamos a generar un DataFrame para poder trabajar con ellos:

```{code-cell} python
import pandas as pd
import numpy as np

# Creamos los dos conjuntos
data_I = [40, 38, 42, 40, 39, 39, 43, 40, 39, 40]
data_II = [46, 37, 40, 33, 42, 36, 40, 47, 34, 45]

# Creamos un DataFrame en formato largo
df = pd.DataFrame({
    "valor": data_I + data_II,
    "grupo": ["Conjunto I"] * 10 + ["Conjunto II"] * 10
})

df.head()
```

Exploremos ahora cuál es la media y la mediana de cada conjunto:

```{code-cell} python
df.groupby("grupo")["valor"].agg(["mean", "median"])
```
A partir de la salida anterior, observamos que, aunque los conjuntos están compuestos por valores distintos, ambos tienen la misma media y la misma mediana. Esto muestra que las medidas de tendencia central no capturan toda la información relevante sobre la distribución de los datos.

A continuación, representamos gráficamente ambos conjuntos utilizando un gráfico de tipo *jitter plot*, que permite visualizar cada observación individual:

```{code-cell} python
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize = (4.5,5))

sns.stripplot(x = 'valor', y = 'grupo', hue = 'grupo', size = 7, data = df)
plt.xlabel('Variable', fontweight='bold')
plt.ylabel('Conjunto', fontweight='bold')

plt.show()
```

Aunque ambos conjuntos comparten la misma media, mediana y moda, el gráfico revela una diferencia sustancial en su distribución. En el Conjunto I, los valores se concentran alrededor de 40 y muestran poca variabilidad. En cambio, en el Conjunto II, las observaciones se encuentran más alejadas del centro y cubren un rango más amplio.

Decimos entonces que el Conjunto II presenta mayor dispersión.

Esta diferencia visual pone de manifiesto que conocer únicamente el centro de los datos no es suficiente. Necesitamos incorporar medidas que cuantifiquen cuánto se apartan las observaciones respecto de ese centro. Estas medidas reciben el nombre de medidas de dispersión y serán desarrolladas a continuación.

Para ilustrar cada medida, utilizaremos la variable ingreso total mensual del hogar (`bhih01`), de la ENFR.

### Rango

El rango es la diferencia entre el mayor y el menor valor observado de la variable:

$$R = X_{max} - X_{min}$$

En Pandas, puede calcularse como:

```{code-cell} python
rango = data.bhih01.max() - data.bhih01.min()

print(rango)
```

A continuación, una forma de interpretar el valor obtenido en el contexto de los datos de la encuesta:

> ***Los ingresos totales mensuales en los hogares encuestados se encuentran en un rango de \$420000.*** 

```{admonition} **Importante**
:class: important

Si el valor obtenido es elevado en el contexto de los datos con los que estamos trabajando, esto sugiere la presencia de una gran heterogeneidad entre las observaciones. No obstante, es preciso tener presente que **el rango depende exclusivamente de los valores extremos**, por lo que puede verse fuertemente afectado por la existencia de observaciones atípicas.
```

### Varianza y desvío estándar

La varianza mide cuánto se desvían, en promedio, las observaciones respecto de la media aritmética. Se define como:

$$S^2=\frac{1}{n-1}\sum_{i=1}^{n}{(X_i - \bar{X})^2}$$

Cuanto mayor es la variabilidad de los datos, mayor será la varianza.

Por su parte, el desvío estándar es la raíz cuadrada positiva de la varianza:

$$S^2=\sqrt{\frac{1}{n-1}\sum_{i=1}^{n}{(X_i - \bar{X})^2}}$$

En Pandas:

```{code-cell} python
varianza = data.bhih01.var()
desv_est = data.bhih01.std()

print(f"Varianza: {varianza}")
print(f"Desvío estándar: {desv_est}")
```

A diferencia de la varianza, el desvío estándar está expresado en las mismas unidades que la variable, lo que facilita su interpretación.

> ***En promedio, los ingresos totales mensuales de los hogares encuestados se desvían aproximadamente \$19756.58 con respecto a la media de \$22446.65.***

### Coeficiente de Variación (CV)

El coeficiente de variación (CV) se define como el cociente entre el desvío estándar y el valor absoluto de la media:

$$CV = \frac{S}{\bar{|X|}}$$

Es una medida adimensional, lo que permite comparar la variabilidad relativa entre variables con distintas unidades o escalas.

En Python:

```{code-cell} python
cv = data.bhih01.std() / data.bhih01.mean()

print(cv)
```

> ***El desvío estándar del ingreso total mensual de los hogares encuestados representa un 88 % del valor promedio correspondiente.***

Retomando su característica de medida adimensional, supongamos que queremos comparar la dispersión del ingreso mensual de los hogares encuestados en Argentina con la de un estudio análogo realizado en Uruguay.

Más allá de la realidad económica de cada país, dado que los niveles de ingreso están medidos en escalas diferentes por tratarse de monedas distintas, no tendría sentido comparar directamente los desvíos estándar expresados en sus monedas originales. Un mayor desvío estándar en Argentina podría deberse simplemente a que los ingresos están expresados en una escala monetaria numéricamente más alta, y no necesariamente a una mayor variabilidad relativa.

En este sentido, el coeficiente de variación, al ser una medida adimensional, permite comparar la dispersión relativa de los ingresos entre ambos países independientemente de la unidad monetaria utilizada.


### Rango intercuartil (RI)

El rango intercuartil se define como la diferencia entre el tercer y el primer cuartil:

$$RI = Q_3 - Q_1$$

Mide la amplitud del 50% central de los datos y, a diferencia del rango, no se ve afectado por valores extremos.

En Pandas:

```{code-cell} python
q1 = data.bhih01.quantile(0.25)
q3 = data.bhih01.quantile(0.75)

ri = q3 - q1

print(ri)
```

> ***El 50% central de los ingresos totales anuales de los hogares encuestados se encuentra en un rango de \$20000.***

El rango intercuartil es una medida robusta de dispersión y resulta especialmente adecuada cuando se utiliza la mediana como medida de tendencia central.

### Desviación mediana absoluta (MAD)

La desviación mediana absoluta (MAD) es una medida robusta de dispersión basada en la mediana. Se define como:

$$MAD = \text{Mediana}(|X_i - Q_2|)$$

Es decir, es la mediana de las diferencias absolutas entre cada observación y la mediana del conjunto.

Supongamos que tenemos el siguiente conjunto de valores observados: 

$$[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]$$ 

Para calcular la MAD seguimos los siguientes pasos:

1. Calcular la mediana:

$$Q_2 = 6$$

2. Calcular la desviación absoluta de cada observación con respecto a la mediana:

$$[5, 4, 3, 2, 1, 0, 1, 2, 3, 4, 5]$$

3. Ordenar la lista de desviaciones absolutas de menor a mayor: 

$$[0, 1, 1, 2, 2, 3, 3, 4, 4, 5, 5]$$

4. Calcular la mediana del conjunto del ítem anterior: 

$$MAD = 3$$

En Python, y considerando el ejemplo de la ENFR con el que venimos trabajando:

```{code-cell} python
mediana = data.bhih01.median()
dif_abs = abs(data.bhih01 - mediana)

mad = dif_abs.median()

print(mad)
```

Interpretación:

> ***El 50 % de los ingresos totales mensuales de los hogares encuestados se aleja de la mediana un total de \$9000 o menos, en valor absoluto.***

La MAD representa la desviación típica respecto de la mediana. Al estar basada en una medida robusta de centralidad, no se ve influenciada por valores extremos, por lo que es especialmente útil en distribuciones asimétricas.

## El método **describe()**

Una forma rápida de obtener varias de las métricas vistas es utilizar el método **`describe()`**:

```{code-cell} python

data.bhih01.describe()
```

Como se observa, este método permite obtener un panorama general de la distribución de la variable en una sola salida, ya que devuelve:

- cantidad de observaciones,

- media,

- desvío estándar,

- mínimo y máximo,

- cuartiles.

Apliquémoslo ahora sobre una variable cualitativa, como el *tipo de vivienda* de la Encuesta Nacional de Factores de Riesgo (ENFR 2018). Antes de continuar, recordemos que la variable bhcv01 está codificada numéricamente:

1 = Casa

2 = Casilla

3 = Departamento

4 = Pieza de inquilinato

5 = Pieza en hotel o pensión

6 = Local no construido para habitación

7 = Otros

Por lo tanto, aunque conceptualmente es una variable cualitativa, su tipo de dato en el DataFrame no es categórico:

```{code-cell} python
data['bhcv01'].dtype
```
En este punto, pandas la interpreta como una variable numérica (`int64`).

Si ejecutáramos el `describe()`, obtendríamos media, desvío estándar y percentiles. Sin embargo, estos estadísticos no tienen interpretación sustantiva, ya que los valores 1, 2, 3, … no representan cantidades, sino categorías. Por ejemplo, un promedio de 2.8 no corresponde a ningún tipo real de vivienda.

Esto ocurre porque Pandas decide qué resumen mostrar en función del tipo de dato almacenado, no del significado conceptual de la variable.

La solución conceptual correcta (y sencilla) es indicarle explícitamente que se trata de una variable categórica:

```{code-cell} python 
data["bhcv01"] = data["bhcv01"].astype("category")
data.bhcv01.describe()
``` 

Ahora `describe()` devuelve:

- `count`

- `unique`

- `top`

- `freq`

que sí son estadísticos adecuados para una variable cualitativa.

Queda como ejercicio propuesto interpretar cada uno de los componentes de esta salida.

## Medidas de forma

Además de describir el centro y la dispersión de una variable cuantitativa, en muchos casos resulta relevante analizar **la forma de su distribución**. Dos medidas clásicas que permiten caracterizarla son el **sesgo** (asimetría) y la **curtosis**. En esta sección abordaremos

### Sesgo (asimetría)

El sesgo mide el grado de asimetría de una distribución respecto de su media.

- Si el sesgo es **cercano a 0**, la distribución es **aproximadamente simétrica**. En este caso, la media, la mediana y la moda tienden a coincidir o a ubicarse muy próximas entre sí.

- Si el sesgo es *positivo**, la distribución presenta una **cola más larga hacia la derecha**. Esto suele implicar que existen algunos valores relativamente altos que “estiran” la distribución hacia ese lado. En este caso, típicamente se verifica: moda < mediana < media.

- Si el sesgo es *negativo**, **la cola se extiende hacia la izquierda**. En este caso, suele observarse: media < mediana < moda.

En términos intuitivos, el sesgo indica hacia qué lado se concentran los valores extremos.

En Python, el coeficiente de asimetría puede calcularse directamente utilizando el método **`skew()`**. 

La siguiente figura ilustra gráficamente los tres casos posibles.

```{figure} imagenes/skewness.png
---
width: 70%
align: center
---
Histogramas que muestran ejemplos de distribuciones simétrica, asimétrica a la derecha (sesgo positivo, *right-skewed*) y asimétrica a la izquierda (sesgo negativo, *left-skewed*). Se indica la posición relativa de la media, la mediana y la moda en cada caso.
```

### Curtosis

La curtosis es una medida de forma que describe el grado en que una distribución se aleja de la **distribución normal** en términos de su apuntamiento (concentración central) y del peso de sus colas.

```{dropdown} Sobre la distribución normal
:class: seealso

La distribución normal (o gaussiana) es una de las distribuciones de probabilidad más importantes en estadística. Tiene forma de campana, es simétrica respecto de su media y queda completamente determinada por dos parámetros:

- la esperanza ($\mu$), que determina el centro,

- la desviación estándar ($\sigma$), que determina la dispersión.

Algunas propiedades relevantes:

- Es simétrica: la esperanza, la mediana y la moda coinciden.

- La mayor parte de los valores se concentra alrededor del centro.

- La probabilidad de observar valores muy alejados de la media decrece rápidamente a medida que nos movemos hacia las colas.

- Aproximadamente:

    - el 68% de los valores cae dentro de $\pm1\sigma$,

    - el 95% dentro de $\pm2\sigma$,

    - el 99.7% dentro de $\pm3\sigma$.

En el estudio de la curtosis, la distribución normal cumple un rol especial: se utiliza como punto de referencia. Cuando hablamos de **exceso de curtosis** (*ver más adelante en el texto principal*), estamos midiendo cuánto se aparta una distribución del nivel de curtosis de la normal.
```

En la distribución normal, el coeficiente de curtosis es igual a 3. Cuando una distribución presenta aproximadamente ese mismo valor, se dice que es **mesocúrtica**.

Si la curtosis es:

- **Mayor que 3**, la distribución se denomina **leptocúrtica**. En este caso, los datos se encuentran más concentrados alrededor de la media que en la distribución normal, lo que produce una forma más "puntiaguda" en el centro. Además, aunque inicialmente la curva decae con mayor rapidez, en los extremos resulta relativamente más alta que la normal. Esto implica una mayor probabilidad de observar valores extremos.

- **Menor que 3**, la distribución se denomina **platicúrtica**. Presenta una menor concentración central y una forma más achatada, con colas relativamente más livianas.

Es importante destacar que la curtosis no mide únicamente la “altura del pico” de la distribución. En realidad, refleja una combinación entre concentración central y comportamiento en las colas. Tampoco debe confundirse con la variabilidad: una distribución leptocúrtica no necesariamente tiene menor varianza por ser más apuntada, ni una platicúrtica mayor varianza por ser más achatada.

En Python, la curtosis puede calcularse mediante el método **`kurt()`**. Es importante señalar que Pandas reporta el **exceso de curtosis**, es decir, la curtosis menos 3. Bajo esta convención, un valor cercano a 0 indica una distribución mesocúrtica (similar a la normal), valores positivos indican leptocurtosis y valores negativos indican platicurtosis.

Al igual que el sesgo, la curtosis es sensible a la presencia de valores extremos, por lo que su interpretación debe realizarse junto con el análisis gráfico de la distribución.

Vamos a analizar tres ejemplos con datos "de juguete":

```{code-python} python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Simulamos tres conjuntos de datos con distinta curtosis
np.random.seed(123)

n = 10000

## Set 1) Curtosis ≈ 0 → Normal
normal_data = pd.Series(np.random.normal(loc=0, scale=1, size=n))

## Set 2) Curtosis < 0 (colas más livianas)
### Parámetros: izquierda=-3, moda=0, derecha=3
triangular_data = pd.Series(np.random.triangular(left=-3, mode=0, right=3, size=n))

## Set 3) Curtosis > 0 (colas más pesadas)
laplace_data = pd.Series(np.random.laplace(loc=0, scale=1, size=n))

# Mostramos el exceso de curtosis
print("Exceso de curtosis:")
print("Normal     :", round(normal_data.kurt(), 3))
print("Triangular :", round(triangular_data.kurt(), 3))
print("Laplace    :", round(laplace_data.kurt(), 3))
```

Observemos los tres conjuntos anteriores representados gráficamente a través de histogramas de frecuencias:

```{code-cell} python

xmin, xmax = -6, 6

# --- Set 1 ---
plt.figure()
plt.hist(normal_data, bins = 40, range = (xmin, xmax), color = '#f7b552', edgecolor = 'black')
plt.title("Set 1 (Curtosis ≈ 0)")
plt.xlabel("Valores", fontweight = 'bold')
plt.ylabel("Frecuencia", fontweight = 'bold')
plt.xlim(xmin, xmax)
plt.show()

# --- Set 2 ---
plt.figure()
plt.hist(triangular_data, bins = 40, range = (xmin, xmax), color = 'lightblue', edgecolor = 'black')
plt.title("Set 2 (Curtosis < 0)")
plt.xlabel("Valores", fontweight = 'bold')
plt.ylabel("Frecuencia", fontweight = 'bold')
plt.xlim(xmin, xmax)
plt.show()

# --- Set 3 ---
plt.figure()
plt.hist(laplace_data, bins = 40, range = (xmin, xmax), color = 'lightgreen', edgecolor = 'black')
plt.title("Set 3 (Curtosis > 0)")
plt.xlabel("Valores", fontweight = 'bold')
plt.ylabel("Frecuencia", fontweight = 'bold')
plt.xlim(xmin, xmax)
plt.show()
```


## Diagrama de caja y bigotes (boxplot)

Los cuartilos, junto con los valores mínimo y máximo, conforman un conjunto de cinco números que resume de manera muy compacta la información esencial de una variable cuantitativa: dónde se ubican los datos, qué tan concentrados están en torno al centro y cuál es el rango de variación.

A partir de estos cinco valores podemos construir un gráfico llamado **boxplot** (o diagrama de caja y bigotes). El boxplot traduce ese resumen numérico en una representación visual sencilla pero muy potente.

En este gráfico, la parte central está formada por una “caja” que se extiende desde el primer cuartil ($Q_1$) hasta el tercer cuartil ($Q_3$). Esa caja contiene el 50 % central de las observaciones. En su interior se dibuja una línea que marca la mediana, es decir, el valor que divide al conjunto de datos ordenados en dos subconjuntos con aproximadamente el mismo número de observaciones. Desde la caja se proyectan hacia ambos lados los llamados *whiskers* (bigotes), que en la versión clásica llegan hasta el mínimo y el máximo observados.

De este modo, el boxplot permite visualizar de un vistazo la posición central, la dispersión y las características de simetría de la distribución. Sin necesidad de ver todos los datos individuales, obtenemos una síntesis clara de su comportamiento.

```{figure} imagenes/boxplot.png
---
width: 70%
align: center
---
Esquema representativo de un diagrama de caja y bigotes (boxplot) clásico.
```

### El boxplot modificado

Existe una versión modificada del boxplot que permite detectar potenciales *outliers*, es decir, observaciones que no son típicas del conjunto. El criterio comúnmente aceptado considera como potenciales outliers aquellas observaciones que se encuentren por fuera del intervalo:

$$(Q_1 - 1.5 RI)\qquad\qquad\text{y}\qquad\qquad(Q_3 + 1.5 RI)$$

donde $RI = Q_3 - Q_1$ es el rango intercuartílico.

En el boxplot modificado:

- Los *whiskers* ya no llegan necesariamente al mínimo y máximo absolutos, sino que se extienden hasta el valor mínimo y máximo que no sea considerado atípico según el criterio anterior.

- Las observaciones que quedan fuera de esos límites se representan como puntos individuales.

Este formato es el que utilizan la mayoría de los softwares estadísticos por defecto.

Para ilustrar el boxplot modificado, trabajaremos con la variable *minutos semanales de actividad física intensa* (según el diccionario de registros de la ENFR 2018, se trata de actividades que hacen respirar mucho más rápido, exigen mayor esfuerzo físico y aceleran el ritmo cardíaco).

Para el ejemplo, filtraremos únicamente las observaciones correspondientes a la provincia de Santa Fe.

```{code-cell} python
data_stafe = data[data['cod_provincia'] == 82]
```

Construimos el boxplot utilizando funciones de las librerías `matplotlib` y `seaborn`, que veremos con más detalle en la próxima unidad:

```{code-cell} python
import seaborn as sns
import matplotlib.pyplot as plt

plt.figure(figsize = (6,4))

sns.boxplot(x = 'biaf02_m', data = data_stafe)
plt.xlabel('Minutos semanales de actividad física intensa', fontweight = 'bold')
plt.show()
```
El gráfico muestra claramente que la distribución de estos datos es asimétrica hacia la derecha, y que hay algunas observaciones que, a la luz del criterio definido anteriormente, son potencialmente atípicas para este conjunto. Notar que el bigote inferior es coincidente con el valor mínimo, mientras que el superior se extiende hasta el máximo valor que se encuentra por debajo de $Q_3 + 1.5 RI$.

Queda como ejercicio propuesto pensar qué medidas de posición y dispersión resultan más representativas para describir a un dataset con estas características.

#### Sobre la interpretación de los *outliers*

Como mencionamos anteriormente, en el contexto del boxplot modificado, un *outlier* es una observación que cae fuera del intervalo definido por

$$(Q_1 - 1.5 RI)\qquad\qquad\text{y}\qquad\qquad(Q_3 + 1.5 RI)$$

Es importante destacar que este criterio es convencional. Fue popularizado por John Tukey en el marco del análisis exploratorio de datos, y su propósito es identificar observaciones inusuales respecto del patrón central de la distribución. **Sin embargo, no constituye una regla que determine automáticamente que un dato sea incorrecto.**

Un *outlier* es, estrictamente, una observación atípica según un criterio estadístico. Eso no implica que sea, necesariamente un error de carga, un valor imposible ni un dato que deba eliminarse.

En muchos contextos, los valores extremos son completamente válidos y forman parte del fenómeno bajo estudio. Por ejemplo:

- En ingresos económicos, es esperable que existan valores muy altos respecto de la mayoría.

- En actividad física semanal, algunas personas pueden reportar minutos de actividad considerablemente superiores al promedio.

- En variables biomédicas, ciertos individuos pueden presentar valores extremos sin que ello implique un error de medición.

**Eliminar observaciones únicamente por el hecho de ser *outliers* puede alterar la distribución original de los datos y sesgar los resultados del análisis.** Por esta razón, ante la detección de valores atípicos, lo adecuado no es eliminarlos automáticamente, sino preguntarse:

- ¿Es un error de medición o de carga?

- ¿Es un valor posible dentro del fenómeno estudiado?

- ¿El objetivo del análisis justifica conservarlo o tratarlo de manera especial?

**¿Cuándo puede justificarse excluir un outlier?**

Si bien los valores atípicos no deben eliminarse automáticamente, existen situaciones en las que su exclusión puede estar justificada. En particular, cuando hay evidencia de que el valor:

- corresponde a un error de carga o digitación (por ejemplo, un cero de más en un ingreso),

- es físicamente imposible (edad negativa, 500 horas de actividad física en una semana, etc.),

- proviene de una falla de medición documentada,

- o no pertenece realmente a la población objetivo del estudio (error de clasificación).

**En estos casos, la exclusión no se basa en que el valor sea “extremo”, sino en que existe una razón sustantiva o técnica para considerarlo inválido.**

También puede ocurrir que, aun siendo correcto, un valor extremo tenga una influencia desproporcionada sobre los valores de ciertas estadísticas, como la media aritmética o las estimaciones de los coeficientes de un modelo estadístico (Unidad 6). En esos casos, en lugar de eliminarlo sin más, es recomendable: analizar el resultado con y sin esa observación, utilizar medidas más robustas (como la mediana) o aplicar métodos estadísticos diseñados para reducir la influencia de valores extremos.

En cualquier caso, una buena práctica es documentar explícitamente cualquier criterio de exclusión aplicado. La transparencia metodológica es parte central del análisis de datos.

#### Consideraciones importantes sobre el ploteo de los *outliers*

En algunas situaciones puede resultar de interés construir la versión clásica del boxplot, es decir, aquella en la que los *whiskers* se extienden hasta el valor mínimo y máximo observados, sin identificar ni marcar potenciales valores atípicos.

Esto puede ser útil cuando el tamaño del dataset es muy grande. En esos casos, el boxplot modificado puede volverse difícil de leer: una proporción considerable de observaciones puede quedar fuera del criterio $1.5 RI$, generando una gran cantidad de puntos individuales que incluso pueden ocultar la caja.

En **`sns.boxplot()`**, existe el parámetro `showfliers = False`, que evita que se dibujen los potenciales *outliers*. Sin embargo, es importante notar que esta opción no construye el boxplot clásico. El criterio del boxplot modificado se mantiene: los *whiskers* se extienden hasta el valor mínimo y máximo observados que se encuentren dentro del intervalo definido por $Q_1 - 1.5 RI$ y $Q_3 + 1.5 RI$. Lo único que cambia es que las observaciones que quedan fuera de esos límites no se muestran en el gráfico.

En términos gráficos, esto equivale a “recortar” el boxplot y ocultar observaciones que efectivamente existen. Si el objetivo es analizar la distribución completa, esta no es una buena elección.

Una forma más apropiada de construir el boxplot clásico sin eliminar ni ocultar datos es modificar el parámetro **`whis`**, extendiendo los *whiskers* hasta el menor y mayor valor observado:

```python
whis = [0, 100]
```

De esta manera, el gráfico respeta la totalidad de los datos y reproduce el esquema original basado en el resumen de cinco números.

**Ejemplo comparativo**

Trabajemos nuevamente con la variable de minutos semanales de actividad física intensa en Santa Fe.

**1. Boxplot modificado (por defecto)**

```{code-cell} python
plt.figure(figsize = (6,4))

sns.boxplot(x = 'biaf02_m', data = data_stafe)
plt.title("Boxplot modificado (por defecto)")
plt.xlabel("Minutos semanales de actividad física intensa", fontweight = 'bold')
plt.show()
```

Aquí los *whiskers* se cortan según el criterio mencionado anteriormente y los potenciales *outliers* aparecen como puntos.

**2. Ocultando los outliers (¡PELIGRO!)**

```{code-cell} python
plt.figure(figsize = (6,4))

sns.boxplot(x = 'biaf02_m', data = data_stafe, showfliers = False)
plt.title("Boxplot modificado sin mostrar outliers")
plt.xlabel("Minutos semanales de actividad física intensa", fontweight = 'bold')
plt.show()
```

En este caso, los valores extremos siguen fuera del rango de los *whiskers*, pero simplemente no se dibujan. La distribución queda parcialmente “recortada”.

**3. Boxplot clásico (*whiskers* hasta mínimo y máximo)**

```{code-cell} python
plt.figure(figsize = (6,4))

sns.boxplot(x = 'biaf02_m', data = data_stafe, whis = [0,100])
plt.title("Boxplot clásico (whiskers hasta min y max)")
plt.xlabel("Minutos semanales de actividad física intensa", fontweight = 'bold')
plt.show()
```

Aquí los *whiskers* se extienden hasta el mínimo y máximo observados. No se eliminan datos ni se altera la representación de la distribución.


## Tabla de frecuencias

Una tabla de frecuencias constituye una forma sencilla y efectiva de resumir la información contenida en las observaciones de una variable.

### Variables cualitativas

Cuando la variable es cualitativa, la tabla presenta las distintas categorías observadas, el número de veces que aparece cada una (frecuencia absoluta), y, si se desea, su frecuencia relativa o porcentaje sobre el total.

Como ejemplo, trabajemos con la variable `bhcv01` (*tipo de vivienda*). Recordemos que la misma se encuentra codificada numéricamente de la siguiente manera:

1 = Casa

2 = Casilla

3 = Departamento

4 = Pieza de inquilinato

5 = Pieza en hotel o pensión

6 = Local no construido para habitación

7 = Otros

Para que la tabla resulte interpretable, primero conviene recodificar los valores. De esta manera, trabajaremos con etiquetas en lugar de códigos numéricos.

```{code-cell} python

{
    "tags": [
        "remove-output"
    ]
}

# Creamos un diccionario de etiquetas
map_vivienda = {
    1: "Casa",
    2: "Casilla",
    3: "Departamento",
    4: "Pieza de inquilinato",
    5: "Pieza en hotel o pensión",
    6: "Local no construido para habitación",
    7: "Otros"
}

# Creamos una nueva variable que contenga las categorías correspondientes
data["tipo_vivienda"] = data["bhcv01"].map(map_vivienda)
```
Una forma simple de construir la tabla es utilizar `value_counts()`:

```{code-cell} python
data["tipo_vivienda"].value_counts()
```

Pero podemos organizarla mejor:

```{code-cell} python
tabla_vivienda = (data["tipo_vivienda"].value_counts().reset_index())

tabla_vivienda.columns = ["Tipo de vivienda", "Frec_absoluta"]
tabla_vivienda = tabla_vivienda.set_index("Tipo de vivienda")

tabla_vivienda
```
La columna `Frec_absoluta` indica la cantidad de veces que aparece cada categoría en el dataset. Sin embargo, en muchos contextos resulta más informativo expresar estas cantidades en términos relativos, es decir, como proporción o porcentaje sobre el total de observaciones.

Podemos agregar ambas medidas directamente a la misma tabla:

```{code-cell} python
tabla_vivienda["Frec_relativa"] = (
    tabla_vivienda["Frec_absoluta"] /
    tabla_vivienda["Frec_absoluta"].sum()
)

tabla_vivienda["Porcentaje"] = (tabla_vivienda["Frec_relativa"] * 100).round(1)

tabla_vivienda
```

La frecuencia relativa toma valores entre 0 y 1 y representa la proporción de cada categoría respecto del total. El porcentaje no es más que esa misma proporción multiplicada por 100, lo que facilita su interpretación en términos más habituales.

De esta manera, la tabla reúne en un único objeto la información esencial para describir la variable: cuántas veces aparece cada tipo de vivienda y qué peso relativo tiene dentro del conjunto de datos.

A partir de esta tabla podemos construir un **gráfico de barras**, que es la representación gráfica natural de una variable cualitativa:

```{code-cell} python

plt.figure(figsize = (8,5))

sns.barplot(x = 'Porcentaje', y = tabla_vivienda.index, data = tabla_vivienda)

plt.xlabel("Porcentaje (%)", fontweight="bold")
plt.ylabel("Tipo de vivienda", fontweight="bold")
plt.show()
```

El gráfico permite visualizar rápidamente cuál es la categoría más frecuente y cómo se distribuyen las demás, complementando la información numérica de la tabla.

En algunos casos, como en el ejemplo, la distribución puede estar fuertemente concentrada en pocas categorías, mientras que otras presentan frecuencias muy bajas. Esto puede generar gráficos en los que algunas barras resultan prácticamente imperceptibles. 

Frente a esta situación, una estrategia posible consiste en agrupar las categorías menos frecuentes en una nueva categoría, por ejemplo “Otros tipos”, con el objetivo de facilitar la visualización. Sin embargo, esta decisión no es neutra: implica modificar la estructura original de la variable. Por lo tanto, su utilización debe estar justificada por el objetivo del análisis.

Si el propósito es describir con precisión la distribución original, conviene conservar todas las categorías. Si el objetivo es comunicar tendencias generales o simplificar la presentación, puede ser razonable agrupar aquellas con muy baja frecuencia.

En cualquier caso, la recodificación debe explicitarse y documentarse, como se muestra a continuación:

```{code-cell} python
{
    "tags": [
        "remove-output"
    ]
}

# Identificamos categorías con menos del 3% de frecuencia en la tabla original
categorias_principales = tabla_vivienda[
    tabla_vivienda["Porcentaje"] >= 3
].index

# Unificamos las categorías menos frecuentes en "Otros tipos"
data["tipo_vivienda_agrupada"] = data["tipo_vivienda"].apply(
    lambda x: x if x in categorias_principales else "Otros tipos"
)
```

La tabla resultante se vería así:

```{code-cell} python
tabla_vivienda_resumida = (data["tipo_vivienda_agrupada"].value_counts().reset_index())

tabla_vivienda_resumida.columns = ["Tipo de vivienda", "Frec_absoluta"]
tabla_vivienda_resumida = tabla_vivienda_resumida.set_index("Tipo de vivienda")

# Agregamos columna con Porcentajes
tabla_vivienda_resumida["Porcentaje"] = (
    tabla_vivienda_resumida["Frec_absoluta"]*100/
    tabla_vivienda["Frec_absoluta"].sum()
).round(1)

tabla_vivienda_resumida
```
El gráfico de barras ahora luciría así:

```{code-cell} python
plt.figure(figsize = (8,5))

sns.barplot(x = 'Porcentaje', y = tabla_vivienda_resumida.index, hue = tabla_vivienda_resumida.index, data = tabla_vivienda_resumida)

plt.xlabel("Porcentaje (%)", fontweight="bold")
plt.ylabel("Tipo de vivienda", fontweight="bold")
plt.show()
```

Es importante señalar que dentro de la categoría *Otros tipos* se agrupan diversas modalidades de vivienda originalmente diferenciadas (pieza de inquilinato, pieza en hotel o pensión, local no construido para habitación, entre otras). Esta decisión mejora la legibilidad del gráfico, pero reduce el nivel de detalle disponible en la descripción de la variable.

### Variables cuantitativas discretas

Cuando trabajamos con variables cuantitativas discretas, los valores que puede tomar la variable son numéricos y separados entre sí. En muchos casos —como ocurre con la edad medida en años cumplidos, el número de ambientes o de miembros de un hogar o el número de días por semana que una persona realiza actividad física— la variable puede asumir varios valores posibles, pero estos tienen la característica de ser numerables.

Si el número de observaciones es grande, pero la variable presenta un conjunto acotado de valores distintos (como muchas veces ocurre cuando trabajamos con datos de variables cuantitativas discretas), es posible construir una tabla de frecuencias similar a la utilizada para variables cualitativas. En este caso, cada fila de la tabla representa un valor numérico posible y la frecuencia absoluta indica cuántas viviendas presentan ese número de miembros.

En nuestro conjunto de datos contamos con la variable `cant_componentes`, que registra la cantidad de miembros del hogar encuestado. Podemos resumir su distribución mediante una tabla de frecuencias de la siguiente manera:

```{code-cell} python

# Tabla de frecuencias para la cantidad de miembros del hogar
tabla_miembros = (data['cant_componentes'].value_counts().sort_index().to_frame())

tabla_miembros.index.name = 'Num_miembros'
tabla_miembros.columns = ['Frec_absoluta']

tabla_miembros
```

En este caso, cada fila corresponde a un número específico de miembros del hogar y la columna `Frec_absoluta` indica cuántas viviendas encuestadas tienen ese número de miembros dentro del conjunto de datos. 

Dado que se trata de una variable cuantitativa discreta, la representación gráfica adecuada es un **gráfico de bastones**. 

Podemos construirlo utilizando el DataFrame anterior y la función `plt.bar()`. Si fijamos un ancho suficientemente pequeño (por ejemplo, `width = 0.4`), las barras adquieren la forma visual de bastones:

```{code-cell} python
plt.figure(figsize=(8,5))

plt.bar(tabla_miembros['Num_miembros'], tabla_miembros['Frec_absoluta'], width = 0.4)

plt.xlabel("Cantidad de miembros del hogar", fontweight = "bold")
plt.ylabel("Frecuencia absoluta", fontweight = "bold")

plt.xlim((0,12))
plt.xticks(np.arange(0,11,1))

plt.show()
```

El gráfico permite visualizar que la distribución de cantidad de miembros de las viviendas encuestadas es asimétrica hacia la derecha. Se observa que la mayor parte de los hogares tiene pocos miembros, mientras que los hogares con muchos integrantes son relativamente menos frecuentes.

### Variables cuantitativas continuas

Cuando trabajamos con variables cuantitativas continuas, la situación es diferente a la de las variables discretas. Estas variables pueden tomar, al menos en teoría, infinitos valores dentro de un intervalo.

Un ejemplo típico es el ingreso mensual del hogar. Desde el punto de vista conceptual, el ingreso es una magnitud continua: podría medirse con el nivel de precisión que quisiéramos (pesos, centavos, fracciones de centavo, etc.). Sin embargo, en los datos suele registrarse en unidades enteras (por ejemplo, en pesos, como ocurre en la ENFR), lo que hace que los valores observados aparezcan discretizados.

Es importante distinguir entonces entre:

- la naturaleza teórica de la variable (continua) 

- y la forma en que fue medida o registrada en el dataset.

Aunque en nuestra base de datos el ingreso figure sin decimales, **puede asumir un gran número de valores diferentes**. Si intentáramos construir una tabla de frecuencias considerando cada valor individual observado, obtendríamos una tabla muy extensa y poco informativa.

Por ello, en lugar de trabajar con valores puntuales, **realizamos un agrupamiento en subintervalos (segmentación)**.

Lo primero es obtener un resumen descriptivo:

```{code-cell} python
data['bhih01'].describe()
```

Este resumen nos permite conocer el rango en el que se encuentran las observaciones de la variable, su dispersión en términos de la desviación estándar y algunos percentiles relevantes. A partir de esta información podemos decidir cómo construir los intervalos.

Dado que los ingresos totales mensuales por hogar se encuentran aproximadamente en el rango de 0 a 350000 pesos, podríamos agruparlos en intervalos de igual amplitud, por ejemplo de 35000 pesos:

- [0, 35000)

- [35000, 70000)

- [70000, 105000)

- [105000, 140000)

- [140000, 175000)

- [175000, 210000)

- [210000, 245000)

- [245000, 280000)

- [280000, 315000)

- [315000, 350000)

[350000, 385000)

Estos intervalos son contiguos y no se superponen. La notación [a, b) indica que el intervalo incluye el límite inferior pero excluye el superior.

Para generarlos utilizamos **`pd.cut()`**:

```{code-cell} python

# Generamos los subintervalos

bins_ingresos = np.arange(0, 390000, 35000)

data['ingreso_seg'] = pd.cut(data['bhih01'], bins = bins_ingresos, right = False)

data['ingreso_seg'].head()
```

A partir de esta segmentación construimos la tabla de frecuencias:

```{code-cell} python
tabla_ingresos = data['ingreso_seg'].value_counts().sort_index()

tabla_ingresos
```

Formateamos la tabla:

```{code-cell} python
tabla_ingresos = tabla_ingresos.reset_index()
tabla_ingresos.rename(columns = {'ingreso_seg':'Ingreso', 'count':'Frec_absoluta'}, inplace = True)
tabla_ingresos = tabla_ingresos.set_index('Ingreso')

tabla_ingresos
```

Agregamos la frecuencia relativa correspondiente a cada subintervalo:

```{code-cell} python

tabla_ingresos['Frec_relativa'] = (
    tabla_ingresos['Frec_absoluta'] /
    tabla_ingresos['Frec_absoluta'].sum()
).round(5)

tabla_ingresos
```

Y las frecuencias acumuladas:

```{code-cell} python
tabla_ingresos['Frec_absoluta_cum'] = (tabla_ingresos['Frec_absoluta'].cumsum())

tabla_ingresos['Frec_relativa_cum'] = (tabla_ingresos['Frec_relativa'].cumsum())

tabla_ingresos
```

Recordemos:

La **frecuencia absoluta acumulada** indica cuántos hogares tienen ingresos menores al límite superior del intervalo correspondiente.

La **frecuencia relativa acumulada** expresa esa misma cantidad en proporción al total de observaciones.

Este tipo de tabla nos permite resumir la distribución sin trabajar con cada valor individual.

**Interpretación de una fila de la tabla**

Interpretemos a continuación cada una de las frecuencias que figuran en la fila de la tabla correspondiente al subintervalo [35000, 70000):

> ***893 hogares presentan un ingreso total mensual mayor o igual a \$35000 y menor a \$70000, lo que representa el 21.95 % de las viviendas encuestadas.***

Las frecuencias acumuladas hasta ese intervalo nos permiten observar que:

> ***3817 hogares presentan un ingreso total mensual menor a los \$70000, lo que representa el 93.83 % de las viviendas encuestadas. En consecuencia, sólo el 6.17 % de los hogares relevado tiene ingresos totales mensuales de \$70000 o más.*** 

#### Histograma de frecuencias

La representación gráfica más adecuada para variables cuantitativas continuas es el **histograma**. A diferencia del gráfico de bastones utilizado para variables discretas:

- Aquí se representan rectángulos contiguos, cuya base abarca cada subintervalo.

- El área del rectángulo representa la frecuencia correspondiente a cada subintervalo. Cuando los intervalos tienen igual amplitud (como en este caso), la altura de la barra es directamente proporcional a la frecuencia.

Podemos construirlo directamente a partir de la variable original:

```{code-cell} python
plt.figure(figsize=(8,5))

sns.histplot(x = 'bhih01', bins = bins_ingresos, color = 'lightgreen', edgecolor = 'black', data = data)

plt.xlabel("Ingreso mensual del hogar (en pesos)", fontweight = "bold")
plt.xticks(np.arange(0, 380000, 70000))
plt.ylabel("Frecuencia absoluta", fontweight = "bold")

plt.show()
```

El histograma permite visualizar la forma general de la distribución: concentración de valores, asimetría y comportamiento en las colas.

En este caso, se observa que la distribución es asimétrica hacia la derecha: una gran proporción de hogares se concentra en niveles de ingreso bajos o medios, mientras que un número relativamente pequeño de hogares presenta ingresos elevados.

#### Sobre la elección del número de intervalos

La elección del número de intervalos (bins) no es arbitraria. Si utilizamos muy pocos intervalos, perdemos detalle. Si utilizamos demasiados, el gráfico se vuelve ruidoso y difícil de interpretar.

Cuando el tamaño del dataset es grande, suele ser conveniente aumentar la cantidad de intervalos. Por ejemplo, si utilizamos 50 bins:

```{code-cell} python

plt.figure(figsize=(8,5))

sns.histplot(x = 'bhih01', bins = 50, color = 'lightgreen', edgecolor = 'black', data = data)

plt.xlabel("Ingreso mensual del hogar (en pesos)", fontweight="bold")
plt.ylabel("Frecuencia", fontweight="bold")

plt.show()
```

En este caso, se aprecia mayor detalle en la estructura de la distribución. Por ejemplo, puede observarse que el intervalo modal no se encuentra exactamente en el tramo inicial y que aparecen máximos locales adicionales en determinados rangos de ingreso.

## Métricas de correlación

La correlación es una medida estadística que describe el grado en que dos variables cambian o “varían” conjuntamente.

### Covarianza

La covarianza es una medida estadística que brinda información acerca del grado en que dos variables varían linealmente en forma conjunta. Suponiendo que se cuenta con un conjunto de n pares de observaciones de dos variables, X e Y, se calcula de la siguiente forma:

$$
S_{xy} = \frac{1}{n-1}\sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})
$$

Analicemos la ecuación anterior:

- Si la relación entre ambas variables es directa (a mayores valores de una variable le corresponden, en general, mayores valores de la otra, y viceversa), el producto $(x_i - \bar{x})(y_i - \bar{y})$ tenderá a ser positivo, dando como resultado una covariancia positiva.
- Si, por el contrario, la relación entre ambas variables es inversa (a mayores valores de una variable le corresponden, en general, menores valores de la otra), el producto $(x_i - \bar{x})(y_i - \bar{y})$ tenderá a ser negativo, dando como resultado una covariancia negativa.

De esta forma, el **signo** de la covariancia indica si la asociación entre las variables X e Y es positiva o negativa. Un ejemplo podría ser precio de chocolate ($Y$) y porcentaje de cacao ($X$). Probablemente, si estudiamos diferentes marcas de chocolate en el mercado y registramos el precio de venta por gramo y el porcentaje de cacao, vamos a observar que a medida que el porcentaje de cacao aumenta, también lo hace el precio. Por lo tanto, si a partir de los datos recolectados calculáramos la covariancia, esperaríamos obtener un valor positivo de la misma.

![positive.png](./imagenes/positive.png)

![negative.png](./imagenes/negative.png)

![cero.png](./imagenes/cero.png)

Cuando $X$ e $Y$ son estadísticamente independientes, se puede demostrar que la covariancia es igual a 0. Sin embargo, lo opuesto no es necesariamente cierto: 2 variables pueden tener una covariancia igual a 0 y aún así no ser estadísticamente independientes. En este caso, $X$ e $Y$ podrían tener una relación con una forma distinta a la lineal y, de esta forma, no ser independientes.

La fórmula de la covarianza  (población) se define con la siguiente fórmula:

$$
COV(X, Y) = E[(X - E[X])(Y - E[Y])]
$$

Donde: 

$E[X], E[Y]$ : es el valor esperado de la variable $X, Y$respectivamente. Es decir, la media poblacional.

 

<aside>
💡 Las $E$ que vemos en la ecuación de arriba indican que se calcula el valor esperado de una variable aleatoria. El tema variables aleatorias se desarrolla con profundidad en PyE.

</aside>

A continuación vamos a desglosar la fórmula de arriba y vamos a desarrollar algunas intuiciones sobre la misma. 

$(X - E[X])$ calcula la distancia entre cada valor $X$y la media de $X$

$(Y - E[Y])$ calcula la distancia entre cada valor de $Y$y la media de $Y$

- Si, la distancia de $X$e $Y$a sus respectivas medias es “grande” y multiplicamos estas distancias, vamos a obtener un valor “grande”.
- Si ambas variables se alejan de sus medias en la misma dirección, es decir ambas son mayores o menores que las mismas, la multiplicación va a ser positiva. Pero, si se alejan en sentidos opuestos, la multiplicación va a ser negativa.
- Para que la covarianza adquiera un valor que se aleja de 0, ya sea positivo o negativo, la multiplicación de las distancias de la mayoría de los valores que asumen $X, Y$ deben estar en el mismo sentido. Si obtenemos multiplicaciones positivas y negativas balanceadas, el valor esperado de eso va a ser cero.
- Si $X,Y$se alejan “poco” de sus medias, no vamos a tener mucha variación de por si y la covarianza va a ser “chica”

**Limitaciones de la covarianza**

Aunque la covarianza es útil para identificar tendencias y relaciones en los datos, también presenta algunas debilidades y limitaciones:

1. Escala de medida: La covarianza depende de las unidades de medida de las variables, lo que puede dificultar la interpretación y comparación de los valores de covarianza entre diferentes pares de variables. Por ejemplo, si las unidades de medida de una variable son metros y las de la otra variable son grados Celsius, la covarianza entre estas dos variables será en unidades de metros por grados Celsius, lo que no es fácilmente interpretable.
2. Sensibilidad a cambios de escala: La covarianza es sensible a cambios de escala en las variables. Si una de las variables se multiplica por una constante, la covarianza también se multiplicará por esa constante, lo que podría dar lugar a interpretaciones incorrectas de la relación entre las variables.
3. Falta de normalización: A diferencia del coeficiente de correlación de Pearson, la covarianza no está normalizada en un rango de -1 a 1. Esto dificulta la comparación de la fuerza de las relaciones entre diferentes pares de variables, ya que no hay un valor máximo o mínimo absoluto para la covarianza.
4. Interpretación de la fuerza de la relación: La covarianza no proporciona información directa sobre la fuerza de la relación entre las variables. Aunque una covarianza positiva indica una relación directa y una covarianza negativa indica una relación inversa, no se puede deducir cuán fuerte es esa relación a partir del valor de la covarianza en sí.
5. Relaciones lineales únicamente: La covarianza solo puede capturar relaciones lineales entre las variables. Si existe una relación no lineal (por ejemplo, cuadrática o exponencial) entre las variables, la covarianza no será un indicador adecuado de esa relación.

### Correlación lineal de Pearson

El coeficiente de correlación lineal de Pearson mide el grado de asociación lineal que existe entre dos variables cuantitativas continuas. Se calcula de la siguiente manera:

$$
r = \frac{S_{xy}}{S_x S_y}
$$

donde $S_{xy}$ es la covariancia muestral entre X e Y, $S_{x}$ es la desviación estándar muestral de la variable X y $S_{y}$ es la desviación estándar muestral de la variable Y.

**Interpretación**

Dado que se trata de una normalización de la covarianza, su valor está siempre comprendido entre -1 (correlación lineal inversa perfecta) y 1 (correlación lineal directa perfecta). 

- Un coeficiente de correlación positivo indica una asociación directa entre las variables (se mueven en la misma dirección). Por el contrario, un coeficiente de correlación negativo es indicativo de una asociación inversa entre las variables (se mueven en direcciones opuestas).
- Cuanto más cercano a 1 sea, en valor absoluto, más intensa es la correlación lineal entre las variables.
- Un valor de r cercano a 0 no es necesariamente una evidencia de la falta de asociación entre las variables, sino sólo de la ausencia de una relación lineal, ya que este coeficiente no dice nada acerca de la intensidad de asociaciones diferentes a las lineales. En estos casos su cálculo e interpretación resultan inadecuados.
- Existe una tendencia a asumir, implícita o explícitamente, que un alto valor de r implica causalidad, lo que constituye una interpretación errónea. El coeficiente de correlación de Pearson es simplemente una medida de la fuerza de la asociación lineal entre las dos variables.

![Ejemplos de correlaciones de variables con el método de Pearson ](./imagenes/Untitled12.png)

Ejemplos de correlaciones de variables con el método de Pearson 

$$
\rho_{X,Y} = {\frac {COV(X,Y)}{\sigma_{X}\sigma_{Y}}}
$$

**Ventajas frente a la covariancia**

1. Al estar acotado entre -1 y 1, el coeficiente de correlación de Pearson proporciona información sobre la fuerza de la asociación lineal entre las dos variables (cuanto más cercano a 1, en valor absoluto, más intensa es la asociación lineal). 
2. Su valor es independiente de las unidades en las que estén medidas las variables y de los cambios de escala en los datos. Si las dos variables están medidas en grados Celsius y las observaciones se convierten a grados Kelvin, el coeficiente de correlación de Pearson calculado en esta nueva situación será idéntico al anterior.
3. El coeficiente de correlación de Pearson es muy fácil de interpretar y de calcular.

**Supuestos y limitaciones**

La correlación de Pearson asume que las variables poseen una distribución normal conjunta y que la relación entre las mismas es lineal. En relación al primer punto, se trata de una exigencia que cobra especial importancia en el contexto de la inferencia estadística. Es generalmente aceptado que el coeficiente de correlación lineal de Pearson puede utilizarse en prácticamente todas las situaciones en las que nos enfrentemos a la necesidad de cuantificar el grado de asociación lineal entre dos variables, independientemente de la forma de la distribución conjunta. Por otra parte, en relación al segundo punto, si existe una asociación no lineal entre las variables (por ejemplo, cuadrática o exponencial), el coeficiente de correlación de Pearson no será un indicador adecuado de esa relación. 

Finalmente, es oportuno destacar que el coeficiente de correlación de Pearson es bastante sensible a la presencia de valores atípicos en las observaciones, los cuales pueden distorsionar enormemente su valor y conducir a conclusiones equivocadas.

En la encuesta de Factores de Riesgo podríamos pensar en estudiar las siguientes correlaciones:

| **X** | **Y** | **Valor** |
| --- | --- | --- |
| Edad | Minutos entrenamiento intenso | - 0.16 |
| Ingresos | Minutos entrenamiento intenso | 0.04 |
| Edad | Minutos de caminata | - 0.03 |
| Ingresos | Edad cuando fumó por primera vez | - 0.03 |

Hay muchas maneras de calcular el coeficiente de variación en Python. Usando pandas podemos hacerlo de la siguiente forma:

```python
# bhch04 es la edad y biaf02_m es la cantidad de minutos entrenados de forma intensa 
# en la semana. El método por default es `pearson`
data.bhch04.corr(data.biaf02_m, method = 'pearson')
```

### Correlación de Spearman

Para poder entender la correlación de Spearman primero necesitamos revisar el ranking

**Ranking**

Un ranking es una lista ordenada de elementos o entidades en función de algún criterio de clasificación específico. El objetivo del ranking es proporcionar una forma de clasificar y comparar los elementos o entidades de interés.

Los rankings se utilizan comúnmente en una variedad de campos, como deportes, negocios, educación, ciencias, entre otros. Por ejemplo, en los deportes, los rankings se utilizan para clasificar a los equipos o jugadores en función de su rendimiento en la temporada actual o en temporadas anteriores. En los negocios, los rankings se utilizan para clasificar a las empresas en función de su tamaño, ingresos, beneficios, entre otros criterios. En la educación, los rankings se utilizan para clasificar a las universidades en función de su reputación, calidad académica, entre otros criterios.

Los rankings pueden basarse en diferentes criterios de clasificación, como puntuaciones, tiempos, ingresos, tamaño, popularidad, entre otros. Es importante tener en cuenta que el criterio de clasificación utilizado para crear un ranking puede afectar la posición de los elementos o entidades en la lista.

Los rankings pueden tener un impacto significativo en la percepción pública y el éxito de los elementos o entidades clasificados. Sin embargo, es importante recordar que los rankings no siempre son una medida precisa o completa del rendimiento o calidad de los elementos o entidades clasificados, y que deben interpretarse con precaución.

El ranking establece una relación entre los valores que asume una variable de forma tal que sabemos el orden de la misma sin que nos importe el valor en si. Por ejemplo, la satisfacción con un producto podría ser rankeada de esta forma

- 😍 : 1
- 😀 : 2
- 😑 : 3
- ☹️ : 4
- 🤬 : 5

Otro ejemplo podría ser rankear la variable `edad` de nuestro ejemplo. Para eso ordenamos las edades y le asignamos el valor 1 a la edad más grande y la cantidad de personas encuestadas al valor más chico. Luego, las edades intermedias reciben el correspondiente rango intermedio. Un ejemplo se vería así:

[18, 20, 22, 30, 34, 35, 40, 47, 50, 51]

[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]

Lo que hicimos arriba es posible porque en nuestro ejemplo las edades no se repiten, lo cual es poco real. Cuando los valores se repiten esto se llama empate y tenemos que buscar una estrategia para asignarles el ranking. Supongamos que tenemos las siguientes edades: 

[18, 20, 22, 22, 30, 34, 35, 35, 35, 40, 47, 50, 51]

A continuación listamos algunas estrategias para rankearlas, [fuente Wikipedia](https://en.wikipedia.org/wiki/Ranking). 

**Ranking de competición estándar**

En la clasificación de competición, los elementos que se comparan y resultan iguales reciben el mismo número de clasificación y luego se deja un espacio en los números de clasificación. La cantidad de números de clasificación que se omiten en este espacio es uno menos que la cantidad de elementos que se compararon como iguales. De manera equivalente, el número de clasificación de cada elemento es 1 más la cantidad de elementos clasificados por encima de él. Esta estrategia de clasificación se adopta frecuentemente en competencias, ya que significa que si dos (o más) competidores empatan en una posición en la clasificación, la posición de todos aquellos clasificados por debajo de ellos no se ve afectada (es decir, un competidor solo queda en segundo lugar si exactamente una persona obtiene una puntuación mejor que ellos, tercero si exactamente dos personas obtienen una puntuación mejor que ellos, cuarto si exactamente tres personas obtienen una puntuación mejor que ellos, etc.).

En nuestro ejemplo, obtendríamos este ranking: 

[1, 2, 3, 3, 5, 6, 7, 7, 7, 10, 11, 12, 13]

**Ranking de competición modificado**

A veces, la clasificación de competición se realiza dejando los espacios en los números de clasificación antes de los conjuntos de elementos con clasificación igual (en lugar de después de ellos, como en la clasificación de competición estándar). La cantidad de números de clasificación que se omiten en este espacio sigue siendo uno menos que la cantidad de elementos que se compararon como iguales. De manera equivalente, el número de clasificación de cada elemento es igual a la cantidad de elementos clasificados iguales o por encima de él. Esta clasificación garantiza que un competidor solo queda en segundo lugar si obtiene una puntuación más alta que todos menos uno de sus oponentes, tercero si obtiene una puntuación más alta que todos menos dos de sus oponentes, etc. El ranking nos quedaría de este modo: 

[1, 2, 4, 4, 5, 6, 9, 9, 9, 10, 11, 12, 13]

**Ranking denso**

En la clasificación densa, los elementos que se comparan de manera igual reciben el mismo número de clasificación y los siguientes elementos reciben el número de clasificación inmediatamente posterior. De manera equivalente, el número de clasificación de cada elemento es 1 más la cantidad de elementos clasificados por encima de él que son distintos con respecto al orden de clasificación. El ranking nos quedaría de este modo: 

[1, 2, 3, 3, 4, 5, 6, 6, 6, 7, 8, 9, 10]

**Ranking ordinal**

En la clasificación ordinal, todos los elementos reciben números ordinales distintos, incluidos los elementos que se comparan como iguales. La asignación de números ordinales distintos a elementos que se comparan igual se puede hacer de manera aleatoria o arbitraria, pero generalmente es preferible utilizar un sistema que sea arbitrario pero consistente, ya que esto proporciona resultados estables si la clasificación se realiza varias veces. Un ejemplo de un sistema arbitrario pero consistente sería incorporar otros atributos al orden de clasificación (como el orden alfabético del nombre del competidor) para garantizar que no haya dos elementos que coincidan exactamente.
El ranking nos quedaría de este modo:

[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]

**Ranking fraccional**

Los elementos que se comparan igual reciben el mismo número de clasificación, que es la media de lo que tendrían bajo las clasificaciones ordinales; de manera equivalente, el número de clasificación de 1 más la cantidad de elementos clasificados por encima de él más la mitad de la cantidad de elementos iguales a él. Esta estrategia tiene la propiedad de que la suma de los números de clasificación es la misma que en la clasificación ordinal. El ranking nos quedaría de este modo:
[1, 2, 3.5, 3.5, 5, 6, 8, 8, 8, 10, 11, 12, 13]

Con `pandas`, podemos rankear una serie con diferentes métodos de ranking: 

```python
import pandas as pd

data = [18, 20, 22, 22, 30, 34, 35, 35, 35, 40, 47, 50, 51]
serie = pd.Series(data)

# min: **Ranking de competición estándar**
# max: **Ranking de competición modificado**
# dense: **Ranking denso**
# average: **Ranking fraccional**
# first: **Ranking ordinal**
ranking_methods = ['average', 'min', 'max', 'dense', 'first']

for method in ranking_methods:
    ranked = serie.rank(method=method)
    print(f"Método de Ranking: {method}")
    print(ranked.tolist())
    print("\n")

"""
Ranking method: average
[1.0, 2.0, 3.5, 3.5, 5.0, 6.0, 8.0, 8.0, 8.0, 10.0, 11.0, 12.0, 13.0]

Ranking method: min
[1.0, 2.0, 3.0, 3.0, 5.0, 6.0, 7.0, 7.0, 7.0, 10.0, 11.0, 12.0, 13.0]

Ranking method: max
[1.0, 2.0, 4.0, 4.0, 5.0, 6.0, 9.0, 9.0, 9.0, 10.0, 11.0, 12.0, 13.0]

Ranking method: dense
[1.0, 2.0, 3.0, 3.0, 4.0, 5.0, 6.0, 6.0, 6.0, 7.0, 8.0, 9.0, 10.0]

Ranking method: first
[1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0, 8.0, 9.0, 10.0, 11.0, 12.0, 13.0]
"""
```

**Correlación de Spearman**

La correlación de Spearman se define como el coeficiente de correlación de Pearson para las variables rankeadas. Esto nos permite evaluar si existe una relación monotónica entre dos variables, es decir si crecen/decrecen juntas, sin importar si la relación es lineal o no, a diferencia de Pearson. Además el valor obtenido es menos sensible a valores atípicos que el coeficiente de Pearson. 

La fórmula general es: 

$$
\rho = {\frac{COV(R(X), R(Y))}{\sigma_{R(X)}\sigma_{R(Y)}}}
$$

La correlación de Spearman tiene una relación directa con los rankings (rangos) porque se basa en los rangos de los datos en lugar de sus valores originales. Al calcular la correlación de Spearman, convertimos los valores de las dos variables en rangos antes de evaluar la asociación entre ellas. Esta característica hace que la correlación de Spearman sea especialmente útil para analizar datos ordinales, donde la información más importante es el orden o la clasificación de las observaciones en lugar de sus valores numéricos exactos.

Cuando comparamos dos conjuntos de rankings, la correlación de Spearman nos indica qué tan bien estos rankings concuerdan entre sí. Si los rankings de las dos variables son idénticos, la correlación de Spearman será 1, lo que indica una relación monotónica creciente perfecta. Si los rankings son exactamente opuestos, la correlación de Spearman será -1, lo que indica una relación monotónica decreciente perfecta. Un valor cercano a 0 sugiere que no hay una relación monotónica evidente entre los rankings de las dos variables.

![Ejemplos de variables monotónicas y no monotónicas](./imagenes/Untitled13.png)

Ejemplos de variables monotónicas y no monotónicas

Antes de calcular la correlación de Spearman, debemos convertir los valores de las variables en rangos. Aquí se detallan los pasos para convertir los datos en rangos:

1. Ordenar los valores de cada variable (X e Y) en orden ascendente, de menor a mayor.
2. Asignar un rango a cada valor en función de su posición en la lista ordenada. El valor más pequeño recibirá el rango 1, el siguiente valor más pequeño recibirá el rango 2 y así sucesivamente.
3. En caso de empates (valores iguales), utilizar el método de ranking ordinal.

Repita estos pasos para ambas variables, X e Y. Al final, tendremos dos conjuntos de datos transformados en rangos, que se pueden utilizar para calcular el coeficiente de correlación de rango de Spearman (ρ).

Aquí hay un ejemplo para ilustrar el proceso:

Suponga que tiene dos variables, X e Y, con los siguientes valores:

X: [10, 25, 30, 45, 55]
Y: [2, 8, 10, 12, 22]

Primero, ordene los valores de cada variable y asígneles rangos:

X: 10 (R1 = 1), 25 (R2 = 2), 30 (R3 = 3), 45 (R4 = 4), 55 (R5 = 5)
Y: 2 (R1 = 1), 8 (R2 = 2), 10 (R3 = 3), 12 (R4 = 4), 22 (R5 = 5)

Luego la fórmula es:

ρ = 1 - (6 * Σd^2) / (n * (n^2 - 1))

$$
\rho = \frac{1-(6\times \sum d^2)}{(n\times(n^2-1)}
$$

Donde:

- n es el número de observaciones (pares de datos)
- Σd^2 es la suma de las diferencias al cuadrado de los rangos.

El coeficiente de correlación de Spearman (ρ) varía entre -1 y 1, donde:

- ρ = 1 indica una relación monotónica creciente perfecta.
- ρ = -1 indica una relación monotónica decreciente perfecta.
- ρ ≈ 0 sugiere que no hay una relación monotónica evidente entre las variables.

Si calculamos el coeficiente de Spearman con Pandas, la librería se encarga de facilitar el proceso de generación de rankings. El coeficiente se puede calcular de la misma forma que el coeficiente de correlación de Pearson pero cambiando el método

```python
data.bhch04.corr(data.biaf02_m, method = 'spearman')
```

Aquí vemos el valor del coeficiente con diferentes funciones como ejemplo ([fuente](https://stackabuse.com/calculating-spearmans-rank-correlation-coefficient-in-python-with-pandas/)):

![Untitled](./imagenes/Untitled14.png)

![Untitled](./imagenes/Untitled15.png)

![Untitled](./imagenes/Untitled16.png)

## Matriz de Covarianza

La matriz de covarianza es una matriz cuadrada donde se muestra el coeficiente de covarianza entre múltiples variables. En el ejemplo de abajo mostramos la matriz de covarianza entre edad (bhch04), minutos semanales de actividad intensa (biaf02_m), ingreso (bhih01), minutos de caminata semanal (biaf06_m) y edad en que comenzó a fumar (bita02).

![Untitled](./imagenes/Untitled17.png)

Esta matriz nos permite ver de forma rápida si existe alguna relación lineal entre las variables y nos puede servir en el futuro como guía para selección de atributos para nuestros modelos. 

Para construir la tabla de arriba usamos Pandas de la siguiente forma:

```python
data[['bhch04', 'biaf02_m', 'bhih01', 'biaf06_m', 'bita02']].cov()
```

## Matriz de Correlación

La matriz de correlación es similar a la anterior pero en lugar de mostrar el coeficiente de covarianza nos muestra el de correlación. 

```python
#en este caso el método por default es el de pearson
data[['bhch04', 'biaf02_m', 'bhih01', 'biaf06_m', 'bita02']].corr()
```

![Untitled](./imagenes/Untitled18.png)

La diagonal siempre tiene el valor de 1 puesto que es la correlación de una variable consigo misma. Tanto en esta matriz como en la anterior, solo nos interesa mirar la parte que está por encima o debajo de la diagonal, puesto que estas matrices son siempre simétricas. 

Una forma de visualizar rápidamente la correlación que existe entre un grupo de variables es  imprimiendo la matriz de correlación con colores:

```python
sns.heatmap(data[['bhch04', 'biaf02_m', 'bhih01', 'biaf06_m', 'bita02']].corr(), annot=True)
```

![Untitled](./imagenes/Untitled19.png)

## Temas avanzados

## Métricas de Distancia y Similaridad

Son métricas que nos a permiten definir cuánto se parecen dos puntos de datos, de forma similar a lo que vimos en la Unidad 2 con la similaridad de Levensthein on Jaro-Winkler. Existen diferentes formas de medir la similaridad y la selección de la misma depende del análisis que se esté realizando. A continuación listamos algunas de ellas.

### **Distancia Euclideana**

La definición formal es que la distancia euclideana es la distancia entre dos puntos en un [espacio euclídeo](https://es.wikipedia.org/wiki/Espacio_eucl%C3%ADdeo). La forma de visualizar esto es pensarlo como la distancia en linea recta entre dos puntos en el espacio. 

Si hablamos de un espacio de n dimensiones y queremos calcular la distancia entre los puntos P y Q, entonces:

$$
d_{E(P,Q)}= \sqrt{\sum_{i = 1}^{n}(p_i - q_i)^2}
$$

![Untitled](./imagenes/Untitled20.png)

**Desventajas**
Aunque es una medida de distancia común, la distancia euclidiana no es una escala invariante, lo que significa que las distancias calculadas pueden estar sesgadas según las unidades de las entidades. Por lo general, uno necesita normalizar los datos antes de usar esta medida de distancia.

Además, a medida que aumenta la dimensionalidad de sus datos, la distancia euclidiana se vuelve menos útil. 

**Casos de uso**
La distancia euclidiana funciona muy bien cuando tiene datos de baja dimensión y es importante medir la magnitud de los vectores. Métodos como [kNN](https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm) y [HDBSCAN](https://en.wikipedia.org/wiki/DBSCAN#Extensions) muestran excelentes resultados listos para usar si se usa la distancia euclidiana en datos de baja dimensión.

### **Distancia Manhattan**

La distancia de Manhattan se calcula como la suma de la diferencia absoluta entre los componentes de dos vectores P y Q 

$$
d_{M(P,Q)} = \sum_{i = 1}^{n}|p_i - q_i|
$$

La distancia de Manhattan es la norma L1 de un vector mientras que la euclideana es la norma L2. 

![Distancia Manhattan contra distancia Euclidiana: Las líneas roja, azul y amarilla tienen la misma longitud (12) en las geometrías Euclidiana y taxicab. En la geometría Euclidiana, la línea verde tiene longitud 6×√2 ≈ 8.48, y es el único camino más corto. En la geometría taxicab, la línea verde tiene longitud 12, por lo que no es más corta que los otros caminos.](./imagenes/Untitled21.png)

Distancia Manhattan contra distancia Euclidiana: Las líneas roja, azul y amarilla tienen la misma longitud (12) en las geometrías Euclidiana y taxicab. En la geometría Euclidiana, la línea verde tiene longitud 6×√2 ≈ 8.48, y es el único camino más corto. En la geometría taxicab, la línea verde tiene longitud 12, por lo que no es más corta que los otros caminos.

**Desventajas**
Aunque la distancia de Manhattan parece funcionar bien para datos de alta dimensión, es una medida algo menos intuitiva que la distancia euclidiana, especialmente cuando se usa en datos de alta dimensión.

Además, es más probable que proporcione un valor de distancia más alto que la distancia euclidiana, ya que no ofrece el camino más corto posible. Esto no necesariamente da problemas, pero es algo que debe tener en cuenta.

**Casos de uso**
Cuando su conjunto de datos tiene atributos discretos y/o binarios, Manhattan parece funcionar bastante bien, ya que tiene en cuenta las rutas que, de manera realista, podrían tomarse dentro de los valores de esos atributos. Tomar la distancia euclidiana, por ejemplo, crearía una línea recta entre dos vectores cuando en realidad esto podría no ser posible.

### **Similaridad de Coseno**

Mide el ángulo entre dos vectores en un espacio de varias dimensiones. Es una métrica popular para la comparación de documentos en procesamiento de lenguaje natural y sistemas de recomendación.

Sabemos que el producto punto entre dos vectores es igual al coseno del ángulo entre ellos por la longitud de los mismos

$$
u.v = cos(\theta) |u||v|
$$

Con esto se define la similitud coseno de la siguiente forma

$$
cos(\theta) = \frac{u.v}{|u||v|}
$$

Mirando la gráfica de la función coseno podemos decir que:

- Cuando $\theta$ es igual a 0, el coseno es igual a 1. En nuestro caso, los dos vectores coinciden perfectamente
- Cuando $\theta$ es igual a $\pi/2$ , el coseno es igual a 0. En nuestro caso, los dos vectores son ortogonales
- Cuando $\theta$ es igual a $\pi$, el coseno es igual a -1. En nuestro caso, los dos vectores se encuentran en posición diametralmente opuesta

![Untitled](./imagenes/Untitled22.png)

<aside>
💡 Notar que la magnitud de los vectores no entra en juego para esta similaridad

</aside>

![Untitled](./imagenes/Untitled23.png)

**Desventajas**
Una de las principales desventajas de la similitud del coseno es que no se tiene en cuenta la magnitud de los vectores, sino simplemente su dirección. En la práctica, esto significa que las diferencias de valores no se tienen plenamente en cuenta. Si toma un sistema de recomendación, por ejemplo, la similitud del coseno no tiene en cuenta la diferencia en la escala de calificación entre diferentes usuarios.

**Casos de uso**
Usamos la similitud del coseno a menudo cuando tenemos datos de alta dimensión y cuando la magnitud de los vectores no es importante. Para los análisis de texto, esta medida se utiliza con bastante frecuencia cuando los datos se representan mediante recuentos de palabras. Por ejemplo, cuando una palabra aparece con más frecuencia en un documento que en otro, esto no significa necesariamente que un documento esté más relacionado con esa palabra. Puede darse el caso de que los documentos tengan longitudes desiguales y la magnitud del conteo sea de menor importancia. Entonces, podemos usar mejor la similitud del coseno que no tiene en cuenta la magnitud.

### **Distancia de Mahalanobis**

En estadística, la distancia de Mahalanobis es una medida de distancia introducida por Mahalanobis en 1936. Su utilidad radica en que es una forma de determinar la similitud entre dos variables aleatorias multidimensionales. Se diferencia de la distancia euclídea en que tiene en cuenta la correlación entre las variables aleatorias.

![La distancia de Mahalanobis (MD) es una métrica de distancia efectiva que encuentra la distancia entre el punto y la distribución. Funciona con bastante eficacia en datos multivariados porque utiliza una matriz de covarianza de variables para encontrar la distancia entre los puntos de datos y el centro. Esto significa que MD detecta valores atípicos en función del patrón de distribución de los puntos de datos, a diferencia de la distancia euclidiana.](./imagenes/Untitled24.png)

La distancia de Mahalanobis (MD) es una métrica de distancia efectiva que encuentra la distancia entre el punto y la distribución. Funciona con bastante eficacia en datos multivariados porque utiliza una matriz de covarianza de variables para encontrar la distancia entre los puntos de datos y el centro. Esto significa que MD detecta valores atípicos en función del patrón de distribución de los puntos de datos, a diferencia de la distancia euclidiana.

En la siguientes figuras, el punto negro representa el centroide de la distribución. En la gráfica de la izquierda, tenemos una distribución no correlacionada. En ese caso, los valores atípicos serían los más alejados del centroide y se podría usar la distancia euclidiana para detectarlos. En el caso de la derecha, cuando las variables están correlacionadas, tiene más sentido usar Mahalanobis, ya que tiene en cuenta la correlación entre ambas variables. Allí el punto 2 sería un outlier, mientras que el punto 1 es parte de la distribución. 

![Untitled](./imagenes/Untitled25.png)

La distancia de Mahalanobis mide la distancia entre un punto P y el centroide de una distribución D y es igual a la distancia euclideana cuando las variables no están correlacionadas. 

Se define como:

$$
d_{M(x, \mu, D)} = \sqrt{(x - \mu)^T S^{-1}(x - \mu)}
$$

Y cuando nos interesa saber la distancia entre dos puntos, en vez de un punto y el centroide de los datos:

$$
d_{M(x,y, D)} = \sqrt{(x - y)^T S^{-1}(x - y)}
$$

Donde

$x, y$: puntos en el espacio

$\mu$: media de la distribución D

$S$: matriz de covarianza

Crearemos un conjunto de datos que muestre el puntaje del examen de 20 estudiantes junto con la cantidad de horas que dedicaron a estudiar, la cantidad de exámenes de preparación que tomaron y su calificación actual en el curso::

```python
import numpy as np
import pandas as pd 
import scipy as stats
from scipy.stats import chi2

data = {'score': [91, 93, 72, 87, 86, 73, 68, 87, 78, 99, 95, 76, 84, 96, 76, 80, 83, 84, 73, 74],
        'hours': [16, 6, 3, 1, 2, 3, 2, 5, 2, 5, 2, 3, 4, 3, 3, 3, 4, 3, 4, 4],
        'prep': [3, 4, 0, 3, 4, 0, 1, 2, 1, 2, 3, 3, 3, 2, 2, 2, 3, 3, 2, 2],
        'grade': [70, 88, 80, 83, 88, 84, 78, 94, 90, 93, 89, 82, 95, 94, 81, 93, 93, 90, 89, 89]
        }

df = pd.DataFrame(data,columns=['score', 'hours', 'prep','grade'])

# Crear función para calcular la distancia de Mahalanobis
def mahalanobis(x=None, data=None, cov=None):

    x_mu = x - np.mean(data)
    if not cov:
        cov = np.cov(data.values.T)
    inv_covmat = np.linalg.inv(cov)
    left = np.dot(x_mu, inv_covmat)
    mahal = np.dot(left, x_mu.T)
    return mahal.diagonal()

# Crea una nueva columna en el marco de datos que contiene 
# la distancia de Mahalanobis para cada fila
df['mahalanobis'] = mahalanobis(x=df, data=df[['score', 'hours', 'prep', 'grade']])

# Calcular el valor p para cada distancia mahalanobis
df['p'] = 1 - chi2.cdf(df['mahalanobis'], 3)

# Mostrar valores p para las primeras cinco filas en el marco de datos
df.head()

"""
  score	hours	prep	grade	mahalanobis	p
0	91	16	3	70	16.501963	0.000895
1	93	6	4	88	2.639286	0.450644
2	72	3	0	80	4.850797	0.183054
3	87	1	3	83	5.201261	0.157639
4	86	2	4	88	3.828734	0.280562
"""
```

La interpretación de los valores p es la siguiente:

- Un valor p cercano a 1 indica que la probabilidad de que la observación sea un valor típico dentro de la distribución es alta, por lo que no se considera un valor atípico.
- Un valor p cercano a 0 indica que la probabilidad de que la observación sea un valor típico dentro de la distribución es baja, lo que sugiere que podría ser un valor atípico. Generalmente, se considera que una observación con un valor p inferior a 0.001 es un valor atípico. En este ejemplo, vemos un valor atípico en el primer registro.

**Desventajas**

Suposición de normalidad: La distancia de Mahalanobis supone que los datos siguen una distribución normal multivariante. Si esta suposición no se cumple, la distancia de Mahalanobis podría no ser apropiada o requeriría ajustes en la metodología.

Sensibilidad a la matriz de covarianza: La distancia de Mahalanobis depende de la matriz de covarianza, y el cálculo de su inversa puede ser numéricamente inestable en algunos casos. Esto puede ser un problema en conjuntos de datos de alta dimensionalidad o cuando hay multicolinealidad (variables altamente correlacionadas).

No apropiada para distribuciones no lineales: La distancia de Mahalanobis no es adecuada para conjuntos de datos con relaciones no lineales entre las variables, ya que solo considera las correlaciones lineales.

**Casos de uso**

Detección de valores atípicos: La distancia de Mahalanobis es útil para identificar valores atípicos en un conjunto de datos multivariante, ya que considera la correlación entre las variables y la variabilidad de cada variable. Los puntos con distancias de Mahalanobis grandes pueden ser considerados como valores atípicos.

Clasificación y reconocimiento de patrones: La distancia de Mahalanobis puede utilizarse en problemas de clasificación y reconocimiento de patrones, donde se intenta asignar un objeto desconocido a una de varias clases predefinidas. La distancia de Mahalanobis puede ser utilizada como métrica de similitud para determinar la cercanía entre un objeto desconocido y los objetos en cada clase.

Análisis de conglomerados (clustering): La distancia de Mahalanobis puede ser utilizada en algoritmos de clustering para medir la similitud entre puntos en un espacio multivariante. Algoritmos como K-means o agrupamiento jerárquico pueden utilizar la distancia de Mahalanobis en lugar de la distancia euclidiana para tener en cuenta la correlación entre variables y la variabilidad de cada variable.

### **Distancia de Jaccard**

El coeficiente de similitud de Jaccard o índice de Jaccard mide la similitud entre dos conjuntos de muestras y fue concebido con la intención de comparar los tipos de flores presentes en un ecosistema de la cuenca de un río, con los tipos presentes en las regiones aledañas. Se define como la relación entre el tamaño de la intersección de ambos conjuntos y el tamaño de la unión:

$$
sim_{J(A,B)} = \frac{|A\cap B|}{|A \cup B|}
$$

La *distancia de Jaccard* mide la disimilitud entre dos conjuntos de muestras y se define como el complemento del coeficiente de Jaccard:

$$
J (A,B) = 1 - sim_{J(A,B)}
$$

El coeficiente de Jaccard toma valores entre 0 y 1, siendo 1 la coincidencia perfecta entre ambos conjuntos, cuando la intersección es igual a la unión

Esta distancia puede utilizarse en análisis de texto: La distancia de Jaccard se utiliza para comparar documentos o textos en función de la similitud entre sus conjuntos de palabras, caracteres o n-gramas, o puede ser útil también en sistemas de recomendación, para medir la similitud entre usuarios o elementos en función de los conjuntos de características, preferencias o comportamientos de los usuarios.

**Desventajas**

La distancia de Jaccard puede verse afectada por diferencias significativas en el tamaño de los conjuntos que se comparan. Por ejemplo, si un conjunto es mucho más grande que otro, la distancia de Jaccard puede ser alta incluso si hay una superposición considerable entre los conjuntos.

No apta para datos numéricos continuos: La distancia de Jaccard está diseñada para comparar conjuntos y, por lo tanto, no es adecuada para trabajar con datos numéricos continuos. Para datos continuos, es necesario utilizar otras medidas de distancia, como la distancia euclidiana o la distancia de Mahalanobis.

No considera la estructura interna de los datos: La distancia de Jaccard solo tiene en cuenta la presencia o ausencia de elementos en los conjuntos, pero no considera la estructura interna o las relaciones entre los elementos dentro de los conjuntos. En situaciones donde la estructura interna de los datos es importante, podrían ser más adecuadas otras medidas de similitud o técnicas de análisis.

Sensibilidad a la presencia de elementos irrelevantes: La distancia de Jaccard puede verse afectada por la presencia de elementos irrelevantes o ruidosos en los conjuntos. Estos elementos pueden aumentar la distancia de Jaccard entre dos conjuntos que, de lo contrario, serían muy similares.

**Casos de uso**

La distancia de Jaccard se utiliza a menudo en el análisis de texto para comparar la similitud entre documentos. Por ejemplo, se pueden comparar documentos en función de las palabras o términos que contienen, y luego agruparlos o clasificarlos según su similitud.

Comparación de comunidades biológicas: En ecología y biología, la distancia de Jaccard se utiliza a menudo para comparar la similitud entre comunidades biológicas, como la presencia de especies en diferentes hábitats o ecosistemas. También se puede aplicar en el análisis de datos genéticos para comparar la similitud entre diferentes genomas, especialmente en el contexto de datos de secuenciación de ADN.

Sistemas de recomendación: La distancia de Jaccard puede ser útil en sistemas de recomendación para medir la similitud entre usuarios o elementos en función de sus características binarias o categóricas. Por ejemplo, se puede utilizar para comparar la similitud entre usuarios en función de las películas que han visto o los productos que han comprado. También en el análisis de redes sociales, la distancia de Jaccard se utiliza a menudo para comparar la similitud entre los miembros de una red en función de sus conexiones o intereses compartidos.

### **Distancias entre palabras**

Pueden consultar a la unidad 2 donde estudiamos distancia de Levensthein y de Jaro-Winkler

[**KPI Estándares**](https://www.notion.so/KPI-Est-ndares-86f5927d6c7745b2ae7fdee69d4decee?pvs=21)