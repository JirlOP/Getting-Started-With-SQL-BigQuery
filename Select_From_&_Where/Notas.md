# Select, From & Where

## Keywords

1. SQL queries
   * SELECT, FROM, WHERE, DISTINCT
   * Example
   * get info
2. `query()`
   * syntax
3. Working with big datasets
   * Check query size
   * Limitar query size

---

## Notas

1. SQL queries: son formas de buscar la información necesaria de grandes volúmenes de datos.
   * `SELECT`: se utiliza para seleccionar columnas específicas.
     * Se pueden seleccionar múltiples columnas con (,)
     * (*) significa todas las columnas.
   * `FROM`: se utiliza para especificar la tabla de la que se seleccionan los datos.
     * Escribiendo una quiery lo que sigue de este campo va entre (`)
   * `WHERE`: se utiliza para filtrar los datos.
     * Cuando lo usemos con texto, se debe poner entre comillas simples ('').
     * Se pueden poner múltiples filtros con `AND`.
   * `DISTINCT`: se utiliza para eliminar los duplicados.

2. `query()`: es una función que se utiliza para enviar una consulta a BigQuery.
   * Se puede convertir el objeto de consulta a un DataFrame de pandas utilizando el método `to_dataframe()`.

3. Hay que tener cuidado con el tamaño de las consultas, ya que si son **muy grandes** pueden tardar mucho tiempo en ejecutarse o otro tipo de problemas.
   * Se puede estimar el tamaño o limitar el tamaño de la consulta.

---

## Resumen

* Ejemplo de una query para seleccionar la ciudad de la tabla `bigquery-public-data.openaq.global_air_quality` donde el país es 'US'. **Python**

   ```python
   query = """
         SELECT city
         FROM `bigquery-public-data.openaq.global_air_quality`
         WHERE country = 'US'
         """
   ```

`bigquery-public-data.openaq.global_air_quality` se divide en:

* `bigquery-public-data`: es el proyecto.
* `openaq`: es el conjunto de datos.
* `global_air_quality`: es la tabla.

* Query con todas las columnas de la tabla:

   ```python
   query = """
           SELECT *
           FROM `bigquery-public-data.openaq.global_air_quality`
           WHERE country = 'US'
           """
   ```

* Query con multiples columnas:

   ```python
   query = """
         SELECT city, country
         FROM `bigquery-public-data.openaq.global_air_quality`
         WHERE country = 'US'
         """
   ```

* Probar el tamaño de la consulta:

   ```python
   # Create a QueryJobConfig object to estimate size of query without running it
   dry_run_config = bigquery.QueryJobConfig(dry_run=True)

   # API request - dry run query to estimate costs
   dry_run_query_job = client.query(query, job_config=dry_run_config)

   print("This query will process {} bytes.".format(dry_run_query_job.total_bytes_processed))
   ```

* Limitar los datos que trae los query

   ```python
   # Only run the query if it's less than 1 MB
   ONE_MB = 1000*1000
   safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=ONE_MB)

   # Set up the query (will only run if it's less than 1 MB)
   safe_query_job = client.query(query, job_config=safe_config)

   # API request - try to run the query, and return a pandas DataFrame
   safe_query_job.to_dataframe()
   ```

Si se ejecuta una consulta que es mayor a 1 MB, se generará un error.

* Para no mostrar cosas repetitas se puede utilizar `DISTINCT`:

   ```python
   query = """
         SELECT DISTINCT country
         FROM `bigquery-public-data.openaq.global_air_quality`
         WHERE unit = 'ppm'
         """
   ```

* Para poner varios filtros se puede utilizar `AND`:

   ```python
   query = """
         SELECT *
         FROM `bigquery-public-data.openaq.global_air_quality`
         WHERE country = 'US' AND city = 'Los Angeles'
         """
   ```

Se pueden filtrar números con `>`, `<`, `>=`, `<=`, `=`, `!=`.


---

## Ejercicio

### 1) Units of measurement

Para este ejercicio es solo sacar los **países** en los que las **unidades** de medida son 'ppm'.

* Se usa `DISTINCT` para no países repetidos.

### 2) High air quality

Buscar todas las columnas en las que `pollution` es 0.

---

## Aprendizajes

Aprendí a filtrar la información de una tabla con `SELECT`, `FROM` y `WHERE`. También aprendí a limitar el tamaño de las consultas y a estimar el tamaño de las mismas.

Además de usar `DISTINCT` con `SELECT` para no mostrar datos repetidos.

