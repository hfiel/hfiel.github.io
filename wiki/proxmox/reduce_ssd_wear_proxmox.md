# Reduce SSD wear in proxmox

In order to reduce the write cycles to a system SSD in proxmox, we can 1) remove unneeded services 2) redirect logs to RAM 3) reduce logs

Please note: reducing information created by logs or moving them to RAM may make very difficult troubleshoting your setup.

## Remove services

If you do not need HA (cluster) features, remove the HA services as explained [here](https://hfiel.github.io/wiki/proxmox/disable_ha_services_proxmox.html)

## Log2RAM

You can install [Log2RAM](https://github.com/azlux/log2ram) to keep the logs on memory and only write them to SSD periofically. This saves a lot of write cycles

Note that I am using Proxmox 9 (based on Debian 13). This may not work on other versions. ALso, I am using the instructions from azlux BUT adapted for the proxmox shell, which means this is being done as root (and no with sudo).

Please be careful.

```
. /etc/os-release
wget -O /usr/share/keyrings/azlux-archive-keyring.gpg https://azlux.fr/repo.gpg
tee /etc/apt/sources.list.d/azlux.list >/dev/null <<EOF
deb [signed-by=/usr/share/keyrings/azlux-archive-keyring.gpg] http://packages.azlux.fr/debian/ $VERSION_CODENAME main
EOF
[ "$VERSION_CODENAME" = trixie ] && tee /etc/apt/preferences.d/log2ram.pref >/dev/null <<EOF
Package: log2ram
Pin: origin packages.azlux.fr
Pin-Priority: 1001
EOF
apt update
apt install log2ram
```

after this, please restart your proxmox
