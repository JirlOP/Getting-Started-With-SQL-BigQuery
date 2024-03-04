# As & With

## Keywords

1. Legibilidad
   * `AS`(Alias)
   * `WITH`
   * `WITH ... AS`
   * CTE (Common Table Expression)
2. `Date()`

## Notas

1. Organización de código y legibilidad.
   * `AS`: Alias para columnas.
   * `WITH`: Alias para subqueries.
   * `WITH ... AS`: Alias para subqueries con múltiples columnas.
     * **CTE** (Common Table Expression): tabla temporal que yo retorno dentro de una consulta. Sirven para hacer subqueries más legibles.
       * Podemos referirnos a la tabla temporal en la misma consulta. Para hacer una quiery más compleja dividida en partes más pequeñas.
       * Se hace pull de `CTEs` con `FROM [CTEname]`
       * **Solo existe en la consulta que la crea**.
     * Va antes de la consulta principal.

2. `Date()`: Función para extraer la fecha de un campo tipo `DATETIME`.

## Resumen

* Uso de `WITH ... AS` para hacer subqueries más legibles.

```sql
  WITH time AS
  (
      SELECT DATE(block_timestamp) AS trans_date
      FROM `bigquery-public-data.crypto_bitcoin.transactions`
  )
  SELECT COUNT(1) AS transactions,
        trans_date
  FROM time
  GROUP BY trans_date
  ORDER BY trans_date
```

* Uso de `DATE()` para extraer la fecha de un campo tipo `DATETIME`.

```sql
  SELECT DATE([datetime_column]) AS [date_column]
  FROM [table]
```

Para ver con ejemplos reales revise los .ipynb de la clase.

## Ejercicios

Ejericio de la clase. Sobre el conjunto de datos de taxis y tráfico.

### 1) Find the data

* Se buscó cómo se llama la tabla ya teniendo el dataset.
* Básicamnete se hizo list de las tablas del dataset.

### 2) Peak at the data

* Se hizo referencia de la tabla con `bigquery` y `dataset.table`.
* Se enseñaron las primeroas filas de la tabla con `head()`.
* Se verificó la información de la tabla.
* Los datos parecen tener NAN en algunos campos, y cómo sabemos tener NAN o NULL en una tabla puede ser un problema.

### 3) Determine when this data is from

* Para saber si los datos son relevantes hay que hacer una query para ver los años en los que se hace más viajes.
* Con una `WITH ... AS` se hace una subquery para convertir datetime a **year**
* Se uso esa subquery + `COUNT` + `GROUP BY` para ver cuántos viajes se hacen por cada año.

### 4) Dive slightly deeper

* Lo mismo que lo anterior pero con meses del año 2016.

### 5) Write the query

Escriba una consulta que muestre, para cada hora del día en el conjunto de datos, el número correspondiente de viajes y la velocidad promedio.

La respuesta debe tener: **hour_of_day**, **num_trips** y **avg_mph**.

* Se usa el extraer de horas de un campo `DATETIME` con `EXTRACT`.
  * Estaba pegado en cómo dividir todo en horas al día(columna con filas de 0 a 23). Resulta que el `EXTRACT` y `HOUR` lo hacen automático.
* No es lo mismo comparar por mes con formato `MM` que con `YYYY-MM-DD`.

## Aprendizaje

* Aprendimos a hacer subqueries más legibles con `WITH ... AS`.
* Aprendimos a extraer la fecha de un campo tipo `DATETIME` con `DATE()`.
* Aprendimos a que `EXTRACT` y `HOUR` hacen el trabajo de dividir en horas al día. Y supongo que igual con minutos y segundos. O meses y días.
* Si se ocupan datos de la tabla grande se pueden pasar por la tabla temporal creada con `WITH ... AS`.
