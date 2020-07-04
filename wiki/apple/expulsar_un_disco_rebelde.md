Cuando tenemos conectado un disco externo y lo expulsamos, muchas veces sale el mensaje de \"En uso\", y aunque cerremos todas las aplicaciones seguimos sin poder cerrarlo. En estos casos, tirando de terminal:
 df
Nos devuelve la lista de los volúmenes montados actualmente, localizamos el que nos interesa y anotamos su ruta. Para expulsarlo:
 hdiutil eject -force /Volumes/VOLUMENREBELDE
¡Y voilá! El disco se desmonta de manera segura sin protestar más.
