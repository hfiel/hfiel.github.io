# Configurar IP fija de forma manual en Ubuntu

Vamos a configurar la red con una IP fija a mano.
Los valores que voy a usar (y que cada cual deberá ajustar segun su red) son lo más típicos.

- Dirección IP del PC: 192.168.1.2
- Máscara de red: 255.255.255.0
- Puerta de enlace: 192.168.1.1
- Servidores DNS: los que te haya dado tu proveedor, o cualquiera que desees. Personalmente yo configuro varios, por si acaso alguno se cae. Por ejemplo, el 80.58.0.33 (telefonica de españa) 8.8.4.4 (google) y 8.8.8.8 (google).


Los pasos son:

1. Indentificar nuestra interfaz (lo podemos hacer con ifconfig):
Normalmente, la primera tarjeta por cable se llama eth0, pero esto depende de tu sistema y del numero de tarjetas que tengas instaladas. Asegurate de buscar la tarjeta correcta.

2. Configurar la red:
Necesitamos editar el fichero /etc/network/interfaces (necesitarás hacerlo mediante sudo)
 sudo nano /etc/network/interfaces

En el fichero, aparecerá la confguración existente. Si nunca has configurado la red, sólo debería aparecer mencionada la conexion de bucle local (lo). Tendremos que añadir lo siguiente (Cambiando los valores por los tuyos. Recuerda poner el nombre adecuado a tu tarjeta, en este caso uso eth0):

```
 auto eth0
 iface eth0 inet static
 address 192.168.1.2
 network 192.168.1.0
 netmask 255.255.255.0
 broadcast 192.168.1.255
 gateway 192.168.1.1
```

Guardamos el fichero.

NOTA: se pueden añadir los dns directamente aqui usando la linea:

```
 dns-nameservers 8.8.8.8 8.8.4.4
```

OJO: Si vuestra red no tiene las direcciones más típicas (`192.168.1.X`, con máscara `255.255.255.0`), los valores de network y broadcast deberéis calcularlos para ponerlos bien.

-Configuramos el fichero con los DNS:
De nuevo mediante sudo y nano, editamos el fichero /etc/resolv.conf

```
 sudo nano /etc/resolv.conf
```

Ahí aparecen los servidores DNS que tengamos conigurados, los que queramos añadir sólo debemos ponerlos en cada línea, poniendo delante la palabra nameserver:

```
 nameserver 80.58.0.33
 nameserver 8.8.4.4
 nameserver 8.8.8.8
```

Salimos y guardamos.

Lo siguiente es reiniciar la red:

```
 sudo /etc/init.d/networking restart
```


NOTA: en algunos sitios (incluida mi raspberry pi) la opcion restart genera un warning por deprecated. Por lo visto se va a eliminar dicha opcion, asi que otra forma es:

```
 sudo /etc/init.d/networking stop
 sudo /etc/init.d/networking start
```

que viene a ser lo mismo...



Si todo es correcto, vuestra conexión debería ir perfectamente. Podéis comprobarlo mediante un ping a alguna web, por ejemplo

```
 ping www.google.com
```


Que debería responder algo así como:

```
 PING www.l.google.com (72.14.235.104) 56(84) bytes of data.
 64 bytes from www.google.com (72.14.235.104): icmp_seq=1 ttl=55 time=41.368 ms
 64 bytes from www.google.com (72.14.235.104): icmp_seq=2 ttl=55 time=41.321 ms
 64 bytes from www.google.com (72.14.235.104): icmp_seq=3 ttl=55  time=41.931 ms
```
