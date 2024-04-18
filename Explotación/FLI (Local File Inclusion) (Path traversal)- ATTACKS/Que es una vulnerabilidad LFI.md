LFI, las sigas de Local File Inclusion, es una tecnica en la cual un atacante aprovecha una funcion del servidor la cual llama algun archivo en especifico para acceder a archivos privilegiados del sistema, o incluso ejecutar codigo de forma remota, lo que en otras palabras es explotar un RCE (Remote Code Execution), pero eso ya es mas avanzado (que igualmente veremos en este modulo), el orden del modulo sera el siguiente: 

- 1 ¿Que es una vulnerabilidad LFI?
- 2 Explotando una vulnerabilidad LFI basica
- 3 ¿Que son los PHP wrappers?
- 4 Evitar proteciones contra LFI 
- 5 ¿Que es una vulnerabilidad RFI?
- 6 Explotando una vulnerabilidad RFI y RCE
- 7 Envenenamiento de registros

Siguiendo con la explicacion de que es un LFI el cual se puede unir o dividir en otras tecnicas, por ejemplo, a pesar de que el LFI y el RFI son tecnicas distintas, ambos se basan en una misma vulnerabilidad que permite cargar y ver archivos, asi que para facilitar este modulo diremos que el RFI deriva del LFI, hice un esquema que hara mas facil de entender las tecnicas que se veran en este modulo: 

<img src="/Z-Imagenes/diagramafli1.png" height="2000" weigth="1000" />

