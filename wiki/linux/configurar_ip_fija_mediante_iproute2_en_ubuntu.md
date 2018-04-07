# Configurar IP fija mediante comandos iproute2 en Ubuntu

Para añadir la IP:

```
 $ sudo ip addr add <IP>/<MASCARA> dev <INTERFAZ>
```

PE, para darle la 192.168.1.140 a la eth0:

```
 $ sudo ip addr add 192.168.1.140/24 dev eth0
```

Para establecer el gateway por defecto:

```
 $ sudo ip route add default via <GATEWAY>
```

PE, para establecer el gateway como 192.168.1.1

```
 $ sudo ip route add default via 192.168.1.1
```

Para levantar/parar la interfaz

```
 $ sudo ip link set <INTERFAZ> <up/down>
```

Por ejemplo, para levantar eth0

```
 $ sudo ip link set eth0 up
```
