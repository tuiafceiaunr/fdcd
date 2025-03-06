# Unidad 1 - Planilla de Cálculo

```{admonition} 📂 Descargar archivos  
[Descargar los archivos para la práctica desde el Campus Virtual](https://campusv.fceia.unr.edu.ar/course/view.php?id=471){target=_blank} 

### Ejercicio N°1

Utilizando el archivo ****`peliculas_top100.xlsx` **resuelva las siguientes consignas:

1. Se requiere generar una tabla en la que, una vez ingresado manualmente el título de una película en la primera columna, se muestre automáticamente el nombre de su director, su duración y rating en las columnas subsiguientes:

| **Título** | **Director** | **Duración** | **Puntaje** |
| --- | --- | --- | --- |
| Black Adam | Jaume Collet Serra | 125 | 6.5 |
| She Said | Maria Schrader | 129 | 7.2 |

Para lograr esto, se pide utilizar la función **VLOOKUP** en cada celda correspondiente. La tabla resultante deberá contener 5 (cinco) películas y se creará en una hoja (*sheet*) separada dentro del mismo archivo en el que se encuentra el dataset.

1. Realizar la misma tabla del ítem anterior mediante el uso de las funciones **INDEX** y **MATCH** en forma anidada.
2. Utilizando la herramienta de creación de **tablas dinámicas,** genere tablas en hojas independientes que permitan responder las siguientes preguntas:
    1. ¿Cuántas películas de cada director se encuentran en el archivo?
    2. ¿Qué duración (*Runtime*) promedio tienen las películas según el género del cual se trate?
    3. ¿Cuáles fueron los 3 países en los que se filmó una mayor cantidad de películas?
    4. ¿Cuál fue el presupuesto (*Budget*) anual de cada director?

### Ejercicio N°2

Utilizando el archivo `superficie-incendiada-por-provincias-tipo-de-vegetacion_2022.xlsx` **resuelva las siguientes consignas:

1. ¿En qué año se registró la mayor superficie quemada total a nivel país?
2. ¿Cuál fue la provincia, para cada año de la serie de tiempo, con mayor superficie quemada total? Para esta pregunta podrá utilizar una combinación de tabla dinámica y las otras funciones vistas en clase.

Algunos resultados que debería obtener:

| **Año** | **Provincia con mayor superficie quemada** |
| --- | --- |
| 1993 | La pampa |
| 1994 | La Pampa |
| 1995 | Córdoba |
| 1996 | Catamarca |
| … | … |
1. Para cada provincia, ¿cuál fue el año en que se registró la mayor superficie quemada total? Para esta pregunta podrá utilizar una combinación de tabla dinámica y las otras funciones vistas en clase). 

Algunos resultados que debería obtener:

| **Provincia** | **Año con máxima superficie quemada** |
| --- | --- |
| Buenos Aires | 2003 |
| Catamarca | 2019 |
| Chaco | 2006 |
| Chubut | 2003 |
| … | … |

### Ejercicio N°3

El archivo `personas_2020.xlsx` contiene información sobre los empleados de CONICET en 2020. En este archivo, algunas variables han sido codificadas y su referencia se encuentra en las otras hojas de la planilla de cálculo. Por ejemplo, la columna `sexo_id` tiene valores de 1 y 2, y la referencia a dichos valores se encuentra en la hoja `ref_sexo` (según esta otra hoja, el valor 1 corresponde a masculino y el valor 2 corresponde a femenino).

1. Utilizando las funciones de búsqueda vistas en clase, inserte una nueva columna en la hoja *personas_2020* que incluya la variable *sexo_descripcion.*
2. Inserte una nueva columna en la hoja *personas_2020* que se denomine *max_grado_academico_descripcion* e incluya la variable *descripción* de la hoja *ref_grado_academico.*
3. Realice una tabla dinámica en la cual se muestre cuántos empleados de CONICET había de cada sexo para cada máximo grado académico en 2020.