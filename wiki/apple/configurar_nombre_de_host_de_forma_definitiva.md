En OS X cuando nos conectamos a una red con DHCP (o con servidores OS X) nuestro nombre de host puede cambiar de manera dinámica. Para evitarlo, podemos configurar
el archivo \'\'/etc/hostconfig\'\' y añadir la línea \'\'HOSTNAME=nombre\'\' al mismo.

Aqui también se puede configurar arranque del automount o de ciertos servicios de servidor.

NOTA: Si en alguno caso sigue saliendo un nombre automático o antiguo, podemos mirar en \'\'/Library/Preferences/SystemConfiguration/preferences.plist\'\' ya que ahi se guarda otro nombre que a veces puede no haberse actualizado.
