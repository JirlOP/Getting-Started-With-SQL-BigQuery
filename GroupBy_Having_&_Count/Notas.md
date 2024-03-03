# Group By, Having & Count

## Keywords

1. GROUP BY, HAVING and COUNT().
    * Ignorar Contar Agrupar
2. Alias y mejoras
    * `AS` y `ORDER BY`.

## Notas

1. Agrupar datos y contar cosas dentro de ellos.
   * `GROUP BY` se utiliza para agrupar datos.
     * Toma el nombre de una o más columnas.
     * No tiene sentido si no se usa con una **función agregada**.
   * `HAVING` se utiliza para filtrar datos.
     * Se usa junto con `GROUP BY`: para ignorar grupos que no cumplan con ciertas condiciones.
   * `COUNT()` se utiliza para contar cosas.
     * Función agregada: toma muchos valores y saca un solo valor.
     * Se usa junto `SELECT`.
     * `COUNT(1)`: cuenta cuántas filas hay en cada grupo.
2. Colocar nombres más descriptivos a las columnas.
   * `AS` se utiliza para cambiar el nombre de una columna(agregarle una alias).
   * `ORDER BY` se utiliza para ordenar los resultados.
     * Por defecto, ordena de forma ascendente.
     * Se puede usar `DESC` para ordenar de forma descendente.

Las palabras reservada de SQL, si se necesitan escribir en un mensaje de una query, se deben escribir entre comillas invertidas.

```python
query = """
        SELECT `by` AS author, parent, COUNT(id)
        """
```

## Resumen

* Uso de `COUNT()` para contar elementos en una columna.

```python
query = """
        SELECT COUNT(columnName)
        FROM `bigquery-public-data.openaq.tableName`
        """
```

* Uso de `GROUP BY` para agrupar datos y contar cuántos datos hay en cada grupo.

```python
query = """
        SELECT column1, COUNT(column2)
        FROM `bigquery-public-data.openaq.tableName`
        GROUP BY column1
        """
```

Selecciona `column1` y cuenta cuántos valores hay en `column2` para cada valor único en `column1`.

* Ignorar grupos que no cumplan con ciertas condiciones.

```python
query = """
        SELECT column1, COUNT(column2)
        FROM `bigquery-public-data.openaq.tableName`
        GROUP BY column1
        HAVING COUNT(column2) > 100
        """
```

Selecciona `column1` y cuenta cuántos valores hay en `column2` para cada valor único en `column1`, pero solo incluye grupos donde el recuento sea mayor que 100.

* Poner nombre al COUNT() más. COUNT(1) para contar filas.

```python
query_improved = """
                 SELECT parent, COUNT(1) AS NumPosts
                 FROM `bigquery-public-data.hacker_news.full`
                 GROUP BY parent
                 HAVING COUNT(1) > 10
                 """
```

## Ejercicios

### 1) Prolific commenters

Buscamos los autores que tenían más de 10000 posts en el conjunto de datos de Hacker News.

### 2) Deleted comments

Buscamos el números de comentarios eliminados.

## Aprendizajes

* Uso de `COUNT()` para contar elementos en una columna.
* Uso de `GROUP BY` para agrupar datos y contar cuántos datos hay en cada grupo.
* Uso de `HAVING` para ignorar grupos que no cumplan con ciertas condiciones.
* Poner nombre al `COUNT()` más. `COUNT(1)` para contar filas.
* Uso de `AS` para cambiar el nombre de una columna(agregarle una alias). Se suele poner en `SELECT`.
* `COUNT(1)` es más rápido que `COUNT(columnName)`.
