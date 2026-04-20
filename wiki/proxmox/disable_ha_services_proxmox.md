# Disable HA services in proxmox

For setups with only a single proxmox instance, the HA features are useless and 1) use some resources that could be better use in VMs and 2) generate logs that are a waste of space

I will STOP and DISABLE `pve-ha-lrm`, which manages the local node resources and `pve-ha-crm` which manages the cluster:

```
systemctl stop pve-ha-lrm
systemctl stop pve-ha-crm
systemctl disable pve-ha-lrm
systemctl disable pve-ha-crm
```

Then you can also STOP and DISABLE `corosync`, in case you do not need it.

```
systemctl stop corosync
systemctl disable corosync
```

And please DO NOT disable the `pve-cluster` service, because it is needed for normal use of PVE. You can momentarily STOP it for temporary work (like doing repairs in filesystems or volumes), but you probably want to keep it enabled.
