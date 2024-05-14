En esta seccion hablaremos sobre las protecciones contra Path-Traversal, en la seccion anterior ([Explotando una vulnerabilidad LFI basica](/Explotación/LFI/2-Explotando_una_vulnerabilidad_LFI_basica.md)) vimos como funciona se ejerce un ataque en el cual no hay protecciones especificas, pero claro que en la mayoria de los casos hay protecciones para evitar esto, y hoy veremos mas sobre estas protecciones y como burlarlas, entonces..., !Empecemos¡.

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

<img src="/Z-Imagenes/FLI7.png" height="250" weigth="500" />

Algunas webs tienen implementado un sistema para evitar ataques LFI, eliminado puntos o barras acostadas "/", Para esto tambien hay una forma de burlar esta proteccion (aunque no siempre funciona) consiste en codificar "../../../etc/" en URL-encode esto puede confundir a esta proteccion haciendo que pase como valida nuestra carga, para esto podemos usar el decoder de Burpsuite: 

<img src="/Z-Imagenes/FLI8.png" height="250" weigth="500" />

Luego meteriamos esta carga en el input vulnerable: 

<img src="/Z-Imagenes/FLI9.png" height="250" weigth="500">

Como pueden ver funciona perfectamente, ahora hablare de una proteccion que esta en el backend por parte del propio codigo, esta proteccion consiste en usar expresiones regulares u otros medios para solo autorizar una cierta entrada de datos 

