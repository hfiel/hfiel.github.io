Para que resolvconf establezca los DNS que queramos al crear /etc/resolv.conf

En el fichero: 
 /etc/resolvconf/resolv.conf.d/base

ponemos los dns que queremos que se incluyan (por ejemplo, los de google) 
 
 nameserver 8.8.8.8
 nameserver 8.8.4.4

NOTA: los DNS configurados aquí aparecen en resolv.conf incluso cuando no existan interfaces configuradas en el sistema


Una vez configurados los dns, ejecutamos la actualizacion de resolvconf

 $ sudo resolvconf -u 

los nuevos dns deben aparecer en /etc/resolv.conf
