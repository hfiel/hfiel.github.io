# Fixing docker storage driver issues in Ubuntu

Updating an Ubuntu system with a simple `apt-get update` and `upgrade` I faced the following issue (I've cleaned the output a bit):

```log
Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service" and "journalctl -xe" for details.
invoke-rc.d: initscript docker, action "start" failed.
...
 Failed to start Docker Application Container Engine.
 docker.service: Unit entered failed state.
 docker.service: Failed with result 'exit-code'.
...
dpkg: error processing package docker-ce (--configure):
 subprocess installed post-installation script returned error exit status 1
Errors were encountered while processing:
 docker-ce
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

And docker was not responding anymore, stuck in a half-updated status.

Checking `systemctl status docker.service` and `journalctl -u docker.service` I found a message regarding several storage drivers being defined:

```log
Error starting daemon: error initializing graphdriver: /var/lib/docker contains several valid graphdrivers: overlay, overlay2; Please cleanup or explicitly choose storage driver (-s <DRIVER>)

```

After googling a bit, [I found someone which had a similar issue](https://tutel.me/c/programming/questions/45103803/not+able+to+start+docker+on+ubuntu+16042+lts+error+initializing+graphdriver), and [the solution](https://docs.docker.com/storage/storagedriver/overlayfs-driver/#configure-docker-with-the-overlay-or-overlay2-storage-driver)

Please note: if you *change* to a different storage driver, your images and containers may seem to __dissapear__ (although they will probably be still there). Check [how to backup images](backup_and_restore_docker_images.md) for information about changing the storage driver.


## Fixing the isue: define the storage driver

1.- Stop Docker service

```bash
sudo systemctl stop docker
```

2.- Edit (or create) `/etc/docker/daemon.json` and define the storage driver (use the current storage driver your system is using)

```json
{
  "storage-driver": "overlay2"
}
```

3.- Restart Docker service

```bash
sudo systemctl start docker
```

4.- Verify with `docker info`

```log
...
Storage Driver: overlay2
...
```
