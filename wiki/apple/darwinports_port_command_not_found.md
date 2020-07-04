!Error en Darwinports: port command not found

Vía: [http://bparanj.blogspot.com/2007/03/sudo-port-command-not-found.html]

export PATH=$PATH:/opt/local/bin
export MANPATH=$MANPATH:/opt/local/share/man
export INFOPATH=$INFOPATH:/opt/local/share/info

Included the above lines in .bash_profile (my mac is using bash shell) and voila a nagging problem sudo: port: command not found is gone!!!

sudo port -d selfupdate
