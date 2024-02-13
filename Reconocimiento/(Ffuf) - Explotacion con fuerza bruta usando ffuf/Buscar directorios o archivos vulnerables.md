Este es un ataque simple y consta de lo siguiente:

``` shell

ffuf -w /home/juan/desktop/juan/wordlists/php-fuzz.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ
```

<img src="..\Z-Imagenes\image.png" />


En este caso -w sirve para indicar la ruta de las wordlists, la cual en mi caso lo guardo en /home/juan/desktop/juan/wordlists y u para indicar la url, los dos puntos siven como indicativo de la palabra clave que se va a usar para realizar el fuzzeo, ffuf es de uso bastante manual, lo cual me gusta mucho, principalmente porque puedes configurar varios filtros o para metros, eso lo veras en este mismo modulo, en el documento llamado "Filtrar resultados y redireccionamiento" 
