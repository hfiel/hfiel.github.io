!Anotaciones varias sobre los comandos ip (iproute2) y relacionados.

!!Chuleta de comandos IP de redhat:

[https://access.redhat.com/articles/ip-command-cheat-sheet]

!!!En pdf:

[https://access.redhat.com/sites/default/files/attachments/rh_ip_command_cheatsheet_1214_jcs_print.pdf]

!!Lista de alternativas a los antiguos comandos:

[https://dougvitale.wordpress.com/2011/12/21/deprecated-linux-networking-commands-and-their-replacements/]


!!Razones para dejar de usar ifconfig:

[http://inai.de/2008/02/19]


!!Si a pesar de todo quieres ifconfig...

El comando ifconfig (y todo net-tools) lleva sin actualizarse "de verdad" desde 2001, a día de hoy básicamente se le meten bugfixes y parches. Hay muchas cosas que no pueden hacerse con él en redes modernas, principalmente debido a que usa cosas bastante antiguas del kernel como ioctl para interactuar con las redes. Como ejemplos de problemas conocidos, ifconfig la lía con algunos tipos de túneles y subredes, y no es capaz de trabajar correctamente con enrutamientos multi-path.

De hecho hay sistemas que ya no lo incluyen.

Si tienes que usarlo pero el sistema ya no lo trae, primero busca bien (es posible que este escondido y no lo tengas en el path), y si de verdad no viene puedes intentar volver a ponerlo.

!!!En debian:

[https://linuxconfig.org/how-to-install-missing-ifconfig-command-on-debian-linux]
