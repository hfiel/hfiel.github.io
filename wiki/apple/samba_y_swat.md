! Para activar Samba (completo) en OS X

OJO #1: la última vez que lo usé fue en MAC OS X 10.5.4, desconozco su comportamiento en versiones actuales.

OJO #2: HACER ESTO PUEDE SUPONER UN IMPORTANTE AGUJERO EN LA SEGURIDAD DE TU SISTEMA. NO SE TE OCURRA HACERLO EN UNA MÁQUINA EN PRODUCCIÓN O CON SALIDA DIRECTA A INTERNET. 

* Vamos a ::Sistema/Libreria/LaunchDaemons/swat.plist::, (recuerda hacer primero un backup por si acaso), y abrimos la ventana de información del original, y cambiamos el propietario a nuestro usuario. Abrimos el archivo original y modificamos la sección ::Root/ProgramArguments/1::, donde pone ::-a:: ponemos ::-d::.

* Abrimos el ::Gestor Netinfo::, abrimos el candado, y en el menú ::seguridad:: activamos la cuenta de root (le asignamos una clave, que recomiendo que sea la del usuario principal, para evitar problemas con sudo). OJO: esto puede ser un ENORME boquete de seguridad. Si estás en un entorno de producción, te recomiendo que pruebes a configurar una cuenta de administración sólo para Samba y Swat, sin privilegios en el resto del sistema.

* Abrimos un terminal, y hacemos copia (con sudo) de ::/etc/smb.conf::, ::/etc/services:: y ::/etc/hostconfig::

* Nos vamos a ::/etc/pam.d:: y copiamos ::sudo cp ./passwd ./samba::

* Editamos ::/etc/services::, buscamos las lineas referentes al puerto 901, comentamos las dos existentes (tcp y udp) y añadimos la linea: ::swat 901/tcp # samba swat::

* Editamos ::/etc/hostconfig:: y en la línea ::SMBSERVER=-NO-:: ponemos ::SMBSERVER=-YES-::

* Iniciamos SWAT: ::/sbin/service/swat start

* Ya podemos acceder a SWAT en [http://localhost:901] (EN TEORÍA no se puede acceder desde otro equipo, sólo desde el local).

* En caso de pedirnos contraseña, indicar la que se asigno para la cuenta root en el paso 2.

* Por defecto, la seguridad está en ::User::, si simplemente se desea compartir una carpeta se puede poner la seguridad en ::Shared::, y crear un nuevo recurso (share) mediante SWAT. Si queremos compartir con autenticación, lo mejor será crear un usuario para compartir con samba (no es recomendable usar root).

* Por último, si no conseguimos ver el equipo desde un PC, hay que evitar que el firewall nos filtre. Nos vamos a preferencias del sistema, Compartir, y activamos el servicio ::Compartir Windows::. Aunque diga que hay que activar una cuenta, si has configurado bien Samba desde SWAT, no debería hacer falta. Ya podemos acceder al equipo desde Windows con \\\\IP ó \\\\Nombre_netbios_equipo

* Si con el nombre no funciona, conecta primero con la IP, y ya podrás conectar con el nombre.

* Si un usuario ya está conectado cuando cerramos el firewall, puede que se quede dentro del recurso, y siga teniendo acceso. Deberemos echarlo a mano desde SWAT, y así evitaremos que pueda seguir conectado.

* Todo lo que se encuentre en la carpeta definida como share aparecerá en el entorno de red de Windows con la configuración indicada.
