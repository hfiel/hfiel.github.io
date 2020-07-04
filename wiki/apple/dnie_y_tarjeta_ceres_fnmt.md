Esto esta sacado (hace siglos) de los foros de Macuarium, es una guía creada por el usuario Goncal Roca, pero personalmente aun no la he probado



-Instalar \"sca-0.2.3pre2\"
[http://www.opensc-project.org/files/sca/experimental/sca-0.2.3pre2.dmg]

-Instalar \"opensc.dnie-1.4.0-5\" 
[http://www.dnielectronico.es/descargas/PKCS11_para_Sistemas_Unix/MAC_OS_X.tar]

-Modificar manualment el fichero:
 /Library/OpenSC/etc/opensc.conf

Cambiar:
 card_drivers = dnie;
 card_driver dnie {
 #The location of the driver library
 module = ;

Por:
 card_drivers = dnie;
 card_driver dnie {
 #The location of the driver library
 module = /Library/OpenSC/lib/libopensc-dnie.1.0.3.dylib;


-Instalar a Firefox el certificado de la policia \"ACRaiz.crt\" [http://www.dnie.es/certs/ACRaiz.crt]

-Abrir Firefox
-Abrir el menú “Firefox->Preferencias”
-Seleccionar el último icono que tiene el nombre “Avanzado”
-Seleccionar la pestaña “Cifrado”
-Pulsar el botón “Visualizar los certificados”.
-Se abre una nueva ventana. Escoger la pestaña “Sitios web”.
-Pulsar el botón “Importar” y escoger el fichero “ACRaiz.crt” que nos hemos bajado de la web y nos aparecerá instalado un certificado de la “DIRECCION GENERAL DE POLICIA”.



-Instalar el mòdul PKCS#11 abriendo con Firefox \"/Library/OpenSC/share/web/instala_modulo.htm\"

-Comprobar los certificados
-Cerrar Firefox

-Conectar el lector de tarjetas
-Introducir el DNIe en el lector

-Abrir Firefox
-Abrir el menú “Firefox->Preferencias”
-Seleccionar el último icono que tiene el nombre “Avanzado”
-Seleccionar la pestaña “Cifrado”
-Pulsar el botón “Dispositivos de seguridad” y podremos ver si no reconoce el módulo “DNIe”. Cerrar la ventana y volvemos a la ventana “Avanzado->Cifrado”
-Pulsar el botón “Visualizar los certificados” y nos pedirá que introduzcamos “la contraseña maestra para el DNI electrónico (PIN1)”. La introducimos y si todo es correcto nos mostrará una ventana con la pestaña “Tus certificados” donde podremos ver un certificado de la “DIRECCION GENERAL DE POLICIA” con nuestro nombre.

-Si aparece el certificado pero no nuestro nombre puede ser que el PIN introducido no sea correcto o tengáis algún problema con las caches (a mi me paso eso). Para vaciar la caches ir al menú “Herramientas->Limpia los datos personales” y volver aprobar.
