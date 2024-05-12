A partir de aqui empieza lo divertido, ya con los conceptos basicos de como una pagina web incluye archivos para su visualizacion o ejecucion, podremos expotar una vulnerabilidad basica LFI, los mas atentos ya se imaginaran que si no hay ningun tipo de sanitizacion, podremos acceder a cualquier archivo del cual tengamos su nombre, usualmente, para comprobar si existe una vulnerabilidad LFI basica se suele inyectar la ruta directa a /etc/hosts, aqui un ejemplo:

<img src="/Z-Imagenes/LFI2.png" />

pero como se imaginaran, como mucho podremos ver que usuarios hay en el back-end por lo tanto, eso que mostre anteriormente no es un ataque, se puede tomar mas como una prueba para confirmar la existencia de una posible vulnerabilidad LFI, por lo tanto usare la ruta /etc/passwd para simplicar tecnicas que vayamos aprendiendo, en la imagen que enseñé anteriormente se muestra como se hace acceso directo a la ruta "/etc/passwd" usando la barra "/" para ir directo a la raiz, pero de vez en cuando esto no funciona ya que la aplicacion por atras puede hacer algo como esto:

````php
$var = $_GET['language']
include("/ruta_hasta_la_carpeta/$var")
````

Que pasa con esto, bueno, que si ponemos /etc/passwd en la entrada de usuario, nos va a dar un error o decir que no se encontro, ¿porque?, bueno, es simple, en sistemas linux, y si no estoy mal en sistemas windows tambien, en las rutas, dos barras diagonales "//" se toman como una sola lo mismo pasa con las barras invertidas es decir "\\\\", por lo tanto el sistema o el propio php en este ejmplo lo toma como "/ruta_hasta_la_carpeta/etc/passwd" y como ya imaginaran, tal cosa no existe, que se puede hacer para solucionar esto, bueno, pues se puede hacer lo siguiente:

<img src="/Z-Imagenes/LFI6.png" />

¿Que es esto?, bueno, es simple aqui en vez de partir de la raiz usamos la expresion "../" lo que hace esta expresion es que hace un recorrido de ruta reverso es decir, si estamos en el directorio /home/juan/hacked, una forma de ir al directorio juan puede ser "/home/juan", pero otra puede usar este recorrido de ruta reverso, el cual quedaria algo asi "/home/juan/hacked/../", en este caso estariamos en el directorio "hacked" y usando "../" nos devolveriamos a juan, ¿porque y como funciona esto?, bueno, lo explico en la seccion [Rutas y directorios](/Sistemas_Operativos/Linux/Rutas_y_directorios.md). El caso es que esto nos servira en caso de que haya un directorio predefinida por codigo, usando este par de tecnicas basicas podremos ejercer Path Traversals basicos, ahora subamos el nivel viendo los [Filtros contra Path-Traversal](/Explotacion/LFI/3-Filtros_contra_Path-Traversal.md)
