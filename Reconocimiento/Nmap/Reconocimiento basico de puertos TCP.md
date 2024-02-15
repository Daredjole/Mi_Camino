map es probablemente la herramienta mas usada por hackers, y como no, hoy aprenderemos a usarla de manera correcta.

Aprender a usar Nmap es como entrar a un laberito de 1000 hectarias, no sabes ni por donde empezar, pero podemos orientarnos juntos  para entender esta maravillosa herramienta, en mi caso, siempre que me veo frente a un objetivo, inicio con un escaneo simple de puertos TCP, esto para luego, grepear estos puertos y usarlos en un escaneo un tanto mas avanzado para obtener informacion sobre cada uno de ellos, este modulo trata sobre ese primer escaneo, entonces, sin mas chachara empecemos.

Usaremos como primera referecia el siguiente comando: 

``` shell
nmap -p- -vvv --min-rate 5000 -n <ip> -oG puertosscaneados
```

desglosando este comando vemos:

- -p- : el parametro "-p" sirve para indicar los puertos objetivo, cuando quieres escanear todos los puertos existentes (65535) es tan simple como poner un guion al lado en vez de un puerto especifico, auqnue tambien podemos escanear todos los puertos de la siguiente forma: -p1-65535

- --min-rate 5000 : el parametro "--min-rate", comno el nombre indica, este parametro es para determinar la cantidad minima de paquetes por segundo que deben ser enviados, esto puede ser util por 2 razónes, una de ellas es que puedde servir para ocultarte de sistemas que puedan corttar tu trafico por ser demasiado agresivo, o porque puede alertar por un intento de hackeo, en casos de auditorias web lo mas recomendable es usar --min-rate 1000, pero al estar en un entorno controlado, como quiero agilizarlo al maximo, hago que envie 5000 paquetes por segundo.

- -n : este parametro ayuda igual que el anterior a mejorar el tiempo de descubrimiento de puertos, pero evidentemente al ser un parametro disttintto hace cosas distintas, en este caso, no hace resoluciones de DNS. 

- -oG : cuando queremos hacer un informe, o como yo mas lo uso, juntando el archivo resultante con la herramienta extractports, (una herramienta para sacar los puertos de un archivo grepeable de Nmap, desarrollada originalmente por s4vitar)

- -vvv : el parametro normalmente es "-v" lo que significa verbose, o sea, sirve para ver la informacion mientras se ejecuta el comando en vez de esperar a que acabe (el comando), la diferencia entre esos dos es que el parametro "-vvv" muestra mucha mas informacion del escaneo antes de concretarse

ese seria un escaneo basico de calle, el mas tipico, y es el que recomiendo usar como primer escaneo, puedes jugar con mas parametros pero lo veo un tanto innecesario como primer scaneo, claro que todo depende de tus necesidades.