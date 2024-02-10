Lo mas usual es que la pagina contenga por lo menos algunos filtros o formas de sanitizar la funcion include(), algunos de los filtros los veremos aqui, todos estos son filtros basicos, en el futuro aprenderemos algunos filtros mas avanzados y otras formas de vulnerar una pagina con LFI.

---
## Concatenación previa de ruta

Por ahora empezaremos con el primer filtro el cual se basa en concatenar al inicio de la cadena una ruta especifica, como se puede ver en el siguiente ejemplo:

```php
include("./languages/" . $_GET['language']);
```

En este caso una consulta como la siguiente no podria funcionar 

	http://<SERVER_IP>:<PORT>/index.php?language=/etc/passwd

Ya que se la ruta por detrás del código se vería algo asi: 

	./lenguages/etc/passwd

Ruta la cual no existe, para evitar este filtro podemos hacer algo asi 

	http://<SERVER_IP>:<PORT>/index.php?language=../etc/passwd

En este caso solo se necesitaria un "../" pero siempre es preferible probar con varios e irlos disminuyendo hasta que deje de dar error, para asi saber cuantos directorios nos estamos saltando.

---
## Prefijo de nombre de archivo

En este caso el código funciona de la siguiente manera, el incluido concatena una porción del nombre de archivo y en la consulta se completa: 

```php
include("lang_" . $_GET['language']);
```

por lo tanto la ruta usualmente quedaría algo como 

	./lang_es.php

o en caso del ingles

	./lang_en

Lo ideal en estos casos es hacer que la consulta empiece por "/", aqui hay un ejemplo de esto: 

	http://<SERVER_IP>:<PORT>/index.php?language=/../../../etc/passwd

Pero lo mas probable es que no funcione porque para funcionar se necesitaría un directorio llamado "lang_", en el futuro discutiremos sobre como podemos evitar este tipo de cosas con tecnicas mas avanzadas.

---
## Extensiones adjuntas 

En este caso de filtro no hay mucho que hacer, lo cual limita nuestras posibilidades pero aun nos podra seguir siendo util ya que aun asi podremos ejecutar archivos .php, hay un par de formas de evitarlo pero es para versiones antiguas de PHP, el filtro se ve algo asi:

```php
include($_GET['language'] . ".php");
```

Como se puede observar en este caso, se concatena la extensión del archivo, lo que haría que leer archivos como por ejemplo /etc/passwd, se interpretara como /etc/passwd.php, lo cual no existe, aun asi podemos intentar un par de cosas.
### Truncamiento de ruta 

Lo interesante de las versiones antiguas de PHP como [[PHPv5.3]] y [[PHPv5.4]]es que tiene un limite de caracteres para las cadenas (Strings) el cual es de 4096 caracteres, probablemente eso se deba a  las limitaciones de los sistemas de 32 bits, ademas, PHP tambien solía eliminar barras diagonales y puntos al final de la ruta ("/etc/passwd/.")  PHP lo interpretaría como("/etc/passwd"), en PHP y Linux se ignora el  uso de varias barras invertidas en la ruta, por ejemplo ("///////etc/passwd") lo cual quedaría como "("/etc/passwd") usando estas dos cosas podemos lograr trucar el .php que se concatena al final de la ruta, y esto lo haremos de una forma similar a esto:

```url
?language=non_existing_directory/../../../etc/passwd/./././.[./ repetido ~2048 veces]
````

Porque hacemos esto?, bueno, como explique mas arriba ("./") no se interpreta ni en medio ni al final de la ruta por lo que lo ponemos usar para llegar al limite de 4096 y hacer que la parte de .php se truquee, por lo tanto no se interprete y por todos los ("./") no hay que preocuparse ya que el PHP tampoco los interpreta, esa es la primera forma de evitar el .php.

### Bits nulos

Las versiones anteriores [[PHPv5.5]]  eran vulnerables a injeccion de bytes nulos, como por ejemplo "%00" esto es porque la memoria en donde se almacenan las cadenas ("Strings")es una memoria de bajo nivel, lo que quiere decir que se debe indicar el final de la cadena con "%00" asi como en lenguajes como: ensamblador, C y C++, entonces, sabiendo esto podemos usar esos bytes nulos para terminar la cadena al final de nuestra cadena y asi hacer que no se tome el ".php" final.

--- 
## Filtros transversales no recursivos 

En este caso se intentan eliminar las rutas transversales que hayan en la consulta, este codigo se ve algo asi: 

```php
$language = str_replace('../', '', $_GET['language']);
```

El problema aqui es que este remplazo no se hace de manera recursiva, un método recursivo quiere decir que se aplica de nuevo al resultado de haberlo aplicado previamente, en este caso al no ser de manera recursiva solo se reemplaza una vez y luego de eso pasa directamente a la funcion include, la manera de evitar este filtro es muy simple, solo basta con escribir "....//" en vez de "../" esto hara que se reemplace lo de en medio y quede "../", el resultado se veria algo asi:

<img src="../Z-Imagenes/LFI3.png">

---

## Codificación

Otra forma de escapar de algunos filtros como el anterior visto es usando la herramienta decoder de Burpsuite para hacer un URL encoding a la carga útil y que el programa no represente directamente los caracteres lo que haría que se interprete sin ser reemplazado.

---
## Caminos aprobados

En este caso se usa a través del código PHP una función que compara una expresión regular con el resultado de la consulta que hayamos enviado, lo cual hace de la siguiente manera:

```php
if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) {
    include($_GET['language']);
} else {
    echo 'Illegal path specified!';
}
```

Sé que se ve algo confuso, pero hablando en expresiones regulares es bastante simple, la barra invertida hacia la derecha ("/") que esta al principio y al final, sirve para indicar que es una expresión  el simbolo de la flecha hacia arriba ("^") sirve para indicar que lo que le sigue tiene que estar de primeras en la cadena, las barras invertidas a la derecha no se interpretan, y sirven para expresar que cosas que normalmente son caracteres especiales como por ejemplo el punto o la barra invertida a la derecha ("") hacen parte del texto y no son caracteres especiales, a esto se le llama "escape" en este caso se esta escapando del punto, lo que significa que el punto hace parte del texto, luego de eso se hace lo mismo con las barras invertidas a la derecha para que no se tomen como caracteres especiales, y como se puede ver al final hay un punto al final del cual no se escapa, es porque este punto sirve para representar una coincidencia de uno o mas caracteres de cualquier, y por ultimo el signo de dolar "$" sirve para representar que eso que represento el punto tiene que estar al final de la cadena, es decir, despues de "./languages", la barra invertida a la derecha ("/") que hay al final, sirve para representar el final de la expresion regular.

En este caso es simple escapar de este filtro, solo basta con dejar "lenguages" como parte de la consulta y luego usar rutas transversales ("../") para volver al directorio raíz o a donde se necesite ir.