## Operadores ";", "||" y "&&"

- "||" En bash este operador logico es lo equivalente al "OR" de otros lenguajes de programacion y se usa principalmente para  para determinar opciones o alternativas, por ejemplo:
````bash
if [[ $hola == $no ]] || [[ $hola -gt $no ]]
then
echo hola;
`````


## Uso de read para hacer inputs en scripts Bash

- '-s' se usa para que lo que escribe el usuario no se vea en pantalla
- '-p' se usa para agregar un texto 
- '-n' Se usa para que solo use los los primeros caracteres que ingresa el usuario por ejemplo, "read -n 3 vari", digamos que el usuario ingresa "hola", en la variable vari solo se guardara "hol" 

Estructura
## Comparadores numericos

Para comprar valores numericos en Bash se usan los siguientes comparadores

- '-eq': Igual a (equal)
-  '-ne': No igual a (not equal)
- '-lt': Menor que (less than)
- '-le': Menor o igual que (less than or equal to)
- '-gt': Mayor que (greater than)
- '-ge': Mayor o igual que (greater than or equal to)


