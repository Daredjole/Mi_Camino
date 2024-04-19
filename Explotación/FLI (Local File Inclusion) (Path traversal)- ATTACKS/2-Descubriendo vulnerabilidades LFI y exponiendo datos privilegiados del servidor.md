

como se puede ver en este caso simplemente se esta incluyendo la ruta sin ser filtrada ni sanitizada, usualmente el servidor muestra algo similar a esto: 

<img src="/Mi_camino/Z - Imagenes/LFI1.png" />

Pero si cambiamos el es.php por otra ruta de archivo que conozcamos de la siguiente manera:

	http://<SERVER_IP>:<PORT>/index.php?language=/etc/passwd

Se podria ver asi:

<img src="/Mi_camino/Z - Imagenes/LFI2.png" />

Esto nos puede ayudar a ver datos privilegiados usando rutas que ya conozcamos o inlcuso podemos convertir un LFI en un RCE pero es un tema en un futuro