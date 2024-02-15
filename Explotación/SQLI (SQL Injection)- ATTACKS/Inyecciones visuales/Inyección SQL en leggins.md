Estas tecnicas de inyeccion tambien se pueden usar en loggins Bypass vulnerables, por ejemplo, supongamos que hay un usuario "admin" con la contraseña "$uperadmin", si hacemos lo siguiente podremos saltarnos la contraseña:

	Username: admin'-- -
	Password: 1234

Esto es porque la query sin inyectar se ve algo asi: 

	SELECT * FROM Users WHERE username='admin' AND password='$uperadmin' 

Y la query inyectada algo asi:

	SELECT * FROM Users WHERE username='admin'-- -' AND password='1234'

Como se puede ver aqui, se esta comentando la parte en la cual esta la contraseña por lo tanto no importa lo que pongas ahi, de todas formas te vas a loggear con el usuario que especificaste, tambien puedes usar lo siguiente, aunque en algunos casos no suele funcionar, como tambien en otros suele funcionar mejor que simplemente comentar la contraseña, y la manera a la que me refiero es esta:

	SELECT * FROM Users WHERE username='admin' OR 1=1-- -' AND password='1234'

