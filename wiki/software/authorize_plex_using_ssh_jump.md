# Authorize a Plex server using a SSH jump

When you install Plex in a remote server (for example, a container in proxmox) you may have issues authorizing the server against the plex account.

This is because when you browse to plex.tv you are doing it through your LOCAL machine, no the server.
The solution is:
1) set SSH on the plex server
2) set an SSH jump on your local machine to the plex server
3) open your LOCAL browser pointing to the web server in the remote plex machine

## SSH on plex

this is for a plex container set in proxmox using the community scripts. Adapt as you need.


set a root password in your plex container
create a ssh keypair
change the config in `/etc/ssh/sshd_config` to:

```
PermitRootLogin yes
```

copy the private key to your local system

## SSH jump using linux

here i am using 192.168.20.22 as the plex server container IP.

```
ssh root@192.168.20.22 -L 8888:localhost:32400
```

open your local browser and go to 

http://localhost:8888/web

## SSH jump using putty

create the tunnel in GUI with
source port: 8888
destination: 127.0.0.1:32400

open your local browser and go to 
http://127.0.0.1:8888/web

