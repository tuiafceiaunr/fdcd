# Unidad 3 - Análisis exploratorio de datos: Medidas de Resumen

### Fundamentos de ciencia de datos

![Banner FCD-1.png](imagenes/Banner_FCD-1.png)

**Ejercicio N°1**

El dataset `alimentos.csv` fue elaborado por una clínica de nutrición que suministró a sus pacientes una lista de alimentos permitidos con sus respectivos contenidos calóricos. También se detalló el tipo de alimento del que se trataba (fruta, verdura, etc.) y el tipo de vitamina que aportaba cada uno (A, B o C).

Por otra parte, la nutricionista a cargo del estudio lleva una planilla de control de la evolución de 50 pacientes (`pacientes.csv`) en la que registra la edad, el sexo, la altura, el peso inicial y el peso final de cada uno de ellos luego de seguir un plan de dieta por una cierta cantidad de tiempo, el cual también fue registrado en el campo “tiempo de tratamiento”. 

1. Importe los datasets al entorno de trabajo y realice una descripción general de los mismos que incluya el tipo y rango de datos que componen cada columna y el número de registros.
2. En relación al campo “aporte_calorico_kcal” informe las medidas descriptivas que le brinden información sobre los siguientes aspectos:
    1. Las kcal que aportan, en promedio, los alimentos que forman parte del dataset.
    2. Aquel valor de aporte calórico tal que el 50% de los alimentos del dataset presentan aportes calóricos menores o iguales a él.
    3. La dispersión o variabilidad del 50% central de las observaciones.
    4. El o los valores que se presentan con mayor frecuencia.
3. Represente la distribución de las observaciones del “aporte_calorico_kcal” a través de un boxplot. 
    1. Identifique en el gráfico la mediana, el primer y el tercer cuartil. 
    2. ¿Cómo caracterizaría a la distribución en relación a sus características de simetría?
    3. En función a lo observado, ¿qué par de medidas de centralidad/posición (media aritmética - mediana) y de dispersión (rango intercuartil - rango - desviación estándar) le parece más adecuada para describir a este conjunto?
    4. ¿Existe alguna observación que pueda ser considerada como atípica? En caso de respuesta afirmativa, ¿cuántas observaciones recibirían esta calificación?
4. 
5. Utilizando los datos de los/las pacientes agregue una columna al dataset en la que se calcule la variación del peso corporal para cada paciente (peso final - peso inicial) y represente los valores observados en función del género a través de un boxplot. ¿En que género considera que se obtuvieron los mejores resultados para el tratamiento? ¿Existen valores atípicos en la distribución de la variación de peso corporal para alguno de los dos géneros? 
6. Para el mismo campo, calcule Q1, Q3 y el rango intercuartil. Luego calcule los percentiles del 25, 50 y 75%. Estos últimos valores, ¿coinciden con alguno/s calculado/s previamente?
7. Represente la distribución de los valores observados de la variable “aporte_calorico_kcal” a través de un histograma y grafique sobre el mismo la moda, la media y la mediana.
8. Represente la distribución de los valores observados de la variable “aporte_calorico_kcal” a través de un boxplot. Identifique en el gráfico la mediana, el primer y el tercer cuartil y el rango intercuartil. ¿Cómo caracterizaría a la distribución en relación a sus características de simetría?
9. En función de los gráficos realizados en los puntos anteriores, ¿existe alguna observación que pueda ser considerada como atípica? En caso afirmativo, elimine dichas observaciones en el dataset y vuelva a realizar el gráfico del ítem anterior. ¿Detecta algún cambio?
10. Calcule la media y la mediana de la variable “aporte_calorico_kcal” según el tipo de alimento. 
11. Realice un boxplot para representar la distribución de los aportes calóricos según el tipo de alimento. ¿Cuál es la categoría de alimentos que parece aportar menos calorías? ¿Qué categoría de alimentos aporta valores calóricos más variables y cuál menos variables? ¿Qué medida descriptiva le aporta información para responder a estas últimas preguntas?
12. Utilizando los datos de los/las pacientes agregue una columna al dataset en la que se calcule la variación del peso corporal para cada paciente (peso final - peso inicial) y represente los valores observados en función del género a través de un boxplot. ¿En que género considera que se obtuvieron los mejores resultados para el tratamiento? ¿Existen valores atípicos en la distribución de la variación de peso corporal para alguno de los dos géneros?

**Ejercicio N°2**

El dataset `winequality-red.csv` contiene un conjunto de variables relacionadas con propiedades fisicoquímicas que fueron determinadas sobre una serie de vinos de una misma variedad, así como un puntaje asignado en cada caso por un panel de enólogos en sesiones de cata.

1. Importe el dataset al entorno de trabajo y realice cualquier tipo de limpieza y adecuación del mismo que considere necesaria para su posterior análisis, incluyendo manejo de valores faltantes y de datos duplicados y/o potencialmente erróneos.
2. El 25% de los vinos del dataset presenta un contenido de alcohol superior a... ¿a qué valor?
3. Realice una tabla en la que se presenten, para las variables densidad y pH, las siguientes medidas: media, mediana, desvío estándar y rango intercuartil. 
Responda a las siguientes preguntas sin realizar ningún gráfico: 
    1. ¿Cómo describiría ambas distribuciones en relación a sus características de simetría?
    2. ¿Cuál de las dos distribuciones presenta mayor variabilidad?
4. Represente la distribución de la variable contenido de alcohol (`alcohol`) a través de un boxplot. En función del gráfico realizado, ¿cuál de las siguientes medidas de posición o centralidad (media aritmética/mediana) le parece más adecuada para describir a esta variable?
5. Realice una tabla de frecuencias para resumir la distribución de los vinos del dataset en función del puntaje asignado según su calidad (`quality`). ¿Cuál de los puntajes fue recibido por una mayor cantidad de vinos? ¿Qué porcentaje de los vinos de la muestra recibieron la calificación más baja?

**Ejercicio N°3**

Importe y explore el conjunto de datos `titanic.csv`

1. Realice una descripción general del conjunto de datos que incluya la descripción de la información brindada por cada columna, el tipo de datos que contiene cada una y el número de registros.
2. Realice cualquier tipo de limpieza y adecuación del dataset que considere necesaria para su posterior análisis, incluyendo manejo de valores faltantes y de datos duplicados y/o potencialmente erróneos.
3. Calcule la media, la mediana y la desviación estándar de la edad de los/las pasajeros/as que murieron y sobrevivieron para cada clase. Realice un boxplot que muestre la distribución de edades para cada grupo (murieron/sobrevivieron) dentro de cada clase. ¿En qué clase las edades de las personas que sobrevivieron fueron más variables? ¿Cuál fue la edad de la persona más joven que sobrevivió en tercera clase?.
4. Represente gráficamente la distribución de los precios de los pasajes en función de la clase del pasajero y calcule el promedio, la moda, la mediana, la desviación estándar y el rango intercuartil del precio del pasaje para cada grupo. ¿En qué clase los precios de pasaje presentaron una mayor variabilidad?
5. a. ¿Qué medida resumen calcularía si quisiera conocer aquel valor que representa el precio que sólo el 25% de los pasajeros superaron a la hora de comprar su boleto? 
    
    b. Identifique cuáles fueron los pasajeros que pagaron un pasaje igual o más caro que el valor calculado en el ítem anterior. Construya una tabla en la que se informen los nombres de estas personas, el número total de personas vinculadas a ellas que se encontraban en el barco y la ciudad en la que embarcaron.
    
    c. En base a la tabla construida en el ítem anterior, ¿con cuántos acompañantes, en promedio, viajaban estos pasajeros? ¿En qué puerto embarcó la mayoría de ellos?
    
6. Construya una tabla en la que se resuma la distribución de pasajeros del Titanic en función de la clase en la que viajaron. La misma debe contener la siguiente información (en distintas columnas): cantidad de pasajeros/as que viajaron en cada clase y porcentajes en relación al total. ¿A qué clase pertenecía la mayoría de los pasajeros del Titanic?
7. Construya una tabla de contingencia cruzando las variables “survived” y “Pclass”. ¿Qué proporción de personas de cada clase sobrevivieron al naufragio del Titanic? Represente gráficamente esta información en un gráfico de barras.
8. a. Categorice la variable edad en los siguientes grupos etarios: 0-18 años, 19-35 años, 36-56 años y >57 años.
    
    b. Construya un gráfico de barras que muestre la cantidad de personas de género masculino y femenino que sobrevivieron y murieron según el rango etario definido anteriormente.
    

**Ejercicio N°4**

Trabajando con el dataset `alimentos.csv`:

1. Construya una tabla de frecuencias que resuma la cantidad de alimentos que aportan los distintos tipos de vitamina (A, B y C) y que incluya la frecuencia relativa de alimentos que pertenecen a cada grupo. ¿Qué proporción de alimentos del dataset aportan vitamina A?
2. Construya un gráfico para visualizar la información anterior.

**Ejercicio N°5**

El dataset `wine_quality.xlsx` contiene información acerca del puntaje que un panel de enólogos asignó a una serie de 76 vinos de tipo *Pinot Noir*. Las cualidades evaluadas incluyeron algunas propiedades organolépticas como claridad (*clarity*), aroma (*aroma*), cuerpo (*body*) y sabor (*flavor*) y una valoración de la calidad general del vino (*quality*). Adicionalmente, se recabó información sobre el grado de envejecimiento (*aging*) de cada uno de los productos evaluados, la cual se encuentra en el dataset `wine_aging.csv`.

1. Importe ambos datasets y realice cualquier tarea de limpieza y adecuación de los mismos que considere necesaria para su posterior análisis. 
2. Realice un gráfico que le permita visualizar la distribución del sabor de los vinos (*flavor*) según los distintos grados de envejecimiento (*aging*). ¿Cuál es el tipo de vino (crianza/reserva/gran reserva) que presenta la mayor mediana para el sabor?
3. Construya la matriz de covariancia y comente qué tipo de información le aporta acerca de la relación entre los distintos pares de variables cuantitativas del dataset.
4. Construya un gráfico que le permita evaluar el grado de asociación lineal que existe entre las distintas variables cuantitativas que componen el dataset. En base al mismo, identifique la/s variable/s que se encuentran más fuertemente correlacionadas e informe e interprete la medida de asociación lineal correspondiente.
5. Elija el par de variables que identificó en el ítem anterior como aquellas que se encuentran más fuertemente correlacionadas linealmente y realice un gráfico que le permita visualizar la relación general que existe entre las mismas.

**Ejercicio N°6**

Utilizando el dataset `calidad_producto.csv`:

1. Calcule los coeficientes de correlación de Pearson y Spearman. En función de los valores obtenidos, ¿cómo describiría la relación entre ambas variables?
2. Realice un gráfico que le permita visualizar la relación general que existe entre las variables analizadas. ¿Qué observa? 
3. Calcule nuevamente ambos coeficientes sin tomar en cuenta los registros que incluyan observaciones potencialmente atípicas. ¿Cómo resultan los valores obtenidos en comparación con los calculados en el ítem 1?
4. En función de las características de la relación entre ambas variables que se observan gráficamente, ¿cuál de las dos métricas informaría para describir en forma cuantitativa el grado de asociación entre ellas?

**Ejercicio N°7**

Utilice el dataset “estación_meteorologica.csv”

1. Calcule y grafique las matrices de correlación y covarianza.
2. Identifique las variables más correlacionadas y grafíquelas una vs. la otra en un gráfico de dispersión. Luego grafique cada una a lo largo del tiempo (para todas las fechas).
3. Identifique las variables menos correlacionadas y grafíquelas una vs. la otra en un gráfico de dispersión. Luego grafique cada una a lo largo del tiempo (para todas las fechas).

¿Qué puede concluir de los gráficos del punto 2 y 3?