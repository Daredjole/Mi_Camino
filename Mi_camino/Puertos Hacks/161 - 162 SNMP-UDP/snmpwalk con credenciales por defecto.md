El protocolo snmp contiene la credencial (contraseña) por defecto "public", como se puede ver a continuacion:


```php
snmpwalk -c public -v2c 10.10.11.248 
```

Recomiendo acompañar este código con una redirección de la entrada estándar y usando tee para mostrar información a través de la tty, claro que se va a ver de forma poco clara pero va a ser grepeable y documentable, esto se puede hacer de esta forma: 

```php
snmpwalk -c public -v2c 10.10.11.248 > snmp-mibs.out | tee /dev/tty 
```

Es una forma mucho mas profesional de manejar los datos por si te preguntas que podras conseguir con esto consulta el modulo [[Para que hackear el protocolo snmp]]


