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

# Unidad 4 - Análisis exploratorio de datos: Visualizaciones

```{admonition} 📂 Descargar archivos  
[Descargar los archivos para la práctica desde el Campus Virtual](https://campusv.fceia.unr.edu.ar/course/view.php?id=471)
```

### **Ejercicio N°1**

El dataset `iris.csv` contiene información sobre 150 flores de iris de tres especies diferentes: *setosa*, *versicolor* y *virginica*. Para cada flor, se midieron cuatro características: longitud y ancho del sépalo (la parte que rodea y protege el capullo de la flor) y longitud y ancho del pétalo (la parte coloreada de la flor).

1. Reproduzca el histograma mostrado en la siguiente figura para visualizar la distribución del ancho de sépalo (`sepal_width_cm`). 

```{code-cell} python
:tags: [remove-input]
:align: center

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_theme(style = 'ticks')

# Importo dataset
data_iris = pd.read_csv('datasets/iris.csv')

# Hago gráfico
plt.figure(figsize=(8, 6))
sns.histplot(x = 'sepal_width_cm', fill = True, stat = 'percent', color = 'darkred', edgecolor = 'black', data = data_iris, bins = np.arange(2,4.5,0.2))
plt.xlabel('Ancho de sépalo (cm)', fontweight = 'bold', fontsize = 11)
plt.title('Distribución del ancho de sépalo de las flores', fontweight = 'bold')
plt.xticks(np.arange(2,4.5,0.4), fontsize = 10)
plt.yticks(fontsize = 10)
plt.ylabel('% de plantas', fontweight = 'bold', fontsize = 11);
```

**Sugerencias:** 

- Configure previamente el *theme* de Seaborn utilizando `sns.set_theme(style='ticks')`.

- El color de las barras debe ser **darkred** y deben tener borde negro.

- El eje vertical debe expresarse en porcentaje.

- Considere una partición en 12 intervalos de igual amplitud (0.2), cubriendo el rango desde 2.0 hasta 4.4. Puede lograrlo mediante el argumento `bins` y/o `binrange`.

2. Realice un boxplot múltiple que permita comparar la distribución del largo del pétalo de las flores entre las distintas especies. Comente brevemente lo observado. ¿Cuál de las especies presenta una mayor mediana del largo del pétalo?

3. Añada al gráfico anterior una representación de la media del largo del pétalo para cada especie. Para ello, puede utilizar `pointplot()`
o una capa adicional con `stripplot()`/`scatterplot()` a partir de datos agregados. 

**Sugerencia:** para construir estos datos agregados, calcule previamente la media del largo del pétalo para cada especie, obteniendo un *DataFrame* reducido con una fila por especie.

Una vez que tenga el gráfico construido, compare media y mediana en cada especie. ¿Qué indica esto sobre la simetría de las distribuciones?

4. Reproduzca el gráfico en paneles mostrado en la figura, en el que se presentan gráficos de violín para el ancho de sépalo (`sepal_width_cm`) y ancho de pétalo (`petal_width_cm`). Utilice `plt.subplots()` para crear los paneles y personalice cada gráfico en su respectivo eje. Superponga además los datos individuales mediante `stripplot()`.

````{admonition} **Sobre plt.subplots()**
:class: tip

La función **`plt.subplots()`** de Matplotlib crea una figura con una cuadrícula de subgráficos (elemento llamado **`axes`**), permitiendo organizar varios gráficos en una sola figura. El uso básico es el siguiente:

```
fig, axes = plt.subplots(nrows, ncols)
```
Donde **`nrows`** y **`ncols`** especifican el número de filas y columnas de subgráficos. La función devuelve una figura (**`fig`**) y un array de ejes (**`axes`**). Cada subgráfico individual es un objeto de tipo **`Axes`** que se puede personalizar de manera independiente.
````

**Sugerencias:**

- Utilice **lightblue** y **darkorange** como colores para el relleno de los gráficos de violín.

- Elimine los valores del eje vertical para mejorar la presentación.

- Ajuste el parámetro `jitter` en `stripplot()` para evitar la superposición excesiva de puntos.

Analice los gráficos obtenidos. ¿Cuál de las siguientes opciones describe de una **manera más precisa** la forma de cada una de las distribuciones? 

- *Distribución simétrica* 
- *Distribución sesgada a la derecha* 
- *Distribución unimodal* 
- *Distribución bimodal* 
- *Distribución normal* 
- *Distribución sesgada a la izquierda* 
- *Distribución uniforme*

```{code-cell} python
:tags: [remove-input, remove-stderr]
:align: center

# Hago gráfico
fig, axes = plt.subplots(1, 2, figsize=(10,3))
plt.subplots_adjust(hspace = 0.5, wspace = 0.3)

columnas = ['sepal_width_cm', 'petal_width_cm']

sns.violinplot(x = 'sepal_width_cm', color = 'lightblue', fill = True, data = data_iris, ax = axes[0], inner = None, linecolor = 'black', gap = 0.15)
sns.stripplot(x = 'sepal_width_cm', color = 'black', alpha = 0.6, data = data_iris, ax = axes[0], jitter = 0.07, linewidth = 0.7, edgecolor = 'gray')
axes[0].set_xlabel('Ancho de sépalo (cm)', fontweight = 'bold')
axes[0].set_yticks([])
axes[0].set_title('Distribución del ancho de sépalo')
axes[1].set_xlabel('Ancho de pétalo (cm)', fontweight = 'bold')
axes[1].set_title('Distribución del ancho de pétalo')
sns.violinplot(x = 'petal_width_cm', color = 'darkorange', fill = True, data = data_iris, ax = axes[1], cut = 0.3, inner = None, linecolor = 'black', gap = 0.15)
sns.stripplot(x = 'petal_width_cm', color = 'black', alpha = 0.6, data = data_iris, ax = axes[1], jitter = 0.07, linewidth = 0.7, edgecolor = 'gray')
axes[1].set_yticks([]);
```

5. Construya un gráfico que le permita analizar la relación general que existe entre las variables ancho y largo del pétalo. Realice un comentario acerca de lo observado y complemente el gráfico anterior informando una medida de la fuerza y la dirección de la asociación lineal entre ambas variables.

### **Ejercicio N°2**
El dataset `registro_temperatura365d_smn.txt` contiene las temperaturas máximas y mínimas registradas diariamente entre el 11/04/2025 y el 10/04/2026 en todas las estaciones meteorológicas de superficie pertenecientes al Servicio Meteorológico Nacional.

1. Explore la estructura del archivo. Notará que no se utiliza un delimitador particular para separar las distintas columnas sino que los distintos campos están alineados en columnas con diferente número de espacios que separan uno del otro. Por este motivo, y aprovechando que las primeras columnas son de ancho fijo, se sugiere utilizar la función `read_fwf()` de Pandas, que permite leer este tipo de archivos. 

```{admonition} **Sobre read_fwf()**
:class: tip

La función **`pd.read_fwf()`** en Pandas se utiliza para leer archivos de texto que tienen columnas de ancho fijo, donde cada columna ocupa una cantidad específica de caracteres. Esta función es útil cuando los datos no están separados por delimitadores como comas o espacios, sino que están organizados en columnas de longitudes fijas.

Al emplear esta función se deben definir los anchos de las columnas mediante el parámetro **`colspecs`**. Esto se hace proporcionando una lista de tuplas, donde cada tupla indica el rango de posiciones que corresponden a cada columna. Como ayuda, en nuestro caso **`colspecs`** comienza con la tupla (0, 8) para definir los límites de la columna **`FECHA`**, es decir: **`colspecs = [(0, 8), ...]`**.

**`read_fwf()`** ignora los espacios en blanco al cargar los datos, por lo que no es necesario preocuparse por los espacios adicionales que puedan existir.
```

2. Construya una tabla resumen que contenga media, mediana, desviación estándar y rango intercuartílico de las temperaturas mínimas y máximas registradas por mes. 

3. Construya un gráfico que le permita comparar las distribuciones de temperaturas mínimas y máximas diarias entre los últimos 10 meses con datos completos (mayo 2025 a marzo 2026) registradas en la estación del Aeropuerto Rosario (”ROSARIO AERO”).

4. En base a lo realizado en los dos ítems anteriores, responda las siguientes preguntas:

    a. ¿Cuál fue el mes del último año con la mayor temperatura máxima mediana?

    b. ¿Cuál fue el mes del último año con la menor temperatura mínima mediana?

    c. Considerando la variabilidad del 50 % central de las temperaturas registradas en el mes, ¿cuál fue el mes del último año con una menor dispersión tanto en sus temperaturas mínimas como en sus temperaturas máximas? 

    d. ¿Cuál fue el mes del último año que presentó una mayor amplitud en sus temperaturas mínimas registradas?

    e. Considerando los meses del invierno 2025, ¿existió algún mes en el cual se haya registrado una temperatura máxima atípica en relación al resto de los registros de ese mes?
    
5. Realice nuevamente el ítem 3 con los datos correspondientes a la estación meteorológica localizada en la Base Marambio de la Antártida Argentina. Compare los dos gráficos y comente las diferencias que encuentra en las distribuciones de las temperaturas registradas en ambas estaciones.

### **Ejercicio N°3**
El dataset **Penguins** contiene información acerca de un conjunto de pingüinos que habitan el Archipiélago Palmer, un archipiélago del Océano Glacial Antártico que se encuentra conformado por un conjunto de islas montañosas. Sobre cada ejemplar se cuenta con la siguiente información:

- **`species`**: especie a la que pertenece (Chinstrap, Adélie o Gentoo).

- **`culmen_length_mm`**: largo del culmen, cresta superior del pico (mm).

- **`culmen_depth_mm`**: altura del culmen (mm).

- **`flipper_length_mm`**: largo de la aleta (mm).

- **`body_mass_g`**: masa corporal (g).

- **`island`**: nombre de la isla del Archipiélago Palmer en la que habita (Dream, Torgersen o Biscoe).

- **`sex`**: sexo.

El mismo puede importarse al entorno de trabajo utilizando la función **`sns.load_dataset('penguins')`** de Seaborn.

1. Reproduzca el gráfico mostrado en la siguiente figura para visualizar la distribución del largo de la aleta entre las distintas especies de pingüinos. En el mismo se utilizaron colores pertenecientes a la paleta **magma**. 

```{code-cell} python
:tags: [remove-input, remove-stderr]
:align: center

# Importo dataset
data_penguins = sns.load_dataset('penguins')

# Hago gráfico
plt.figure(figsize=(8, 6))
ax = sns.kdeplot(x = 'flipper_length_mm', hue = 'species', fill = True, data = data_penguins, palette = 'magma', common_norm = True)
plt.xlabel('Longitud de la aleta (mm)', fontweight = 'bold', fontsize = 11)
plt.title('Distribución de la long. de la aleta según especie', fontweight = 'bold')
plt.xticks(np.arange(160,241,20), fontsize = 10)
plt.yticks(np.arange(0, 0.021, 0.01), fontsize = 10)
plt.ylabel('Densidad', fontweight = 'bold', fontsize = 11);

legend = ax.get_legend()  # Obtener la leyenda generada por Seaborn
legend.set_title('Especie')
legend.get_title().set_fontweight('bold')
legend.get_title().set_fontsize(10)

legend.set_bbox_to_anchor((1, 1))
legend.set_loc('upper left')

for text in legend.get_texts():
    text.set_fontsize(10) 
```

2. Realice una tabla en la que se muestre qué porcentaje de pingüinos del dataset pertenece a cada una de las tres especies.

3. Teniendo en cuenta las características del gráfico realizado en el ítem 1 y la información contenida en la tabla realizada en el ítem 2, ¿qué observación puede realizar acerca de las curvas de densidad representadas para cada especie? **Sugerencia:** dentro de la [documentación de Seaborn](https://seaborn.pydata.org/index.html), busque información sobre el parámetro **`common_norm`** de la función **`kdeplot()`** que utilizó para construir el gráfico.

4. ¿A cuál de las tres especies se refiere la siguiente frase? **El 90% de los pingüinos presenta una longitud de aleta menor o igual a 198 mm.**

### **Ejercicio N°4**
Utilizando el dataset `iris.csv` del **Ejercicio N°1**:

1. Construya un gráfico que le permita visualizar la distribución de los valores observados del ancho de sépalo. A partir del gráfico realizado, ¿qué puede decir acerca de la simetría de la distribución?

2. Realice un gráfico que permita comparar la distribución del largo del pétalo de las flores entre las distintas especies. Comente brevemente lo observado.

3. Construya un gráfico que le permita analizar la relación general que existe entre las variables ancho y largo del pétalo. ¿Qué observa?
    
4. Modifique el gráfico realizado en el ítem anterior de tal manera que le permita analizar si la relación general entre el ancho y el largo del pétalo se mantiene según la especie. Comente brevemente lo observado.
    
5. Construya una matriz de gráficos que le permitan estudiar la asociación que existe entre todos los pares de variables cuantitativas del dataset. *Sugerencia*: utilice la función **`pairplot()`** de **Seaborn**. 

6. Sobre las mismas variables cuantitativas del dataset, genere la matriz de correlación lineal de Pearson y represéntela gráficamente a través de un correlograma.  
    
7. A partir de lo realizado en los dos ítems anteriores, caracterice el grado de asociación lineal entre los distintos pares de variables de interés, incluyendo fuerza y dirección, y analizando la correspondencia entre los valores calculados y lo observado gráficamente.

### **Ejercicio N°5**
El set de datos `viajes_tup.xlsx` contiene información sobre el número de viajes mensuales registrados en el Transporte Urbano de Pasajeros (TUP) de la ciudad de Rosario entre los años 2015 y 2021.

1. Realice una tabla que resuma el total de viajes realizados por año y represente gráficamente dicha información. ¿Cuál fue el año en el que se registró la mayor cantidad de viajes en el TUP?

2. Construya un gráfico en el que se represente la evolución del número de viajes registrados en el TUP a lo largo de los meses para los años 2019 y 2020. Comente brevemente lo observado.

### **Ejercicio N°6**
Utilizando el dataset `partos2022.txt`, el cual contiene información sobre los partos atendidos en el 2022 en el Hospital Roque Sáenz Peña (HRSP) y la Maternidad Martin (MR), efectores municipales de la ciudad:

1. Indique los meses en los que se registró la mayor y la menor cantidad de partos atendidos. ¿Qué porcentajes del total de partos atendidos en el año representan?

2. Represente gráficamente la distribución del número de partos atendidos en el 2022 según el efector. ¿Qué puede decir acerca de la institución en la que tuvieron lugar los partos?

3. Realice un gráfico que permita comparar la distribución del peso de los recién nacidos entre las distintas categorías de la edad gestacional. ¿Qué observa?

4. Realice una descripción general de las variables **rango etario de la madre** (**`rango_edad_mama`**) y **tipo de parto** (**`terminacion_parto`**) que incluya: tipo de variables, valores que toman, distribución de cada una en la muestra y presencia de datos faltantes.
    
5. Recategorice la variable **`rango_edad_mama`** de la siguiente manera: 10-19 años, 20-29 años, 30-39 años y 40 años o más.
    
6. Recategorice la variable **`terminacion_parto`** de forma tal que la categoría **Fórceps** se encuentre comprendida dentro de **Otros**.
    
7. Construya un gráfico de barras paralelas que muestre la distribución general del tipo de parto (Cesárea/Normal/Otros) según el rango etario de la madre, en el que los porcentajes de cada categoría se encuentren calculados **sobre el total general de partos atendidos para los que se cuenta con información sobre la edad de la madre (n = 4577)**.
    
8. Construya un gráfico de barras paralelas que muestre la distribución del tipo de parto según el rango etario de la madre, en el que los porcentajes de cada categoría se encuentren calculados **sobre el total de partos atendidos para cada uno de estos grupos etarios**.
    
***Para tener de referencia, en la siguiente figura se muestran ambos gráficos terminados. Para su construcción se utilizó, en ambos casos, la función `plot.barh()` de Pandas, previa generación de las respectivas tablas de doble entrada, y los colores de las barras pertenecen a la paleta `deep`.***
    
```{code-cell} python
:tags: [remove-input, remove-stderr]
:align: center

# Importo dataset
data_partos = pd.read_csv('datasets/partos2022.txt', encoding = 'latin-1', delimiter = '\t')

# Recategorizo rango_edad_mama
def recategorizar(rango_edad_mama):
    if (rango_edad_mama == '10 a 14 años') | (rango_edad_mama == '15 a 19 años'):
        return '10 a 19 años'
    elif (rango_edad_mama == '20 a 24 años') | (rango_edad_mama == '25 a 29 años'):
        return '20 a 29 años'
    elif (rango_edad_mama == '30 a 34 años') | (rango_edad_mama == '35 a 39 años'):
        return '30 a 39 años'
    elif (rango_edad_mama == '40 a 44 años') | (rango_edad_mama == '45 a 49 años') | (rango_edad_mama == '50 años y más'):
        return 'Más de 40'
    else:
        return rango_edad_mama
  
data_partos['rango_edad_mama_recat'] = data_partos['rango_edad_mama'].apply(recategorizar)

# Recategorizo terminacion_parto
data_partos['terminacion_parto_recat'] = data_partos['terminacion_parto'].replace('Fórceps', 'Otros')

# Genero tabla de doble entrada con % calculado sobre el total
tabla_frecuencias = data_partos.groupby(['rango_edad_mama_recat','terminacion_parto_recat']).size().unstack()

tabla_porcentajes_totales = tabla_frecuencias.div(len(data_partos))*100

# Genero tabla de doble entrada con % calculado sobre total rango_edad_mama_recat
tabla_porcentajes_relativos = tabla_frecuencias.div(tabla_frecuencias.sum(axis = 1), axis = 0)*100

# Construyo gráficos
# Hago gráfico

color = sns.set_palette('deep')

fig, axes = plt.subplots(2, 1, figsize=(8,12))
plt.subplots_adjust(hspace = 0.5, wspace = 0.3)

#columnas = ['sepal_width_cm', 'petal_width_cm']

tabla_porcentajes_totales.plot.barh(stacked = False, width = 0.85, ax = axes[0], color = color)
axes[0].set_xlabel('Porcentaje (%)', fontweight = 'bold', fontsize = 10)
axes[0].set_ylabel('Rango etario de la madre', fontweight = 'bold', fontsize = 10)
axes[0].tick_params(axis='both', labelsize=9)
axes[0].legend(bbox_to_anchor = (1,1), title='Tipo de parto', fontsize=8, title_fontsize=8)
axes[0].set_title('Gráfico ítem 7', fontsize=10, fontweight='bold')
tabla_porcentajes_relativos.plot.barh(stacked = False, width = 0.85, ax = axes[1], color = color)
axes[1].set_xlabel('Porcentaje (%)', fontweight = 'bold', fontsize = 10)
axes[1].set_ylabel('Rango etario de la madre', fontweight = 'bold', fontsize = 10)
axes[1].tick_params(axis='both', labelsize=9)
axes[1].legend(bbox_to_anchor = (1,1), title='Tipo de parto', fontsize=8, title_fontsize=8)
axes[1].set_title('Gráfico ítem 8', fontsize=10, fontweight='bold');
```
    
9. Compare los gráficos realizados en el ítem anterior. ¿Qué tipo de información brinda cada uno?
    
10. **PARA PENSAR:** ¿Cuál de los gráficos anteriores le permite analizar si la edad de la madre influye en la probabilidad de recurrir a una cesárea como método de parto? ¿Qué observa?
  