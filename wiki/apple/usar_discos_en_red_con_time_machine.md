Si queremos que nuestros Backups de Time Machine se lleven a cabo sobre discos en red (para facilitar la centralización de los backups), podemos emplear la versión Server de Leopard, o emplear este comando en la terminal:
 defaults write com.apple.systempreferences TMShowUnsupportedNetworkVolumes 1

Con esto, los volúmenes en red aparecen en la lista de disponibles para Time Machine, y los podemos emplear como si fueran locales.
