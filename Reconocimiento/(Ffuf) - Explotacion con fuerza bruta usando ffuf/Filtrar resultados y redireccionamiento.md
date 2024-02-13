ffuf contiene diversos filtros para encontrar lo que buscamos, y en aqui veremos cuales son los mas usados y su correcto uso.

Usualmente estos filtros suelen usarse despues del primer fuzzeo simple, ya que necesitamos saber varias cosas, entre ellas esta lo siguiente

- Varios directorios los cuales siguen un mismo redireccionamiento
- Codigos de HTML que indiquen un redireecionamiento o algun contenido no deseado
- Directorios que contienen poca informacion
- directorios que tengan acceso denegado
- etc...

Los filtros que yo mas iuso personalmente son -fs, -fc -r false/true:

- -fs: es un filtro que evita resultados con un cierto tamaño, estte filtro despone de rangos, lo cual podemos usar de la siguiente forma: 

``` shell
ffuf -w <Path del wordlist> -u <url> -fs 2380-2890
```

obviamente tambien se puede hacer de manera individual dando valores precisos:

``` shell
ffuf -w <Path del wordlist> -u <url> -fs 2380
```
y por ultimo, tambien podemos hacerlo de manera listada:

``` shell
ffuf -w <Path del wordlist> -u <url> -fs 2380,2890,3000,etc..
```

- -fc: con este filtro podemos evitar ciertos codigos de error HTML (200, 300, 400, 500), las variaciones en el uso de este filtro son las mismas que en el filtro anterior por lo tanto no explicare esto de nuevo.

- -r: esto tecnicamente no es un filtro pero prefiero ponerlo aqui ya que es importante para el escaneo exitoso de directorios.

Este parametro permite seguir (o no) redirecciones (true/false), usualmente prefiero que NO siga redirecciones pero esto depende de muchos casos, sugiero hacer siempre los primeros ecaneos con este parametro en "true" y despues observando el comportamiento de la web decidir si desactivar ese parametro o dejarlo activado

``` shell
ffuf -w <Path del wordlist> -u <url> -r true
```