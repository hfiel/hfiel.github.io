# Notes about LVM for myself

Please kindly ignore me, I think this is useless for 99.99999% of the population out there.



Commands
```
pvdisplay
vgdisplay
lvdisplay
lvrename
```

manually mounting a logical volume
```
mkdir /data
chown -R nobody:nogroup /data/
chmod 777 /data/

echo '/dev/pve-4tb/userdata /data ext4 defaults 0 2' >> /etc/fstab
```

mountpoint for a container
```
pct set 101 -mp0 /data,mp=/data,backup=0
```
