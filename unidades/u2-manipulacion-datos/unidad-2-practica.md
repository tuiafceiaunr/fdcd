# Unidad 2 - Manipulación de Datos

```{admonition} 📂 Descargar archivos  
[Descargar los archivos para la práctica desde el Campus Virtual](https://campusv.fceia.unr.edu.ar/course/view.php?id=471)
```
### **Ejercicio N°1**

Lea en *Colab* el archivo `flete-aereo-vacunas-covid19-al-2021-06-28.xlsx` y realice cualquier tarea de limpieza y/o adecuación del dataset que considere necesaria.

1. Según la información que se encuentra en la base de datos, ¿cuál fue el número de vuelo que realizó una mayor cantidad de fletes? *Sugerencia:* utilice el método `value_counts()` de la librería Pandas.
2. ¿Cuántos registros no contienen información sobre el número de vuelo?
3. Calcule el promedio de lo facturado en todos los vuelos realizados **considerando únicamente los registros de viajes que se facturaron en USD** según la información contenida en la columna `factura_moneda_monto`. 
4. ¿Qué porcentaje de los vuelos realizados tuvieron como origen a Rusia o a China?
5. ¿Cuál es el vuelo más reciente de los que se tiene registro en el dataset? ¿Cuántos días transcurrieron entre el primer y el último vuelo realizados?
6. Escriba el archivo en formato .parquet.

### **Ejercicio N°2**

Lea en *Colab* el archivo `incendios-cantidad-causas-provincia_2022.csv` y realice cualquier tarea de limpieza y/o adecuación del dataset que considere necesaria.

1. Obtenga el número de incendios totales por año **para todo el país**. ¿Cuál fue el año en el que se presentó un mayor número de incendios?
2. Obtenga el número de incendios totales por año para el período 1993-2021 **en la provincia de Córdoba**.
3. Realice una tabla en la que se muestre, para cada año del periodo 1993-2021, la provincia en la que tuvo lugar el mayor número de incendios intencionales. *Sugerencia:* explore las funcionalidades del método `idxmax()` de la librería Pandas.
4. Realice un gráfico de barras para visualizar el número de incendios intencionales, por negligencia y naturales que tuvieron lugar durante el periodo 2015-2021 en la provincia de Santa Fe.
5. Obtenga el número promedio de incendios intencionales, por negligencia y naturales para la provincia de Río Negro durante el periodo 1993-2021.

### **Ejercicio N°3**

A partir de los datos de la siguiente tabla, construya una función que realice una interpolación lineal por tramos. La función debe recibir como entrada un valor $x$ y devolver como salida el valor interpolado de $y$, utilizando los intervalos definidos por los puntos dados.

| **x** | **y** |
| --- | --- |
| 1 | 2 |
| 2 | 3 |
| 3 | 5 |
| 10 | 6 |

### **Ejercicio N°4**

La siguiente tabla resume la evolución de la población total argentina desde 1960 a la actualidad según los censos nacionales de población (fuente: [INDEC](https://www.indec.gob.ar/indec/web/Nivel4-Tema-2-18-77#:~:text=Aqu%C3%AD%20presentamos%20los%20datos%20del,de%20la%20poblaci%C3%B3n%20del%20pa%C3%ADs.&text=La%20poblaci%C3%B3n%20nacional%20est%C3%A1%20compuesta,mujeres%20hay%2094%2C8%20varones.)):

| **Año** | **Población total** |
| --- | --- |
| 1960 | 20013793 |
| 1970 | 23364431 |
| 1978 |  |
| 1980 | 27949780 |
| 1986 |  |
| 1991 | 32615528 |
| 2001 | 36260130 |
| 2010 | 40117096 |
| 2014 |  |
| 2022 | 46044703 |

Utilizando una interpolación lineal, complete la información sobre **Población total** para aquellos años en los que no se cuenta con datos de censos nacionales.

### **Ejercicio N°5**

Usando los datos de `listings_ba.csv` de Buenos Aires que se encuentran cargados en el campus realice la imputación de los precios de alquiler faltantes empleando:

1. La media y moda de los datos no faltantes 

2. Una medida de resumen de su elección por barrio y tipo de habitación

3. Los 10 puntos más cercanos geográficamente a cada dato faltante usando las coordenadas.

En cada uno de los casos indicar la cantidad de datos que se usaron para la imputación.

### **Ejercicio N°6**

Utilizando *regex*: 

1. Escriba una función que determine si una url es válida e imprima ‘URL válida’ (por ejemplo para `"https://pythondiario.com/"`) si la url dada como input es válida y ‘URL no válida’ en caso de que no sea válida (p.ej: `"htps:/pythondiario.com/"`).

2. Escriba una función que determine si una dirección de correo electrónico es un correo electrónico **de gmail** válido.

3. Escriba una función que determine si un string corresponde a una fecha válida y se encuentra en el formato YYYY-MM-DD.

4. Utilizando el archivo `Me_gustas_tu-Manu_Chao.txt` , que contiene la letra de la canción ***Me gustas tú*** de Manu Chao: 

- indique cuántas veces en la canción se hace referencia al verbo *gustar*

- ¿cuántos verbos en infinitivo tiene la letra de la canción?

- Realice una lista de todas las cosas que le gustan a Manu Chao. Por ejemplo: cosas_que_le_gusta = [los aviones, viajar, la mañana, el viento, soñar, la mar …. etc].

### **Ejercicio N°7**

Utilizando los archivos `conicet_personas_2020.xlsx`, `conicet_ref_sexo.xlsx` y `conicet_ref_grado_academico.xlsx`, genere una tabla en la cual se informe cuántos empleados de CONICET hay de cada sexo para cada máximo grado académico en 2020.

### **Ejercicio N°8**

Utilizando el archivo `incendios-cantidad-causas-provincia_2022.csv` del Ejercicio N°2, genere una tabla que muestre el número de incendios intencionales por provincia para cada año de los incluidos en dicho dataset. 


### **Ejercicio N°9**

1. Represente la siguiente tabla en cada uno de los formatos estudiados en la Unidad 2, utilizando un procesador de texto:

- CSV (utilizando `|` como delimitador)

- TXT

- YAML

- XML

- JSON

- HTML

| **id** | **desc_prod** | **precio** | **proveedor** |
| --- | --- | --- | --- |
| 0049570 | camisa | 2000 | fashionistas |
| 0769298 | jean | 6000 | tu moda |
| 8458909 | polera | 3000 | el ropero |

2. Modifique los archivos generados para especificar que el producto *jean* es de tipo *skinny*.

3. En *Google Colab*, importe cada uno de los archivos generados en el inciso 1 utilizando las librerías y funciones apropiadas. Luego:

- Verifique que los datos se hayan cargado correctamente.

- Explore el DataFrame resultante (por ejemplo, visualizando sus primeras filas, tipos de datos y dimensiones).

