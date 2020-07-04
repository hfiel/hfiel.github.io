::find $1 -type f -name .DS_Store -print0 | xargs -0 rm::

Lo guardas como .sh (PE, limpiads.sh), y le das permisos de ejecución (744).

la manera de usarlo es:
::./limpiads.sh directorio_a_limpiar::

Ojo, si se te va la pinza y limpias los .DS_Store de alguna carpeta del Mac puedes liarla. No debería pasar nada (se vuelve a crear sólo) pero nunca se sabe. Si tienes una carpeta con miles de fotos y borras el fichero, puede tardar un buen rato en regenerarse.

Si siempre vas a limpiar el mismo directorio, puedes cambiar el $1 del script por la ruta al directorio objetivo, y ya solo tienes que usar:
::./limpiads.sh::

Una vez ejecutado, te borra los archivos .Ds_Store del directorio y de todos los que haya por debajo.
