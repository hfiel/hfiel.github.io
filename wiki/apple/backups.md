Como hacer backups, a la manera de JWZ. Me parece especialmente deliciosa la frase final del punto 2.3 XD:

[http://www.jwz.org/doc/backups.html]

NOTA: en el caso de ciertos equipos (PE, Macbook Pro Santa Rosa - Mid 2007), no es posible arrancar con una unidad externa USB. Sólo podrás hacerlo desde FireWire (o por red, o por otro equipo arrancado en modo disco destino, o...)

Hello, this is a public service announcement. I am here to tell you about backups. It\'s very simple.

! Option 1:
Learn not to care about your data. Don\'t save any old email, use a film camera, and only listen to physical CDs and not MP3s. If you have no posessions, you have nothing to lose.

! Option 2:
goes like this:

* You have a computer. It came with a hard drive in it. Go buy two more drives of the same size or larger. If the drive in your computer is SATA2, get SATA2. If it\'s a 2.5\" laptop drive, get two of those. Brand doesn\'t matter, but physical measurements and connectors should match.

* Get external enclosures for both of them. The enclosures are under $30.

* Put one of these drives in its enclosure on your desk. Name it something clever like \"Backup\". If you are using a Mac, the command you use to back up is this:

::sudo rsync -vaxE --delete --ignore-errors / /Volumes/Backup/::

If you\'re using Linux, it\'s something a lot like that. If you\'re using Windows, go fuck yourself.

* If you have a desktop computer, have this happen every morning at 5AM by creating a temporary text file containing this line:

::0 5 * * * rsync -vaxE --delete --ignore-errors / /Volumes/Backup/::

and then doing ::sudo crontab -u root that-file::

If you have a laptop, do that before you go to bed. Really. Every night when you plug your laptop in to charge.

* If you\'re on a Mac, that backup drive will be bootable. That means that when (WHEN) your internal drive scorches itself, you can just take your backup drive and put it in your computer and go. This is nice.

* When (WHEN) your backup drive goes bad, which you will notice because your last backup failed, replace it immediately. This is your number one priority. Don\'t wait until the weekend when you have time, do it now, before you so much as touch your computer again. Do it before goddamned breakfast. The universe tends toward maximum irony. Don\'t push it.

* That third drive? Do a backup onto it the same way, then take that to your office and lock it in a desk. Every few months, bring it home, do a backup, and immediately take it away again. This is your \"my house burned down\" backup. 

\"OMG, three drives is so expensive! That sounds like a hassle!\" Shut up. I know things. You will listen to me. Do it anyway.



!Addendum A:

Mac users: for the backup drive to be bootable, you need to do two things:
* When you first format the drive, set the partition type to \"GUID\", not \"Apple Partition Map\";

* Before doing your first backup, Get Info on the drive and un-check \"Ignore ownership on this drive\" under \"Ownership and permissions.\" 

You can test whether it\'s bootable by holding down Option while booting and selecting the external drive. 

!Addendum B:

RAID is a waste of your goddamned time and money. Is your personal computer a high-availability server with hot-swappable drives? No? Then you don\'t need RAID, you just need backups.
