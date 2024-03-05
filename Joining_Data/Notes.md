# Joining Data

## Keywords

1. Combine sources
   * Multiple tables
   * `JOIN`
   * Example of relational tables
   * `ON`
   * `INNER JOIN`
   * `WHERE ... LIKE ...`

## Notas

1. Tablas relacionales
   * `JOIN`: Combinar tablas relacionales. Crear una tabla con columnas de dos tablas.
     * Se puede crear con las columnas que se relacionan.
   * `ON`: Especificar la columna que se relaciona de una tabla con otra.
     * Es buena práctica siempre especificar la tabla de la que se está tomando la columna.
   * `INNER JOIN`: Especificar que se quiere unir las tablas con las columnas que se relacionan.
     * Solo funciona si las columnas que se relacionan tienen valores en ambas tablas.
     * Es más común que `JOIN`.
   * `WHERE ... LIKE ...`: Filtrar resultados con una condición.
     * `LIKE` es un operador que se usa para comparar texto.
     * `%`: Representa cualquier número de caracteres.

## Resumen

* Uso de `INNER JOIN` para combinar tablas relacionales.
```sql
  SELECT L.license, COUNT(1) AS number_of_files
  FROM `bigquery-public-data.github_repos.sample_files` AS sf
  INNER JOIN `bigquery-public-data.github_repos.licenses` AS L
      ON sf.repo_name = L.repo_name
  GROUP BY L.license
  ORDER BY number_of_files DESC
```

* Uso de `WHERE ... LIKE ...` para filtrar resultados.
```sql
  SELECT *
  FROM `bigquery-public-data.pet_records.pets`
  WHERE Name LIKE 'Ripley'
```

Esta consulta devuelve todas las columnas de la tabla `pets` para las filas donde el valor de la columna `Name` es **'Ripley'**.

* Uso de `%` para representar cualquier número de caracteres.

```sql
  SELECT *
  FROM `bigquery-public-data.pet_records.pets`
  WHERE Name LIKE '%ipl%'
```

Consulta que devuelve todas las columnas de la tabla `pets` para las filas donde el valor de la columna `Name` contiene la secuencia de caracteres **'ipl'** en cualquier parte del texto.


## Ejercicios

Servicio que provee los expertos de Stack Overflow para ciertos temas.

### 1) Explore the data

* Busque qué tablas tenía disponible la database.
* Se sacaron las tablas con una función de una línea con `for`

### 2) Review relevant tables

* La tabla más relevante para el servicio es **posts_answers**.
* Esa tabla va a ser la que se vea los primeros valores para ver si sirve.
* Hay un espacio `parent_id` que hace referencia a la pregunta que se respondió.
* La tabla `posts_questions` tiene la información de las preguntas, este de relaciona con `posts_answers` con `id` y `parent_id`.
* Para brindar el servicio:
  * Si observamos, para encontrar las respuestas con más relevancia, se pueden unir las tablas con `id` y `parent_id`(el id de la pregunta y la referencia del mismo ID de la respuesta respectivamente), para ver que usuario(con `owner_user_id`) de la tabla `posts_answers` tiene más likes(`score`) a cierta pregunta(`parent_id`).
  * Y para dividir por tema, la tabla `posts_questions` tiene la columna `tags` y `title`.

### 3) Selecting the right questions

* Aprendimos a usar `WHERE` y `LIKE` para filtrar resultados. Más `%`.
* Obtenemos todos los resultados que tienen `bigquery` en la columna tags.

### 4) Your first join

Escriba una consulta que devuelva las columnas id, body y owner_user_id de la tabla posts_answers para respuestas a preguntas relacionadas con "bigquery".

* No problemas con este ejercicio, para esto use: `INNER JOIN`, `ON` y `WHERE ... LIKE ...`.
* Tener cuidado con con las comillas en el código SQL.

### 5) Answer the question

Ahora hay que buscar los usuarios que han respondido más preguntas.

* Se usó `GROUP BY` y `COUNT` para contar las respuestas de cada usuario.
* Igual se combinaron las tablas con `INNER JOIN` y `ON`.

### 6) Building a more generally useful service

¿Cómo convertiría las consultas anteriores para que se usen en un website, y que se puedan llamar del backend?

Puede ser un panel de opciones en el que se escoja cómo se puede filtrar la información. Seǵun las etiquetas de las tablas.

Por ejemplo, si se quiere filtrar por usuarios que han resolvido más preguntas. Se puede usar la query del ejercicio 5. Nadamas editandole el `WHERE` según el tema que quiera el usuario.

## Aprendizaje

Aprendí a usar `INNER JOIN` para combinar tablas relacionales. Y a usar `WHERE ... LIKE ...` para filtrar resultados. Más `%` para representar cualquier número de caracteres.
