Al encontrar los puertos indicados por el primer escaneo basico de puertos TCP es facil seuguir el hilo segun nuestras necesidades, pero generalizando, siempre es importante saber las versiones de cada protocolo, si hay una pagina web cual es el dominio.

En este caso el tema se acompleja bastante mas, ya que al ser un escaneo mas preciso existen decenas de parametros para hacer nuestro scaneo lo mejor posible, en mi caso para la maquina Pov de Hack the box, realice el siguiente scaneo: 

``` shell
nmap -p80,22 -sCV -vvv -T5 10.10.11.251 -oN avansc
```

- -sC : A pesar de que en el ejemplo se vea junto (basicamente porque son escaneos que se pueden juntar para ahorrar tiempo), son comandos completamente distintos "-sC" y "-sV", en este caso -sC sirve desplegar un conjunto de paquetes de reconocimiento pordefecto, para ver las distintas propiedades de cada puerto seleccionado.

- -sV : Este parametro sirve para ver las versiones de cada protocolo, esto nos puede ayudar a buscar CVE's en la etapa de ataque

- -T5 : Este parametro cumple una funcion similar a la que vimos en reconocimiento basico de puertos TCP, solo que este parameto hace que el scaneo en general sea mas regulable, siendo 5 el modo mas agresivo y 0 el mas lento, en entornos como Hack the box, lo ideal es usar un scaneo agrsivo, ya que no pasa absolutamente por ser un entorno controlado

- -oN : En este parametro como en "-oG" (output Grepeable), la "o" representa la salida o output, y la letra grande, o sea la "N" representa el formato, en este caso es Normal

Cabe aclarar que en este caso solo scanee los puertos 80 y 22 pero en otras maquinas hay hasta 10 puertos abiertos por lo tanto se pueden agregar los que hagan falta.

