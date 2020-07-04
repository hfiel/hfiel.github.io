!Pasos
En synaptic (u otro gestor), marcamos e instalamos el paquete ::swat::, y el solo debería marcar como dependencia ::openbsd-inetd::

Si queremos hacerlo desde consola, pues mediante ::apt-get install swat::


Al final, lanzamos ::/etc/init.d/openbsd-inetd restart:: y listos.

Nos faltaría añadir en el archivo ::rc.local:: la línea para que arranque openbsd-inetd cada vez que iniciemos el sistema.

Para que luego digan que linux es complicado.
