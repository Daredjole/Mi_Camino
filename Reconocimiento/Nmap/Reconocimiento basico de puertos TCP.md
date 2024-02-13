Nmap es probablemente la herramienta mas usada por hackers, y como no, hoy aprenderemos a usarla de manera correcta.

Aprender a usar Nmap es como entrar a un laberito de 1000 hectarias, no sabes ni por donde empezar, pero podemos orientarnos juntos  para entender esta maravillosa herramienta, en mi caso, siempre que me veo frente a un objetivo, inicio con un escaneo simple de puertos TCP, esto para luego, grepear estos puertos y usarlos en un escaneo un tanto mas avanzado para obtener informacion sobre cada uno de ellos, este modulo trata sobre ese primer escaneo, entonces, sin mas chachara empecemos.

Usaremos como primera referecia el siguiente comando: 

``` shell
nmap -p- --min-rate 5000 -n <ip> -oG puertosscaneados
```

desglosando este comando vemos:

- -p- : el parametro "-p" sirve para indicar los puertos objetivo, cuando quieres escanear todos los puertos existentes (65535) es tan simple como poner un guion al lado en vez de un puerto especifico, auqnue tambien podemos escanear todos los puertos de la siguiente forma: -p1-65535