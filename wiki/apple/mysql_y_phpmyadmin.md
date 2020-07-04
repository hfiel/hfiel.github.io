OJO: lo que TÚ estás buscando probablemente sea MAMP: [http://www.mamp.info/en/index.html]

Esto SÓLO es por si tienes ganas de complicarte la vida (y mucho) :D

**Instalacion de MySQL y phpMyAdmin en OS-X (a mano)**

- Bajamos el paquete de MySQL e instalamos
(podemos compilarlo si tenemos ganas y tiempo, tarda unos 20 minutos en hacer el build)
- Necesitamos añadir algunas extensiones a php, como mínimo mcrypt.
Para ello:

1) Bajamos libmcrypt-2.5.8, [http://sourceforge.net/project/showfiles.php?group_id=87941]

2) PHP 5.2.6 [http://us.php.net/get/php-5.2.6.tar.gz/from/a/mirror]

3) Instalamos Xcode 3 tools (de Apple Developer Connection, es casi 1 GB)

4) En terminal, creamos un directorio \'\'SourceCache\'\' en la raiz y extraemos los dos primeros archivos dentro.

5.1) Vamos al directorio libmcrypt-2.5.8 e introducimos esto

MACOSX_DEPLOYMENT_TARGET=10.5 CFLAGS=\'-O3 -fno-common -arch i386 -arch x86_64 -arch ppc7400 -arch ppc64\' LDFLAGS=\'-O3 -arch i386 -arch x86_64 -arch ppc7400 -arch ppc64\' CXXFLAGS=\'-O3 -fno-common -arch i386 -arch x86_64 -arch ppc7400 -arch ppc64\' ./configure --disable-dependency-tracking

5.2) \'\'make -j6\'\'

5.3) \'\'sudo make install\'\'

6.1) Vamos al directorio /SourceCache/php-5.2.6/ext/mcrypt

6.2) Lanzamos \'\'/usr/bin/phpize\'\' (phpize debe estar en /usr/bin, si no hay que buscarlo)

6.3) lanzamos:

MACOSX_DEPLOYMENT_TARGET=10.5 CFLAGS=\'-O3 -fno-common -arch i386 -arch x86_64 -arch ppc7400 -arch ppc64\' LDFLAGS=\'-O3 -arch i386 -arch x86_64 -arch ppc7400 -arch ppc64\' CXXFLAGS=\'-O3 -fno-common -arch i386 -arch x86_64 -arch ppc7400 -arch ppc64\' ./configure --with-php-config=/Developer/SDKs/MacOSX10.5.sdk/usr/bin/php-config

6.4) \'\'make -j6\'\'

6.5) \'\'sudo make install\'\'

7.1) Editamos \'\'/etc/php.ini\'\' (si no existe, lo creamos copiando \'\'php.ini.default\'\'.

7.2) Buscamos y comprobamos que tenemos \'\'enable_dl = On\'\'.
Buscamos \'\'extension_dir = \"./\"\'\' y nos aseguramos de que tiene un ; delante.
Añadimos una línea en la seccion \'\'Dynamic Extensions\'\' con el texto \'\'extension=mcrypt.so\'\'

8) Agregamos una clave al usuario root de MySQL
\'\'/usr/local/mysql/bin/mysqladmin -u root password <password>\'\'

9) Damos de alta un nuevo usuario con todos los privilegios (para evitar trabajar como root):
\'\'/usr/local/mysql/bin/mysql -u root -p\'\' e introducimos la clave.
Una vez dentro del cliente mysql, lanzamos el siguiente comando:
\'\'GRANT ALL PRIVILEGES ON *.* TO \'usuario\'@\'localhost\' IDENTIFIED BY \'clave\';\'\'

10) Editamos \'\'php.ini\'\' y buscamos la línea \'\'mysql.default_socket\'\'. Añadimos
la ruta \'\'/tmp/mysql.sock\'\' tras el signo =

11.1) Descomprimimos phpMyAdmin en una carpeta del servidor apache (PE, en /Library/WebServer/Documents/phpmyadmin)

11.2) Creamos una carpeta \'\'config\'\' dentro de ese directorio, y le damos permisos de lectura y escritura a otros: \'\'chmod o+rw config\'\'.

11.3) Abrimos la en el explorador \'\'localhost/phpmyadmin/setup\'\' y se debe crear automaticamente el script de configuracion. Puede que de algunos warnings o errores. Esto no lo he solucionado todavia. 

12) Ya deberíamos poder entrar en \'\'http://localhost/phpmyadmin\'\' usando el usuario y contraseña que hemos definido antes en MySQL (o tambien el root de MySQL)
