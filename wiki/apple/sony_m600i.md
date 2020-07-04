Precioso. Pequeño. Potente. Táctil. Multimedia. Teclado QWERTY. 3G. ¡Hasta funciona el SCUMMVM! Pero el muy #$%@ NO sincroniza en un MAC. Simplemente NO FUNCIONA. Así que tras buscar un poco (un mucho) encontré dos pequeñas soluciones.

!Agenda:

Simplemente enciendes el bluetooth (si quieres, puedes sincronizar los dos dispositivos antes para evitar confirmaciones extra), te vas a la agenda en el MAC, seleccionas todos los contactos (**cmd+a**), y le das a **Tarjeta** > **Enviar estas tarjetas**. Se abre el menú bluetooth, buscas el móvil, lo envías y a esperar. Cuando en el móvil te aparezca la vCard recibida (tranquilo, aunque mandes 500 contactos sólo aparece una vCard) le das a **ver** y a **guardar**. Listos. Seguirás teniendo el mensaje en la bandeja de entrada del móvil, pero ya puedes borrarlo. 

Por cierto, que la agenda del móvil ordena por apellido, no por nombre, y lo primero que sale son los contactos con paréntesis en el apellido, como cuando guardas a alguien como **Pepe (el del bar)**, que sale **(el del bar), Pepe**. Raya bastante, así que buscaré cómo cambiar el orden en el móvil.

!Calendario:

!!!Actualización: en Leopard, envias la agenda al movil y ejecutas el archivo. iCal actualiza el contenido sin problemas.

Esta es mas chunga. Pero por suerte algún super-friki se compró un peazo Sony P990 (primo hermano del m660), y se curro la aplicación que convierte eventos del calendario del móvil (.vcs) en algo entendible por iCal.

La web del nota: [http://www.girard.li]. El programa se llama **p990iCal**

Una vez que lo tienes descargado y copiado en Aplicaciones, abre iCal, HACES COPIA DE SEGURIDAD, y creas un nuevo calendario, que será donde se guarden los eventos del móvil (así que ponle algún nombre lógico, como **móvil** o **sony**).

Abres y configuras el p990iCal, desde su menú de preferencias (**p990iCal** > **Preferences**). Puedes dejar casi todo como está, pero le tienes que decir donde guardar los eventos del teléfono, en **Put events created on phone into:** y en esa lista seleccionas el calendario que has creado antes en el iCal. Cierras las preferencias y estamos listos para empezar.


Pasos a seguir cada vez que quieras sincronizar:

*Abres el calendario del móvil, y le das a **Más** > **Enviar la carpeta** y la mandas por bluetooth al MAC. De nuevo, si ya tienes los dos equipos sincronizados es más fácil. Recibes un archivo **Todo.vcs**.

*Cierras iCal, y abres p990iCal (si tienes iCal abierto, el programa lo cierra por ti). NO he probado que pasa si abres iCal mientras tanto, tú mismo. Le das a **choose** y le dices donde está el archivo **Todo.vcs**

*Le das al dibujo del iCal y el móvil (un p990i), y te pregunta en que carpeta guardar el archivo del móvil. Este archivo tiene un nombre según fecha y hora, de la forma: 2phone_añomesdiaThoraminutosegundo_0.vcs. OJO, no le des a ningún botón en la ventana que se abre todavía, espera un poco más.

*Mandas ese archivo al teléfono, y cuando lo hayas recibido le das a **ver**, te dice que el archivo contiene X entradas de calendario, y le das a **guardar**. Cuando acabe, el sólo borra el archivo de la lista de recibidos.

*Ya está todo sincronizado. Ahora, si la sincronización ha ido bien, le das a **Yes** en la ventana que se abrió en p990iCal (y si ha ido mal, pues a **Cancel**, pero aún no lo he sufrido).

Sólo hay un problema, cuando borras un evento en el móvil, no se borra en el iCal (porque el evento de iCal puede estar en otro calendario, y el programa sólo modifica el calendario asociado al móvil. Si alguna vez la agenda del móvil se vuelve loca, la borras entera, y haces el proceso otra vez: volverás a tener los eventos en el móvil tal y como estén en el iCal.

Parece muy largo, pero cuando lo hayas hecho un par de veces, todo se simplifica y es bastante sencillo. Al menos, más que intentar sincronizar a mano los dos calendarios.
