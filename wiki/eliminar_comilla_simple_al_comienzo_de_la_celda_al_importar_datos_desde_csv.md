# Eliminar comilla simple al comienzo de la celda al importar datos desde CSV

Cuando en Calc importamos valores desde un CSV, en ocasiones los valores importados no permiten dar formato adecuadamente.

Esto se produce porque los cambios de codificación al importar el contenido agregan al comienzo de las celdas una comilla simple invisible.

Esto se observa por ejemplo al importar fechas: cuando intentamos cambiar la configuración de la celda para que muestre la fecha con otro formato, la visualización de la celda no cambia.
Aunque resulta invisible, al comienzo de contenido de la celda hay una comilla simple que está impidiendo a Calc reconocerla adecuadamente como fecha.

Notar que la comilla únicamente aparece tras aplicar formato a la celda.

## Eliminar una única comilla

Para ver esta comilla, tenemos que seleccionar la celda y ediar su valor (haciendo clic en el cuadro de valor o pulsando 'F2'). Eso nos mostrará la comilla en dicho cuadro, y podremos eliminarla, tras lo cual el formato se aplicará correctamente.

## Eliminar la comilla de múltiples celdas

Cuando se ha llevado a cabo una importación masiva de datos, modificsr una a una las celdas no es factible. En estos casos debemos recurrir a las opciones de Buscar y reemplazar.

1. Seleccionamos todas las celdas/filas/columnas donde queremos aplicar el formato y eliminar las comillas.

2. Aplicamos el formato deseado (recordemos que la comilla no aparece hasta aplicar formato)

3. Aún con las celdas seleccionadas, abrimos el menú 'Editar' > 'Buscar y reemplazar'.

4. En el campo 'Búsqueda' escribimos:

```
^.*$
```

5. En el campo 'Reemplazar con' escribimos:

```
&
```

6. Activamos las opiones 'Aplicar solo a la selección actual' y 'Usar expresiones regulares'

7. Hacemos clic en 'Aceptar'
