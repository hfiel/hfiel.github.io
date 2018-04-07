# Ubuntu 10.04 sin monitor

Acabo de terminar de montar un Ubuntu 10.04 que voy a dejar quietecito en una esquina y sólo con un cable de red y el de corriente.

Pretendo usarlo principalmente como servidor de almacenamiento y descarga, y además de a las carpetas compartidas, necesito poder acceder a él por medio de escritorio remoto para poder ver la interfaz gráfica al completo. Así que configuro el autologin y el sistema de escritorio remoto para que se pueda controlar sin problemas. Como lo uso yo solo en mi propia red, le quito las claves para mayor comodidad.

Cuando lo tengo todo listo y probado, lo llevo a su lugar definitivo y me encuentro con que necesita tener un monitor conectado para que pueda llegar a arrancar.

Tras mucho dar vueltas y probar soluciones varias, en los foros de ubuntu encuentro la solución:
(http://ubuntuforums.org/showthread.php?t=1452600&page=3)

En concreto, la aportada en el mensaje número 24 por paosms funciona a la perfección:

- Editamos /etc/default/grub

```
 sudo nano /etc/default/grub
```

- Buscamos la línea GRUB_CMDLINE_LINUX=""
y le añadimos nomodeset al final. Nos queda esto:

```
 GRUB_CMDLINE_LINUX="nomodeset"
```

- Salimos y guardamos y actualizamos grub mediante:

```
 update-grub
```

- Lo siguiente es crear un fichero xorg.conf de pega:

```
 sudo nano /etc/X11/xorg.conf
```


- En el fichero ponemos lo siguiente:

```
 Section "Device"
 Identifier "VNC Device"
 Driver "vesa"
 EndSection

 Section "Screen"
 Identifier "VNC Screen"
 Device "VNC Device"
 Monitor "VNC Monitor"
 SubSection "Display"
 Modes "1024x768"
 EndSubSection
 EndSection

 Section "Monitor"
 Identifier "VNC Monitor"
 HorizSync 30-70
 VertRefresh 50-75
 EndSection
```


- Guardamos y reiniciamos. Ya podemos quitar tranquilamente el cable, que el sistema arrancará (al menos, hasta que se le rompa otra tripa, que te obligue a conectar de nuevo el monitor).
