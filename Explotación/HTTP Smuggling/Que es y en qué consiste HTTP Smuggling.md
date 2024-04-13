HTTP Smuggling es un ataque tan interesante como divertido, y es cuando un atacante envía una solicitud HTTP mal intencionada la cual puede pasar controles de seguridad engañando al servidor.

Pero, ¿en qué consiste HTTP Smuggling?, como se menciono arriba, HTTP Smuggling (a lo cual vamos a nombrar HTTPSmug para abreviar), consiste en engañar al servidor con solicitudes HTTP POST, aunque es muy probable que usaremos otra pero ya es otro tema.

Muchos servidores tienen implementado un servidor back-end y front-end, como dice el nombre se puede entender que el servidor back-end es el que se encarga de los procesos que no son vistos por el usuario en cambio el front-end se puede entender como todo lo contrario, todo aquello que el usuario  ve y con lo que interactua, como por ejemplo la pagina web en si, ahora, ya sabiendo esto podemos entemder mucho mejor como funciona HTTP Smuggling, hay que tener en cuenta que está vulnerabilidad afecta mayoritariamente a servidores que traten con solicitudes HTTP/1.1, también hay vulnerabilidades de este tipo con solicitudes HTTP/2 pero son muy contadas y no las tocaremos aquí, Okey, ahora, ¿Como los servidores tratan los datos de las peticiones HTTP/1.1 y como esto puede generar una vulnerabilidad?, bueno, para esto hay que entender que al usar HTTP/1.1 hay 2 tipos de delimitadores de datos, Transfer-Encoding y Content-Length, vamos a ver como funcionan estos delimitadores :D.

- ##Content-Length:

Este delimitador es muy simple de usar, te mostraré un ejemplo en el que se use y lo desglosaremos poco a poco.

<img src="/Z-Imagenes/HTTPS1.png" />

- Como podemos ver el Content-Length indica la cantidad de caracteres, pero la manera más exacta de denominar esto es que indica la cantidad de bytes, esto incluye en fin de linea y el salto de linea. Evidentemente el fin de este contenido queda  al final de admin. simple, ¿no?

- ##Transfer-Encoding:

Este delimitador también es fácil de entender pero un poco más complejo que el Content-Length

POST /admin HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked

21
password=heavypass123&user=admin
0

- Lo primero que podemos ver es que el parámetro Transfer-Encoding está establecido como "chunked", esto lo explicaré más adelante, ese "21" representa la longitud de el contenido que se quiere enviar en  hexadecimal, (incluídos los retornos de Carro y saltos de linea) y por último, una linea de valor nulo (0), acompañada de la secuencia \r\n\r\n 






