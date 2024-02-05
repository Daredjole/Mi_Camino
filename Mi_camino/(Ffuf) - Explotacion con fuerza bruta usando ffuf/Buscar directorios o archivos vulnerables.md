Este es un ataque simple y consta de lo siguiente:

``` bash

ffuf -w /home/juan/desktop/juan/wordlists/php-fuzz.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ
```

En este caso -w sirve para indicar la ruta de las wordlists, la cual en mi caso lo guardo en /home/juan/desktop/juan/wordlists
