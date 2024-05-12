LFI, las sigas de Local File Inclusion, es una tecnica en la cual un atacante aprovecha una funcion del servidor la cual llama algun archivo en especifico para acceder a archivos privilegiados del sistema, o incluso ejecutar codigo de forma remota, lo que en otras palabras es explotar un RCE (Remote Code Execution), pero eso ya es mas avanzado (que igualmente veremos en este modulo), el orden del modulo sera el siguiente: 

- 1 [¿Que es una vulnerabilidad LFI?](/Explotación/LFI/Que_es_una_vulnerabilidad_LFI.md)
- 2 [Explotando una vulnerabilidad LFI basica](/Explotación/LFI/2-Explotando_una_vulnerabilidad_LFI_basica.md)
- 3 [Filtros contra Path-Traversal](/Explotacion/LFI/3-Filtros_contra_Path-Traversal.md)
- 4 ¿Que son los PHP wrappers?
- 5 ¿Que es una vulnerabilidad RFI?
- 6 Explotando una vulnerabilidad RFI y RCE
- 7 Envenenamiento de registros

Siguiendo con la explicacion de que es un LFI el cual se puede unir o dividir en otras tecnicas, por ejemplo, a pesar de que el LFI y el RFI son tecnicas distintas, ambos se basan en una misma vulnerabilidad que permite cargar y ver archivos, asi que para facilitar este modulo diremos que el RFI deriva del LFI, hice un esquema que hara mas facil de entender las tecnicas que se veran en este modulo: 

<img src="/Z-Imagenes/diagramafli1.png" height="250" weigth="500" />

Como pueden ver, de el LFI pueden desencadenarse diversos ataques y por eso es una vulnerabilidad tan grave, Empezaremos hablando de lo que es el LFI como tal.

El LFI o (Local File Inclusion) es una vulnerabilidad en la cual el servidor permite acceder a archivos locales de este, esto puede generar una gran variedad de problemas, como permitir ver el codigo fuente, ver archivos privilegiados, usar wrappers para hacer un SSRF (si no sabes que son los wrappers no te preocupes ya que lo veremos mas adelante), e incluso, en  los casos mas extremos, podremos inyectar y ejecutar codigo malicioso en el servidor, y te preguntaras, "¿Como funciona estas inlcusiones?", bueno pues un servidor utiliza distintos lenguajes parar mostrar ciertos contenidos, estos lenguajes pueden ser, PHP, Javascript, Java, .net, entre otros, usualmente se suelen usar una cierta variedad de funciones que llaman a archivos locales del servidor con la función de que dichos archivos se representen en la pagina, usualmente suelen ser imágenes o plantillas, cuando el programa llama directamente, sin filtrar y sin sanitizar correctamente la ruta usando alguna de las funciones que están en la [Hoja de funciones LFI](/Explotación/LFI/Hoja_de_funciones_LFI.md) (en la cual estan todas las funciones que permiten un LFI y con que permisos cuentan, de lenguajes como: NodeJS, Java, PHP y .NET ) se puede inyectar código maliciosos que nos ayude a ver datos privilegiados y mucho mas, en este caso solo veremos acerca de ver algunos datos para mostrar como es un LFI básico, usualmente para saber si se están llamando archivos del servidor la URL suele ver algo asi:

	http://<SERVER_IP>:<PORT>/index.php?language=es.php

en este caso se esta ejecutando un archivo .php llamado es para definir el idioma de la pagina, por detras el codigo se ve algo asi 

```php
include($_GET['language']);
```
Perfecto!, ahora que tenemos claro como actua un programa de una pagina web que incluye archivos podremos aplicar conocimientos [Explotando una vulnerabilidad LFI basica](/Explotación/LFI/2-Explotando_una_vulnerabilidad_LFI_basica.md)
