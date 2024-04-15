Lo primero que hice en este caso es buscar archivos .php con los que pudiera jugar ya que vi que el servidor auto completaba automáticamente con ".php" al final, por lo tanto no podia hacer otra cosa que usar archivos php, para esto procedí a [[Buscar directorios o archivos vulnerables]], y vi que habia un archivo llamado "configure.php" el cual al ejecutarlo no hacia nada, por lo tanto procedí a ver el código fuente, para esto usé [[PHP Wrappers]].

En php existen los Wrappers los cuales son usados usualmente por los desarrolladores para el manejo y transmisión de datos mediante protocolos o pseudo protocolos, esta trasmisión o manejo de datos puede ser remota o local, un ejemplo de trasmisión remota de datos es usando el protocolo "http://" y en el caso de archivos locales seria "file://" este tipo de cosas son muy útiles para los desarrolladores web pero tambien son muy beneficiosos para el pentesting web, hoy la meta es revelar el código detrás de un archivo .php, cuando hacemos una llamada a un archivo php con permisos de ejecución, este se ejecuta automáticamente ya que se esta usando un include, entonces, en casos en los que querremos averiguar el código de este tipo de archivos tendremos que hacer uso de los PHP wrappers, en este caso usare lo siguiente:

````php
php://filter/read=convert.base64-encode/resource=configure
````

Empecemos por partes, *php://* este es el wrapper que usaremos *filter/*  es un indicador que hace parte del wrapper *php://* , *read=* sirve para indicar el metodo con el que se manejara el archivo en cuestion, y *convert.base64-encode* es un filtro que hace parte de *filter/* , y por ultimo *resource=configure* hace referencia al recurso o archivo el cual pediremos que nos muestre codificado, en este caso no le puse el ".php" al final ya que el servidor auto completaba esto.

