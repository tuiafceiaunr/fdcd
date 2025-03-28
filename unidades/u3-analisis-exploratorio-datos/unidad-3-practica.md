# Unidad 3 - Análisis exploratorio de datos: Medidas de Resumen

```{admonition} 📂 Descargar archivos  
[Descargar los archivos para la práctica desde el Campus Virtual](https://campusv.fceia.unr.edu.ar/course/view.php?id=471)
```

### **Ejercicio N°1**

El dataset `winequality-red.csv` contiene un conjunto de variables relacionadas con propiedades fisicoquímicas que fueron determinadas sobre una serie de vinos de una misma variedad, así como un puntaje asignado en cada caso por un panel de enólogos en sesiones de cata. Importe el dataset al entorno de trabajo y realice cualquier tipo de limpieza y adecuación del mismo que considere necesaria para su posterior análisis.

1. Clasifique las variables del dataset en cualitativas, cuantitativas discretas y cuantitativas continuas.

2. El 25% de los vinos del dataset tiene un contenido de alcohol superior a… ¿qué valor?

3. Realice una tabla en la que se presenten, para las variables densidad y pH, **únicamente** las siguientes medidas descriptivas: media, mediana, desvío estándar y rango intercuartil. A continuación, responda a las siguientes preguntas **sin realizar ningún gráfico:**

- ¿Cómo describiría ambas distribuciones en relación a sus características de simetría?
- ¿Cuál de los dos conjuntos de observaciones (densidad o pH) presenta mayor variabilidad?

4. Represente la distribución de las observaciones de la variable contenido de alcohol (alcohol) a través de un boxplot. *Sugerencia*: utilice la función `sns.boxplot()` de la librería  seaborn`. Basándose en el gráfico, ¿cuál de las siguientes medidas de posición o centralidad (media aritmética/mediana) le parece más adecuada para describir a esta variable?

5. Realice una tabla de frecuencias para resumir la distribución de los vinos del dataset en función del puntaje asignado según su calidad (quality).

- ¿Cuál de los puntajes fue recibido por una mayor cantidad de vinos?
- ¿Qué porcentaje de los vinos de la muestra recibieron la calificación más baja?


### **Ejercicio N°2**

El dataset `alimentos.csv` fue elaborado por una clínica de nutrición que suministró a sus pacientes una lista de alimentos permitidos con sus respectivos contenidos calóricos. También se detalló el tipo de alimento del que se trataba (fruta, verdura, etc.) y el tipo de vitamina que aportaba cada uno (A, B o C).

Por otra parte, la nutricionista a cargo del estudio lleva una planilla de control de la evolución de 50 pacientes (`pacientes.csv`) en la que registra la edad, el sexo, la altura, el peso inicial y el peso final de cada uno de ellos luego de seguir un plan de dieta por una cierta cantidad de tiempo, información que también fue registrada en el campo “tiempo de tratamiento”.

1. Importe ambos datasets al entorno de trabajo y realice cualquier tarea de limpieza y/o
adecuación de los mismos que considere necesaria.

2. En relación al campo `aporte_calorico_kcal` informe las medidas descriptivas que le brinden información sobre los siguientes aspectos:

- Las kcal que aportan, en promedio, los alimentos que forman parte del dataset.
- Aquel valor de aporte calórico tal que el 50% de los alimentos del dataset presentan aportes calóricos menores o iguales a él.
- La dispersión o variabilidad del 50% central de las observaciones.
- El o los valores que se presentan con mayor frecuencia entre las observaciones.

3. Represente la distribución de las observaciones de la variable `aporte_calorico_kcal` a través de un boxplot.

- Identifique en el gráfico la mediana, el primer y el tercer cuartil.
- ¿Cómo caracterizaría a la distribución en relación a sus características de simetría?
- En función a lo observado, ¿qué par de medidas de centralidad/posición (media aritmética - mediana) y de dispersión (rango intercuartil - rango - desviación estándar) le parece más adecuada para describir a este conjunto?
- ¿Existe alguna observación que pueda ser considerada como atípica? En caso de respuesta
afirmativa, ¿cuántas observaciones recibirían esta calificación?

4. ¿Qué tipo de alimento presenta la mayor mediana de aporte calórico?

5. Realice un boxplot para representar la distribución de los aportes calóricos de alimentos de los siguientes tipos: frutas, verduras y alimentos elaborados.
- ¿Qué tipo de alimentos presenta valores calóricos más variables y cuál menos variables?
- ¿Qué medida descriptiva utilizó para responder a estas últimas preguntas?

6. Utilizando los datos de los/las pacientes, genere una variable que corresponda a la variación de peso para cada paciente a lo largo del tratamiento (`peso_final_kg` - `peso_inicial_kg`).
- Represente los valores observados de la variable “diferencia de peso” en función del sexo a través de un boxplot.
- ¿Qué medida descriptiva utilizaría para comparar los resultados del tratamiento entre personas de ambos sexos? En función de su respuesta, ¿las personas de qué sexo obtuvieron los mejores resultados para el tratamiento?

### **Ejercicio N°3**

Importe y explore el conjunto de datos `titanic.csv`.

1. Realice una descripción general del conjunto de datos que incluya la descripción de la información brindada por cada columna, el tipo de datos que contiene cada una y el número de registros.

2. Realice cualquier tipo de limpieza y adecuación del dataset que considere necesaria para su posterior análisis, incluyendo manejo de valores faltantes y de datos duplicados y/o potencialmente erróneos.

3. Calcule la media, la mediana y la desviación estándar de la edad de los/las pasajeros/as que murieron y sobrevivieron para cada clase. Realice un boxplot que muestre la distribución de edades para cada grupo (murieron/sobrevivieron) dentro de cada clase. ¿En qué clase las edades de las personas que sobrevivieron fueron más variables? ¿Cuál fue la edad de la persona más joven que sobrevivió en tercera clase?

4. Represente gráficamente la distribución de los precios de los pasajes en función de la clase del pasajero y calcule el promedio, la moda, la mediana, la desviación estándar y el rango intercuartil del precio del pasaje para cada grupo. ¿En qué clase los precios de pasaje presentaron una mayor variabilidad?

5.

  a. ¿Qué medida resumen calcularía si quisiera conocer aquel valor que representa el precio que sólo el 25% de los pasajeros superaron a la hora de comprar su boleto? 
    
  b. Identifique cuáles fueron los pasajeros que pagaron un pasaje igual o más caro que el valor calculado en el ítem anterior. Construya una tabla en la que se informen los nombres de estas personas, el número total de personas vinculadas a ellas que se encontraban en el barco y la ciudad en la que embarcaron.
    
  c. En base a la tabla construida en el ítem anterior, ¿con cuántos acompañantes, en promedio, viajaban estos pasajeros? ¿En qué puerto embarcó la mayoría de ellos?
    
6. Construya una tabla en la que se resuma la distribución de pasajeros del Titanic en función de la clase en la que viajaron. La misma debe contener la siguiente información (en distintas columnas): cantidad de pasajeros/as que viajaron en cada clase y porcentajes en relación al total. ¿A qué clase pertenecía la mayoría de los pasajeros del Titanic?

7. Construya una tabla de contingencia cruzando las variables `survived` y `Pclass`. ¿Qué proporción de personas de cada clase sobrevivieron al naufragio del Titanic? Represente gráficamente esta información en un gráfico de barras.