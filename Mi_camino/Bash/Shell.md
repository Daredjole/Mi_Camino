
## Tuberias (pipes) en shell, y como funcionan

En Bash las tuberias se usan para pasar el resultado de un comando a otro , por ejemplo el siguiente comando

<img src="/Mi_camino/Z - Imagenes/LFI4.png" />

Como podemos ver en este caso se pasan los resultados de cada operacion, echo "echo hola" pasa el resultado "echo hola" a base64 el cual lo convierte en "ZWNobyBob2xhCg\==" lo cual se envia para decodificarse, lo cual lo devuelve a "echo hola" y esto se pasa a bash para que lo interpretem y asi funcionan las tuberias en bash.