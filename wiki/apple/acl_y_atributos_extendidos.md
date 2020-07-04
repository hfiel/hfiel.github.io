!Ver ACL y atributos extendidos (en terminal)

Cuando hacemos un \'\'ls -l\'\' en un directorio, a veces vemos que en la parte de permisos aparecen \'\'+\'\' y \'@\'. Estos indican respectivamente la presencia de ACL y de Atributos de archivo extendidos.

Para ver los ACL:
::ls -le::

Para ver los atributos extendidos:
::ls -l@::

Para leer los atributos extendidos podemos usar el comando \'\'xattr\'\':
::xattr -p com.attribute.name archivo::

Para más información sobre el comando, \'\'xattr -h\'\'
