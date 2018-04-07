# Conexiones SSH sin password

Vía:  [http://www.thegeekstuff.com/2008/11/3-steps-to-perform-ssh-login-without-password-using-ssh-keygen-ssh-copy-id/](http://www.thegeekstuff.com/2008/11/3-steps-to-perform-ssh-login-without-password-using-ssh-keygen-ssh-copy-id/)

* En el host local, ejecutamos

```
 ssh-keygen -t rsa
```

NOTA: no debemos indicar passphrase, o si no todo esto no valdrá para nada, dado que nos pedirá una constraseña para acceder al contenido de la pareja de claves.

Esto crea la pareja de claves rsa (`id_rsa` e `id_rsa.pub`)
Es recomendable dejar que lo haga en la ruta por defecto (`~/.ssh/`)

* Trasladamos la clave PUBLICA al host remoto

```
ssh-copy-id -i ~/.ssh/id_rsa.pub usuario_remoto@host_remoto
```

En el proceso, si es la primera vez que conectamos con esa maquina, nos pedirá verificar la clave RSA del host remoto (para garantizar que no nos estamos dando de alta en un servidor que no queremos), y validarnos con la clave del usuario_remoto.

Si por alguna razon no podemos usar `ssh-copy-id`, podemos hacer el traslado a pelo.
Para ello, en el host remoto, creamos (si no existe) el fichero `~/.ssh/authorized_keys`

Copiamos la `id_rsa.pub` local al remoto (ojo, que en el remoto, en `~/.ssh/` puede haber ya alguna `id_rsa.pub`, no vayas a sobreescribirla), usando `scp`.

En el remoto, añadimos la nueva clave al `authorized_keys` mediante:

```
cat id_rsa.pub >> authorized_keys
```


* Con las claves añadidas a la lista de autorizadas, procedemos a probar que podemos entrar:

```
ssh usuario_remoto@host_remoto
```

y no deberia pedirnos clave.

* Ahora, solo queda repetir el copiado a cada máquina a la que tengamos que conectarnos.


## Extra: buenas practicas ssh

[http://www.cyberciti.biz/tips/linux-unix-bsd-openssh-server-best-practices.html](http://www.cyberciti.biz/tips/linux-unix-bsd-openssh-server-best-practices.html)
