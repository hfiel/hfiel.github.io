¿Qué ocurre cuando hacemos un rm en la consola para borrar un archivo o un directorio y de repente nos damos cuenta de que nos hemos equivocado?

Tendremos que buscar un software de recuperación.

¿No habría sido mejor que se moviera a una papelera, como ocurre con el equivalente gráfico?

Buenas noticias, se puede hacer, y muy fácilmente.

Basta con instalar el paquete libtrash. En Ubuntu podéis ejecutar este comando:

sudo aptitude install libtrash

En Debian tendríais que ejecutar como usuario root:

aptitude install libtrash

y añadir en tu fichero de usuario .bashrc una nueva variable de entorno LD_PRELOAD (que carga en memoria la librería dinámica que le indiquemos) con la ruta a la librería

echo \"export LD_PRELOAD=/usr/lib/libtrash/libtrash.so.2.4\" >> ~/.bashrc

Ahora puedes cerrar y abrir la consola de nuevo para grabar los cambios o ejecutar

source ~/.bashrc

Ahora vamos a probar nuestros nuevos superpoderes. Creamos un archivo cualquiera

echo \"hola\" > hola.txt

y ahora lo borramos

rm hola.txt

el archivo habrá ido a parar al directorio Trash en nuestro directorio HOME, que es el que nos hace de papelera.

Pero esto puede mejorar aún más: libtrash tiene un archivo de configuración que nos permite indica el directorio a utilizar como papelera, entre otras cosas. Si le indicamos .Trash, que es el archivo que usa Gnome como papelera, ¡cuando borremos un archivo este ira a parar a la papelera de Gnome y podremos borrarlo desde allí de forma gráfica!

echo \"TRASH_CAN = .Trash\" > ~/.libtrash
