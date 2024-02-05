Para esto utilizaremos un ataque UNION, el cual podrá concatenar columnas de otras tablas, lo cual nos ayudara a observar cosas como la versión de la base de datos, en el caso de Oracle, los nombres de las tablas en las cuales se almacenan la versión de la base de datos, son las siguientes: 

	SELECT banner FROM v$version
	SELECT version FROM v$instance

Pero para poder lograr que se muestren estos datos y no salga un error tenemos que saber las columnas que ya ha llamado el servidor antes de que hiciéramos la inyección, ya que para usar UNION se necesita que las columnas seleccionadas sean de la misma cantidad, para saber cuantas columnas se necesitan para que la consulta sea correcta solo basta con usar "ORDER BY" de la siguiente manera:

	' ORDER BY 1-- 
	' ORDER BY 2--
	' ORDER BY 3--
	etc...

Hariamos esto hasta que el servidor de un error, ¿Que esta pasando aqui?, bueno, supongamos que la query la cual estamos inyectando luce algo asi: 

	SELECT * FROM shop WHERE category='Gifts' ORDER BY 1--' 

en este caso se esta organizando la información respecto a la columna 1, al ir incrementando este numero eventualmente dará error ya que no existirá tal columna, supongamos que aqui solo hay 3 colmnas, entonces al poner "ORDER BY 4--" daria error ya que no existe una cuarta columna, asi sabriamos que contiene 3 columnas, por lo tanto si queremos saber la version del servidor tendriamos que hacer algo asi:

	SELECT * FROM shop WHERE category='Gifts' UNION SELECT banner, NULL, NULL FROM v$version-- 

Lo que yo hago es rellenar los espacios faltantes con datos nulos, tambien se puede remplazar esos "NULL" por " 'a' "

## En bases de datos Microsoft/MySQL

Primero averiguamos cuantas columnas tiene la tabla que se esta llamando desde un inicio usando "ORDER BY" en el siguiente caso solo hay 2 columnas ya que en "ORDER BY 3" da error, revisando la [[Hoja de trucos SQLi]] vemos que para ver la versión de la base de datos en estas bases de datos no es necesario llamar a una tabla en especifico, tambien aparece que la forma de comentar para MySQL es distinta que en Oracle y PostgreSQL, en caso de MySQL a pesar de parecer lo mismo hay que tener muy en cuenta que hay que poner un espacio al final, lo que yo hago es poner un espacio y un guion para asegurarme que funcione correctamente, por lo tanto la query quedaria algo asi al inyectar el Payload:

	SELECT * FROM products WHERE category='Gifts' UNION SELECT @@version, NULL-- -'

