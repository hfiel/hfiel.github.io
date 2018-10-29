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

_Cuidado:_ esto permitirá a cualquier persona (cualquier IP) usar para el reenvío de paquetes los puertos del Equipo B que se definan en la conexión (la conexión la creamos en el siguiente punto). En la práctica, todas las peticiones a C estarían llegando desde B, por lo que esto se podría usar para enmascarar un ataque que se estuviera produciendo desde A o desde otro equipo D que sea capaz de ver a B.

Ten presente esto si configuras la opcin `GatewayPorts` en un servidor expuesto a Internet. Consulta en la documentacin de ssh la opción `clientspecified` si necesitas permitir el reenvío sólo para ciertas IP.


## En el host A:

Lanzamos una conexión SSH al host B (192.168.0.11) mapeando el puerto 9000 (local del equipo A, 192.168.0.10) al puerto 80 (remoto, del equipo C, 192.168.1.10)

```
ssh -R 9000:192.168.1.10:80 usuario@192.168.0.11
```

## En el host A:

En el navegador web abrimos:

```
http://192.168.0.11:9000
```

la petición se enviará al Equipo B, que la reenviará al puerto 80 del equipo C
