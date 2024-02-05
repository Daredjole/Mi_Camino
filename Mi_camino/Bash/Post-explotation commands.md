
## Elevate privileges 

- uname -a: Lista una serie de datos importantes de linux 
- sudo -l: lista los programas/archivos que se puede ejecutar con privilegios de super usuario+
- /proc/self/environ es la lista de variables de entorno para el usuario que lo ejecuta
- /etc/shadow Contraseñas de usuarios hasheadas (solo se puede acceder con permisos administrativos)
## Tratamiento de la TTY 

- script /dev/null -c bash
- *Presionar Ctrl-Z*
- stty raw -echo; fg
- reset
- export TERM=xterm
- export SHELL=bash
- stty size *ejecutar en la terminal normal para mirar el tamaño de la tty *
- stty rows 40 columns 123 *En filas y columnas setear el resultado del comando stty size en la consola normal*

