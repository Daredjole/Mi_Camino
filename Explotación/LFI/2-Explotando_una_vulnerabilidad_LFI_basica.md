A partir de aqui empieza lo divertido, ya con los conceptos basicos de como una pagina web incluye archivos para su visualizacion o ejecucion, podremos expotar una vulnerabilidad basica LFI, los mas atentos ya se imaginaran que si no hay ningun tipo de sanitizacion, podremos acceder a cualquier archivo del cual tengamos su nombre, usualmente, para comprobar si existte una vulnerabilidad LFI basica se suele inyectar la ruta directa a /etc/hosts, aqui un ejemplo:

<img src="/Z-Imagenes/LFI2.png" />

pero como se imaginaran, como mucho podremos ver que usuarios hay en el back-end por lo tanto, eso que mostre anteriormente no es un ataque, se puede tomar mas como una prueba para confirmar la existencia de una posible vulnerabilidad LFI, por lo tanto usare la ruta /etc/