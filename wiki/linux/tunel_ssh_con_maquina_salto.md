# Tunel SSH remoto con máquina de salto


## Situación de ejemplo:

Equipo A (192.168.0.10) -> Equipo B (192.168.0.11) -> Equipo C (192.168.1.10)

* Podemos conectar mediante SSH desde A hasta B.
* Podemos conectar mediante Web (puerto 80) desde B hasta C.
* No podemos conectar mediante Web (puerto 80) desde A hasta C (por ejemplo, porque no hay ruta).


## En el host B:

Configuramos SSH para que nos permita hacer reenvío de puertos remotos desde fuera del equipo.

En el fichero /etc/ssh/sshd_config  modificamos la línea:

```
GatewayPorts no
```

por

```
GatewayPorts yes
```



## En el host A:

Lanzamos una conexión SSH al host B (192.168.0.11) mapeando el puerto 9000 (local del equipo A, 192.168.0.10) al puerto 80 (remoto, del equipo C, 192.168.1.10)

```
ssh -R 9000:192.168.1.10:80 usuario@192.168.0.11
```

## En el host A:

En el navegador web abrimos:

```
http://localhost:9000
```

la petición se enviará al Equipo B, que la reenviará al puerto 80 del equipo C
