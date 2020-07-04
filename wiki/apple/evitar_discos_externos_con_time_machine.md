Cuando conectamos un nuevo disco a nuestro equipo, Time Machine nos ofrece la posibilidad de usarlo, pero a veces no queremos esto. Si conectamos muchos discos, el mensaje llega a ser molesto, así que tenemos dos posibles soluciones:

! 1- Con Time Machine activado:
en el terminal ponemos lo siguiente
 defaults write com.apple.TimeMachine DoNotOfferNewDisksForBackup -bool YES

! 2- Con Time Machine desactivado:
En el terminal ponemos:
 defaults write /Library/Preferences/com.apple.TimeMachine
 DoNotOfferNewDisksForBackup -bool YES

Y para deshacer los cambios, lo mismo pero poniendo NO en lugar de YES
