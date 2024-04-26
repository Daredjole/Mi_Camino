un servidor utiliza distintos lenguajes parar mostrar ciertas cosas, estos lenguajes pueden ser, PHP, Javascript, Java, .net, entre otros, usualmente se suelen usar una cierta variedad de funciones que llaman a archivos locales del servidor con la función de que dichos archivos se representen en la pagina, usualmente suelen ser imágenes o plantillas, cuando el programa llama directamente, y sin filtrar ni sanitizar correctamente la ruta usando alguna de las funciones que están en la [[Hoja de funciones LFI]] se puede inyectar código maliciosos que nos ayude a ver datos privilegiados y mucho mas, en este caso solo veremos acerca de ver algunos datos para mostrar como es un LFI básico, usualmente para saber si se están llamando archivos del servidor la URL suele ver algo asi:

	http://<SERVER_IP>:<PORT>/index.php?language=es.php

en este caso se esta ejecutando un archivo .php llamado es para definir el idioma de la pagina, por detras el codigo se ve algo asi 

```php
include($_GET['language']);
```

como se puede ver en este caso simplemente se esta incluyendo la ruta sin ser filtrada ni sanitizada, usualmente el servidor muestra algo similar a esto: 

<img src="/Z-Imagenes/LFI1.png" />

Pero si cambiamos el es.php por otra ruta de archivo que conozcamos de la siguiente manera:

	http://<SERVER_IP>:<PORT>/index.php?language=/etc/passwd

Se podria ver asi:

<img src="/Z-Imagenes/LFI2.png" />

Esto nos puede ayudar a ver datos privilegiados usando rutas que ya conozcamos o inlcuso podemos convertir un LFI en un RCE pero es un tema en un futuro
