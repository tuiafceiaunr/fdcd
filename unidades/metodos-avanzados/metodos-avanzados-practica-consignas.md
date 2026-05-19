# Métodos Avanzados: medidas de similaridad y distancia

```{admonition} 📂 Descargar archivos  
[Descargar los archivos para la práctica desde el Campus Virtual](https://campusv.fceia.unr.edu.ar/course/view.php?id=471)
```

### Ejercicio N°1

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


