# Desconectar sesiones SSH inactivas

Via: (http://haydenjames.io/kill-inactive-ssh-sessions/)


Cuando trabajamos con un sistema remoto mediante SSH, en ocasiones la conexión se cae dejando sesiones SSH "muertas" en el sistema remoto.

Podemos configurar SSH para que estas sesiones se acaben desconectando con el tiempo, pero podemos limpiarlas de forma manual.

Para identificar estos casos, mediante el comando `w` podemos ver cuantos usuarios hay conectados al sistema, por ejemplo aquí vemos que hay dos conexiones del usuario hfiel:

```
13:28:52 up  4:21,  3 users,  load average: 0,20, 0,33, 0,30
USUARIO  TTY      DE               LOGIN@   IDLE   JCPU   PCPU WHAT
hfiel    pts/0    192.168.1.20     12:34    12:34  12:34  1.23s -bash
hfiel    pts/1    192.168.1.20     23:45    23:45  23:45  2.34s w
```

Para localizar y eliminar el proceso SSH bloqueado, lanzamos el comando:

```
 pstree -p
```

En la salida del mismo, veremos el árbol de procesos junto con los PID de cada proceso, y localizaremos el proceso "padre" `sshd`. De dicho proceso cuelgan las distintas sesiones SSH

```
 ├─sshd(1234)─┬─sshd(2345)───bash(3456)
 │            └─sshd(4567)───bash(5678)───pstree(6789)
```

La sesión actualmente activa es la que contiene el proceso `pstree` (en este ejemplo, la sesión SSH con el PID 4567).
La sesión inactiva no tendrá dicho proceso (en este ejemplo, la sesión SSH con el PID 2345).

Para desconectar la sesión incativa, procedemos a matar al proceso asociado a la misma mediante `kill`, en este ejemplo:

```
kill 2345
```

y verificamos con `w` que ya sólo tenemos nuestra sesión

```
13:28:52 up  4:21,  3 users,  load average: 0,20, 0,33, 0,30
USUARIO  TTY      DE               LOGIN@   IDLE   JCPU   PCPU WHAT
hfiel    pts/1    192.168.1.20     23:45    23:45  23:45  2.34s w
```

## Para activar la desconexión automática de las sesiones inactivas

Editamos el fichero  `/etc/ssh/sshd_config` para añadir las siguientes opciones:

```
ClientAliveInterval 600
ClientAliveInterval 3
```
