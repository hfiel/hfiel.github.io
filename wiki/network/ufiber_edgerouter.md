# UFiber y EdgeRouter para conectar a la red de fibra de MasMovil / Pepephone

Instrucciones para reemplazar el modem/router de fibra que te instalan en MasMovil / Pepehone con una solución Ubiquiti

## Requisitos

1. GPON Ubiquiti, tanto el *UFiber Loco* como el *UFiber Nano G* son válidos.
2. Router Ubiquiti EdgeRouter (en este ejemplo se muestra con un *EdgeRouter Lite*, es equivalente en otros modelos como el *Edgerouter X SPF*).
3. La clave de conexión a tu servicio de fibra.
   - Esto te lo puede dar el técnico al hacer la instalación si se lo pides amablemente (es uno de los parámetros que introduce en la configuración del router)
   - Si ya tienes la instalación terminada, lo puedes intentar sacar de la configuración de tu router a través del bug de Samba. Puedes encontrar instrucciones detalladas en el [Blog tecnológico de Álferez](https://www.alferez.es/sacando-los-datos-del-router-de-fibra-masmovil/)
4. Descarga de la [web de Ubiquiti](https://www.ui.com/download/) el firmware más actualizado para tu UFiber Loco / Nano G y guárdalo en tu PC.
5. Descarga de la [web de Ubiquiti](https://www.ui.com/download/) el firmware más actualizado para tu EdgeRouter y guárdalo en tu PC.

## Configuración del GPON

1. Configura tu tarjeta de red en el PC con una dirección IP estática de la red 192.168.1.X (por ejemplo, 192.168.1.3)
2. Conecta el cable de red y la alimentación al GPON, conecta el otro extremo del cable de red al PC (directamente) y accede 3 la interfaz de gestión (http://192.168.1.1) con el usuario predeterminado (ubnt/ubnt).
4. Actualiza el firmware del gpon a la última versión disponible y vuelve a conectar a la interfaz de gestión.
5. *OPCIONAL*: Para evitar colisiones al configurar otros equipos, modifica la dirección IP del GPON con una IP y subred que no vayas a utilizar en el futuro (por ejemplo, ponlo en una 192.168.0.2) y por seguridad cambia la clave predeterminada (el nombre del usuario, *ubnt*, no puedes modificarlo). Cambia la IP de tu PC para que corresponda a la subred que has establecido en el GPON (Por ejemplo, 192.168.0.3) y vuelve a conectar a la interfaz de gestión.
6. En la configuración de GPON selecciona:
   - OLT PROFILE: **Profile 2**
   - LOGICAL ID: **123456789** (Opcional, puedes dejarlo en blanco)
   - LOGICAL PASSWORD: **123456**
   - PLOAM PASSWORD: **<CLAVE DE TU FIBRA>**
   - HEX FORMAT: **OFF**
   - CHANGE SN PREFIX TO HWTC: **OFF**
7. Aplica los cambios y guarda la configuración.
8. Desconecta el UFiber Loco del PC.
  
## Configuración del EdgeRouter

*NOTA*: para evitar colisiones de red con otros equipos en el futuro, yo configuro el router con la IP 192.168.2.1, dejando de esta forma libre la 192.168.1.1 para instalar dispositivos nuevos, y además configuro otra segunda subred para tener dos segmentos aislados con salida independiente a Internet. Ajusta los valores de las IP según tu caso, y ajusta las configuraciones de cada interfaz si tienes otro modelo de EdgeRouter.

1. Conecta la alimentación y el cable de red al puerto **eth0** de tu edgerouter, conecta el otro extremo del cable de red al PC (directamente) y accede a la interfaz de gestión (http://192.168.1.1) con el usuario predeterminado (ubnt/ubnt). Cuando te aparezca el asistente de configuración inicial, cancela el asistente.
2. Actualiza el firmware del edgerouter a la última versión disponible y vuelve a conectar a la interfaz de gestión.
3. Cuando te aparezca el asistente de configuración inicial, cancela el asistente.
4. En el menu de wizards, selecciona WAN+2LAN2 y configura los siguientes parámetros: 
   - Internet port (eth0):
     - Internet connection type: **DHCP**
     - VLAN: marca la casilla **Internet connection is on VLAN** y pon como *VLAN ID* el valor **20**
     - Firewall: marca la casilla **Enable the default firewall**
   - LAN ports (eth1):
     - Address: **192.168.2.1** / **255.255.255.0**
     - DHCP: marca la casilla **Enable the DHCP server**
   - Secondary LAN ports (eth2):
     - Address: **192.168.3.1** / **255.255.255.0**
     - DHCP: marca la casilla **Enable the DHCP server**
   - User Setup: *OPCIONAL*: por seguridad deberías crear un nuevo usuario administrador con una clave segura. Te recomiendo que al terminar, una vez verifiques que puedes conectar con este nuevo usuario, elimines el usuario predeterminado *ubnt*.
5. Aplica los cambios y confirma que quieres reiniciar el router. Ten en cuenta que al aplicar y reiniciar perderás la conectividad al router, dado que hemos cambiado la configuración IP.
6. Vuelve a dejar la conexión de red de tu PC en **DHCP**.

## Conexión de UFiber, EdgeRouter y LAN

Las conexiones finales:

1. Conecta un cable de red del UFiber al puerto eth0 del EdgeRouter.
2. Conecta tu PC (o tu switch, si lo tienes) al puerto eth1 del EdgeRouter (si tienes y quieres conectar la otra subred, por ejemplo a otro equipo o switch, conecta otro cable al puerto eth2).
3. Desconecta con cuidado el cable de fibra de tu router antiguo. *PRECAUCION*: no tires del cable de fibra ni de la camisa protectora del final, tira con cuidado únicamente del conector en sí que hay al extremo del cable.
4. Conecta el cable de fibra al puerto PON del UFiber.

## Verificaciones

1. UFiber:
   - En el caso del UFiber Loco, el indicador de conectividad debe mostrar de 1 a 3 líneas en color blanco fijo, según sea el nivel de señal que se reciba. Si se quedan las 3 líneas fijas en naranja revisa los cables, y si parpadean naranja/blanco, conecta por cable directamente al UFiber y revisa la configuración.
   - En el caso del UFiber Nano G, deberás ver el símbolo del mundo en verde, y tocando el display deberás poder ver información sobre la conexión. Si el símbolo del mundo está en rojo fijo es un error (la pantalla indicará más información) y si parpadea en rojo aun está sincronizando.
2. Desde el PC, debes poder hacer ping al router (si estás conectado a la interfaz eth1, debes hacer ping a la IP 192.168.2.1), y debes poder acceder a la interfaz de gestión en https://192.168.2.1.
3. En la interfaz de gestión del EdgeRouter, en el *dashboard* debe aparecer una interfaz llamada **eth0.20** y esta interfaz debe aparecer conectada y tener asociada una IP (esta IP será la entregada por el proveedor, y cambiará en cada caso)
4. Desde el PC, debes poder hacer ping a conexiones en Internet (PE, a los DNS de Google en 8.8.8.8)
5. Desde el PC, debes poder navegar.

Si alguna de las verificaciones falla, revisa los cables y la configuración de cada equipo.
