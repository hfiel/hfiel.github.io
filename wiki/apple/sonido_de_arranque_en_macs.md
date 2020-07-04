!Silencia tu MAC al arrancar

** La última vez que lo probé era en Leopard (10.5), desconozco si sigue funcionando **

Para hacer que el sonido de arranque sea menos molesto, podemos crear un comando que se lanza cada vez que apagamos el ordenador, y establece el volumen que deseamos que tenga el equipo cuando se vuelva a encender.
Modificamos el archivo \'\'/etc/rc.shutdown.local\'\', y añadimos la siguiente línea:
 osascript -e \"set volume 1\"
cambiando el 1 por el valor que queramos, entre 0 y 7

Esto no soluciona el sonido a volumen alto que se produce tras los reinicios obligados (por ejemplo, tras actualizar el sistema), pero podemos modificar el archivo \'\'/etc/rc.local\'\' para ajustar aún más el sistema.
