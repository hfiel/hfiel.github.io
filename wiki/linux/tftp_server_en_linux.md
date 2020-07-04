 Obtenido originalmente del blog http://chikogollo.blogspot.com/
 Original de: Javier Mecklenburg
 **El blog ya no existe**
 http://chikogollo.blogspot.com/2007/03/tftp-server-on-linux.html

Ya, muchos de los que trabajamos con equipamiento de red, necesitamos montar un TFTP server ya sea para guardar configuraciones, IOS de equipos etc.

Lo primero que debemos saber que es xinetd, Xinetd es un super servicio que controla a muchos otros tales como ftp, telnet, IMAP, etc.

Primero debemos instalar los paquetes necesarios para levantar este servicio. lo que necesitamos en tftpd y ftp. Entonces...

#apt-get install tftpd tftp

Una vez que esta listo y instalado debemos ir a /etc/xinetd.d/

cd /etc/xinetd.d/

si el archivo tftp no existe lo creamos y si existe lo modificamos.

si no existe debemos dejar algo como esto.

# Service es el servicio que debe estar identificado en /etc/services
service tftp
{
#protocolo TCP/UDP con el que vamos a trabajar este debe estar especificado en #/etc/protocols
protocol = udp
#Puerto en el cual vamos a escuchar las peticiones.
port = 69
#tipo de socket si es UDP corresponde a dgram si es TCP debe ser stream
socket_type = dgram
wait = yes
#el usuario a acceder al servicio
user = nobody
#el ejecutable que inicia el servicio
server = /usr/sbin/in.tftpd
#lugar donde van a alojarse los archivos entrantes o salientes.
server_args = /tftpboot
#si el servicio esta habilitado o no
disable = no
}


(Los \"#\" indican comentarios y no serán tomados en cuenta dentro de la configuracion.)

Si el fichero existe lo debemos modificar segun corresponda.

Ahora debemos crear el fichero donde vamos a alojar los archivos.

#mkdir /tftpboot

Luego debemos darle permisos y a quien corresponde el fichero


chmod -R 777 /tftpboot
chown -R nobody:nobody /tftpboot


Ahora debemos reiniciar el servicio xinetd

#/etc/init.d/xinetd restart

Una vez que este proceso termina, vemos si estamos escuchando peticiones udp al puerto 69

#netstat -anu
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address Foreign Address State
udp 0 0 0.0.0.0:68 0.0.0.0:*
udp 0 0 0.0.0.0:68 0.0.0.0:*
udp 0 0 0.0.0.0:69 0.0.0.0:*

Como vemos si estamos escuchando peticiones asi que estamos listos para probar el servicio.

como lo probamos, desde una maquina remota nos conectamos al servidor y colocamos o sacamos archivos.

Bueno eso seria.. ojala les sirva

saludos!
