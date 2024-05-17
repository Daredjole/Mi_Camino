En esta seccion hablaremos sobre las protecciones contra Path-Traversal, en la seccion anterior ([Explotando una vulnerabilidad LFI basica](/Explotación/LFI/2-Explotando_una_vulnerabilidad_LFI_basica.md)) vimos como funciona se ejerce un ataque en el cual no hay protecciones especificas, pero claro que en la mayoria de los casos hay protecciones para evitar esto, y hoy veremos mas sobre estas protecciones y como burlarlas, entonces..., !Empecemos¡.

## Reemplazado de ruta reversa "../" 

Como programador Backend, ¿Que es lo primero que harias para evitar un Path-Traversal?, pues simple, intentarias eliminar de raiz el problema, ! y nunca mejor dicho lo de raiz¡, porque la primera proteccion se basa en eliminar los recorridos reversos de ruta "../" y remplazarlos por... !NADA¡, esto se veria de esta forma

```php
$protect = str_replace('../', '', $_GET['language']);
include('/ruta-a-la-carpeta/language/' . $protect)
```

En este caso si hacemos lo mismo que la vez pasada tendremos problemas, ya que la ruta quedaria algo como:

```
/ruta-a-la-carpeta/language/etc/passwd
`````

Obviamente esta ruta no existe, entonces, ¿que podemos hacer en este caso?, es simple, solo tenemos que usar "....//" porque al no ser una eliminacion recursiva (es decir, que no se repite con el resultado del proceso anterior) pues se elimina el "../" de en medio y queda "../", esta tecnica tambien sirve con "..././", recuerda que el programador pudo haber hecho un  reemplazo al resultado de lo anterior haciendo que "....//" no sirva, pero si no es un bucle que se repite hasta que elimine todos los "../", podemos usar "......///", y asi hasta que funcione:

<img src="/Z-Imagenes/LFI7.png" />

## Eliminado de ruta reversa "../" a traves de filtros web

Algunas webs tienen implementado un sistema para evitar ataques LFI, eliminado puntos o barras acostadas "/", Para esto tambien hay una forma de burlar esta proteccion (aunque no siempre funciona) consiste en codificar "../../../etc/" en URL-encode esto puede confundir a esta proteccion haciendo que pase como valida nuestra carga, para esto podemos usar el decoder de Burpsuite: 

<img src="/Z-Imagenes/LFI8.png" />

Luego meteriamos esta carga en el input vulnerable: 

<img src="/Z-Imagenes/LFI9.png"/>

Como pueden ver funciona perfectamente.

## Validacion a traves de coincidencias (Expresiones regulares)

Ahora hablare de una proteccion que esta en el backend por parte del propio codigo, esta proteccion consiste en usar expresiones regulares u otros medios para solo autorizar una cierta entrada de datos, en este caso hablare de las expresiones regulares ya que es lo mas comun, pero esto puede aplicar con cualquier sistema que haga un proceso similar al que hace la siguiente expresion regular. Se que es un poco repentino y tal vez dificil de entender, les voy a dar un ejemplo:

```php
if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) {
    include($_GET['language']);
} else {
    echo 'Illegal path specified!';
}
```

Bueno, antes que nada hay que aclarar que las expresiones regulares no son como tan un valor que le damos a una variable, sino que sirve mas bien como un condicional de lo que debe contener una variable, por eso es que en este caso se usa en un condicional, en el cual, si la expresion regular se cumple lanzara un valor "True" y si no se cumple, lanzara un valor "False", ya sabiendo esto vamos a desglosar poco a poco a que hace referencia esta expresion regular, si te quedan dudas sobre expresiones regulares te recomiendo visitar el modulo [Expresiones regulares](/Scripting-y-lenguajes-de-programacion/Expresiones-Regulares).

```php
 /^\.\/languages\/.+$/
```

- La primera y ultima barra "/" se usan para delimitar expresiones regulares, es decir, determinar el inicio y el final.

- Luego el signo "^" indica el inicio de la cadena, es decir, lo que le sigue a esto es con lo que tiene que empezar la cadena.

- Las barras "\" sirven para escapar de caracteres especiales, y como el punto "." es un caracter especial, usando la barra "\" se va a interpretar como un caracter normal.

- El "." suele hacer referencia a que en ese espacio puede haber cualquier caracter, pero como escapamos de el con la barra "\" anterior, queda como un punto "." normal.

- Las barras "/" que estan al inicio y final de la palabra "languages" se toman como caracteres normales y no como caracteres especiales gracias a las barras "\".

- El punto "." indica que puede haber cualquier caracter despues de "/languages/", combinado con el mas quiere que decir que deben haber uno o mas caracteres.

- El signo de dolar "$" indica el final de la expresion regular, es decir, que se tiene que coicidir con el final de esa expresion regular que seria ".+" (uno o mas caracteres despues de "/languages/")


Entonces, despues de toda esta biblia, ¿Cual es el punto?, es decir, como se burla esta proteccion, pues es simple, teniendo entendido que lo unico que se necesita cumplir es que necesitamos partir de la carpeta "/languages/" pues es logico que lo unico que tenemos que hacer es simplemente poner en la consulta "/languages/../../../../etc/passwd" y puede que te preguntes, pero como yo se que esta expresion regular esta por detras, bueno, no hay una forma especifica de saberlo, las expresiones regulares son tan solo una de las formas de comprar y/o validar texto, te daras cuenta que se esta validando algo en especifico porque en el propio campo por defecto va a aparecer que parte de esa carpeta, aqui un ejemplo para que se entienda mejor:

<img src="/Z-Imagenes/FLI10.png" />

Como pueden ver, por defecto la pagina pone en el input del usuario la carpeta languages seguido del lenguaje seleccionado, y esto es logico, ya que las paginas estan diseñadas para guiar al usuario, imaginense lo que seria adivinar que penso el desarrollador de una pagina para poder navegar en ella, !! Seria terrible ¡¡, y nosotros como pentesters podemos aprovecharnos de esto, por eso siempre hay que navegar la pagina como haria un usuario para ir notando como se comporta esta, y asi hacernos una idea de como funciona por detras, en fin, despues de todo esto, asi se veria con el ataque realizado: 

<img src="/Z-Imagenes/LFI11.png" />

## Extension de archivo previamente definida

### ¿Que hacer si es un servidor actualizado?

Esta proteccion es bastante molesta ya que no se puede evitar como tal, o al menos no si el servidor esta correctamente actualizado, y solo nos podremos limitar a usar archivos terminados en la extension que haya definido el programador.

### ¿Que hacer si el servidor no está actualizado? (Y el LFI parte de PHP)

En caso de que el servidor no este actualizado nos podemos aprovechar de que en el pasado el lenguaje PHP tenia una vulnerabilidad y esque debido a las limitaciones de 32 bits solo podia soportar una cantidad de datos especifica  en sus variables, lo que salga de ese limite no se tomaba en cuenta, por cierto, el limite es 4096, otra cosa es que PHP en versiones anteriores eliminaba los puntos y barras que estuvieran de forma individual, es decir "././" ya que esto literalmente expresa el directorio actual, tambien se eliminaban las barras diagonales repetidas "/example-folder////////language" se tomaba solo como "/example-folder/language" de hecho esto aun sigue pasando en las terminales aqui un ejemplo para que me entendan mejor: 

<img src="/Z-Imagenes/LFI12.png"/>

Ya entendiendo esto podremos burlar este agregado de extension, ¿Como?, bueno es simple, aca hay un ejemplo de como se hace este agregado de extension en PHP:

````php
include($_GET['languages'] . ".php");
````

Entonces, lo logico y primero que se me ocurre con lo anterior explicado es que podemos usar este limite de caracteres para hacer que la parte de ".php" no se interprete, y podemos llegar a ese limite usando puntos y barras individuales "./", y se que te preguntaras lo mismo que yo me pregunte al inicio,¿ Al eliminarse todas esas barras y puntos individuales no quedaria espacio para el ".php", haciendo que se interprete?, pues aparentemente no, y supongo que, primero, al no caber en el buffer se elimina directamente y luego se hace el tratado de informacion en el que se eliminan esos puntos y barras diagonales individuales, haciendo que quede como una consulta limpia, ahora, como podemos generar esa cantidad de puntos y barras individuales, !!Podemos usar un script en bash¡¡: 

```bash
echo -n "non_existing_directory/../../../etc/passwd/" && for i in {1..2048}; do echo -n "./"; done
```

Este comando es bastante simple, lo desglosare para que se entienda facilmente pero si quieres saber mas sobre bash te recomiendo visitar el modulo [Bash](Scripting-y-lenguajes-de-programacion/Bash), voy a desglosar este comando para que se pueda entender facilmente:

- "echo" es un comando usado en distribuciones de linux para mostrar texto en pantalla

- "-n" sirve para desplegar opciones de comandos, en este caso este interruptor hace que cuando se use echo no ponga un salto de linea al final del comando

- El texto entre comillas es el contenido que se quiere mostrar

- "&&" es un operador logico en bash para ejecutar otro comando usando la mima linea

- Ahora hay un bucle de tipo for en el cual se define como variable interna del bucle la "i" pero en este caso no usaremos esta variable, solo usaremos el bucle para que se repita 2048 veces el comando seguido de "do"

- Por ultimo, gracias al bulce se imprime 2048 veces "./" delante del texto que pusimos en el primer comando ("non_existing_directory/../../../etc/passwd/"), el resultado seria "non_existing_directory/../../../etc/passwd/./././././././<Hasta repetirse 2048>"

Aqui el resultado grafico de todo el comando: 

<img src="/Z-Imagenes/LFI14.png" heiht="250" weigth="500" />

¡¡Y listo!!, ya tenemos nuestra carga util lista para burlar esta proteccion, aqui como se ve con el ataque realizado: 





