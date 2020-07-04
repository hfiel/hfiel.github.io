Para liberar la concesion existente:

 $ sudo dhclient -r

Para obtener una nueva:

 $ sudo dhclient


Para indicar la interfaz de red específica (por ejemplo, eth0):

 $ sudo dhclient -r eth0
 $ sud dhclient eth0


Para dejar limpias las concesiones de dhcp (en eth0):

 $ sudo dhclient -r -v eth0 && sudo  rm /var/lib/dhcp/dhclient.* && sudo dhclient -v eth0

NOTA: se queda esperando una nueva concesión
