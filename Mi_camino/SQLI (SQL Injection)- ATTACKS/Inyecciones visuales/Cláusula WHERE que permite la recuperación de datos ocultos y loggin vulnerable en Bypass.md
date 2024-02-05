
## Inyección en URL

En este tipo de vulnerabilidad SQLi se pueden ver mas datos los cuales esten ocultos por la consulta, la cual puede ser algo asi:

	SELECT * FROM products WHERE category = 'Gifts' AND released = 1

Donde released es el estado de la publicación, si esta oculto o publico, publico siendo 1, por lo tanto la consulta llama a todos los productos que tengan el estado 1 y pertenezcan a la categoria de "Gifts", el Payload correspondiente para este tipo de vulnerabilidad seria algo semejante a esto:

	SELECT * FROM products WHERE category = 'Gifts' OR 1=1-- -' AND released = 1

La primera comilla sirve para cerrar la comilla anterior y luego la sentencia " OR 1=1" al ser verdadera, hace que la categoria no se tome en cuenta y los guiones que estan despues sirven para comentar " ' AND released = 1", haciendo que no se interprente la comilla restante, ni el estado de la publicación, tambien se podria poner solo lo siguiente

	SELECT * FROM products WHERE category = 'Gifts'-- -' AND released = 1

Pero esto solo haria que se presenten todos los productos en la categoria seleccionada, el problema es que la idea es ver todos los datos, si, esto ayudaria a ver mas productos de lo normal, pero no todo lo que neesitamos, en cambio a usar " ' OR 1=1-- -" no se interpretaran ninguna de las sentencias

--- 
## Inyección en loggin

Estas tecnicas de inyeccion tambien se pueden usar en loggins Bypass vulnerables, por ejemplo, supongamos que hay un usuario "admin" con la contraseña "$uperadmin", si hacemos lo siguiente podremos saltarnos la contraseña:

	Username: admin'-- -
	Password: 1234

Esto es porque la query sin inyectar se ve algo asi: 

	SELECT * FROM Users WHERE username='admin' AND password='$uperadmin' 

Y la query inyectada algo asi:

	SELECT * FROM Users WHERE username='admin'-- -' AND password='1234'

Como se puede ver aqui, se esta comentando la parte en la cual esta la contraseña por lo tanto no importa lo que pongas ahi, de todas formas te vas a loggear con el usuario que especificaste, tambien puedes usar lo siguiente, aunque en algunos casos no suele funcionar, como tambien en otros suele funcionar mejor que simplemente comentar la contraseña, y la manera a la que me refiero es esta:

	SELECT * FROM Users WHERE username='admin' OR 1=1-- -' AND password='1234'













