!Crear una imagen ISO de un disco

!!Cómo crear una ISO desde la terminal, como los pros.

1. Introducir el disco en la unidad

2. Abrir el terminal, y buscar el dispositivo al que corresponde la unidad, con el comando \'\'drutil status\'\'.
 \'\'$drutil status\'\'
 Vendor   Product           Rev 
 MATSHITA DVD-R   UJ-835E   GAND
           Type: DVD-ROM              Name: ##red##/dev/disk1##
      Cur Write:    8x DVD          Sessions: 1
      Max Write:    8x DVD            Tracks: 1
   Overwritable:   00:00:00         blocks:        0 /   0.00MB /   0.00MiB
     Space Free:   00:00:00         blocks:        0 /   0.00MB /   0.00MiB
     Space Used:  364:08:27         blocks:  1638627 /   3.36GB /   3.13GiB
    Writability: 
      Book Type: DVD-ROM

3. Desmonta el disco:
 \'\'$diskutil unmountDisk /dev/disk1\'\'
 Disk /dev/disk1 unmounted

4. Crea la ISO por medio de dd (y espera a que termine):

 $\'\'dd if=/dev/disk1 of=imagen.iso bs=2048\'\'

5. Prueba la ISO montando la imagen o abriéndola desde finder:

 $\'\'hdid imagen.iso\'\'

6. Graba la imagen o almacénala
