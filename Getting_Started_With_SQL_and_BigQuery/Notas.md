# Getting Started With SQL and BigQuery

## Keywords

1. SQL(Structured Query Language)
2. BigQuery
   * Llamada a BigQuery desde Python
   * Contruir una referencia
   * Sincronización de datos
   * Proyecto
   * Colección de tablas(dataset)
3. Esquema de entidades
4. Esquema(Schema)
   * SchemaField

## Notas

1. SQL es un lenguaje de programación que se utiliza para comunicarse con y manipular bases de datos.
   * Es el lenguaje estándar para la gestión de bases de datos relacionales.
2. **BigQuery** es un servicio de Google Cloud que permite a los usuarios analizar conjuntos de datos grandes y complejos. O sea, es un servicio web.

* El **cliente es el objeto principal** para interactuar con BigQuery. Se utiliza para enviar y recibir datos de BigQuery.

* En BigQuery, los datos se almacenan en **proyectos**.

* Cada dataset es una **colección de tablas**. Filas y columnas.

3. El **Cliente** contiene proyectos.
  * Los **proyectos** contienen **datasets**.
    * Los **datasets** contienen **tablas**.
![Jerarquia de BigQuery](JerarquiaBigQuery.bmp)

4. La estructura de la tabla se llama **esquema**.

* **SchemaField** es un objeto que contiene información sobre una columna específica en una tabla.
  * **Datos**:
    * **nombre**
    * **tipo de datos**
    * **modo**: NULLABLE(def, permite valores nulos), REQUIRED, REPEATED
    * **descripción** acerca del dato en ese campo

En este caso de revisaron los primeros 5 registros de la tabla.
`to_dataframe()` convierte los resultados en un DataFrame de Pandas.
Devuelve `RowIterator` -> objeto que se puede convertir en un DataFrame con pandas.

## Resumen

* ¿Cómo llamar a BigQuery desde Python?

  ```python
  from google.cloud import bigquery
  client = bigquery.Client()
  ```

* **Construir una referencia** al dataset que se quiere consultar y **sincronizar los datos**.

  ```python
  dataset_ref = client.dataset("hacker_news", project="bigquery-public-data")
  dataset = client.get_dataset(dataset_ref)
  ```

* **Listar las tablas** en el dataset.

  ```python
  tables = list(client.list_tables(dataset))
  for table in tables:
      print(table.table_id)
  ```

* ¿Cómo obtener el esquema de una tabla?

  ```python
  table.schema
  ```

Contiene los metadatos de la tabla.

`table.table_id` es el nombre de la tabla.

* **Listar las columnas** de una tabla.

  ```python
  table_ref = dataset_ref.table("comments")
  table = client.get_table(table_ref)
  ```
`client.get_table(table_ref)` es un API request. Igual que `client.get_dataset(dataset_ref)`.

* ¿Cómo imprimir el esquema de una tabla?

  ```python
  for schema_field in table.schema:
      print(schema_field)
  ```

* ¿Cómo chequear las primeras lineas de una tabla?

  ```python
  client.list_rows(table, max_results=5).to_dataframe()
  ```

* Para ver una columna específica del esquema de la tabla.

  ```python
  table.schema[1]
  ```

  O limitando a 5 registros.

  ```python
  client.list_rows(table, selected_fields=table.schema[:1], max_results=5).to_dataframe()
  ```


## Ejercicio

El ejercicio de esta lección consiste en utilizar los métodos aprendidos para obtener información de una tabla de BigQuery.
El ejercicio y sus respuestas se encuentra en el archivo [ejercicio de la leccion](exercise-getting-started-with-sql-and-bigquery.ipynb)

### Pregunta 2
Acordarse que en una tabla `data` se refiere al tipo de dato. Los tipos de datos son:

* `STRING`
* `INTEGER`
* `FLOAT`
* `BOOLEAN`
* `TIMESTAMP`

## Aprendizajes

Aprendí a usar los métodos de la librería `google.cloud.bigquery` para obtener información de una tabla de BigQuery.
Ahora sé que es un Clinte, un Proyecto, un Dataset y una Tabla en BigQuery. Además de la navegación básica de el esquema de una tabla. También un poco de visualización de los datos.
