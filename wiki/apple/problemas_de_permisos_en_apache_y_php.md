!!¿Te has dado cabezazos con el Apache/PHP y sus permisos?

** ¡CUIDADO! jugar con las ACL puede hacer pupa a tus ficheros. **

Por defecto, podemos trabajar con los documentos del servidor en \'\'/Library/WebServer/Documents\'\', pero si creamos una nueva carpeta dentro tendremos problemas para acceder, y más aún para usar las funciones de escritura de ficheros de PHP.

Solucion: jugar con las ACL (Access Control Lists) y tocar una línea de php.ini

Lo de las ACL viene a ser como los permisos UNIX de toda la vida, pero a lo bestia, permitiendo una flexibilidad mucho mayor. Se pueden establecer a mano, pero puede ser un verdadero suplicio. Lo mejor es usar una aplicacion llamada \'\'Sandbox\'\' [http://www.mikey-san.net/damage/], y establecer los siguientes ACL (seguro que se puede afinar bastante más con los permisos, pero con esto seguro que funciona sin tener que hacer un chmod 777)

Seleccionamos la carpeta donde queremos cambiar los permisos.

Añadimos a nuestro propio usuario (para evitar problemas de acceso en la carpeta seleccionada, con todos los permisos posibles.

Añadimos al usuario/grupo \'\'_www\'\', que es el que ejecuta Apache (y por ende, PHP), de nuevo con todos los permisos.

Una vez hecho esto, propagamos estos dos ACL por el resto de directorios que haya bajo el principal. Esto se hace desde Entries->Propagate Selected Entry.

Si en algun momento la liamos, desde el menu entry podemos eliminar todas las ACL que haya en esa carpeta y las subcarpetas. 


Por ultimo, para poder subir archivos desde las páginas, en \'\'/etc/php.ini\'\' tenemos que configurar la carpeta temporal, que por defecto viene sin ruta, 
para que apunte a \'\'/tmp\'\'.
