# Disable HA services in proxmox

For setups with only a single proxmox instance, the HA features are useless and 1) use some resources that could be better use in VMs and 2) generate logs that are a waste of space

I will STOP and DISABLE `pve-ha-lrm`, which manages the local node resources and `pve-ha-crm` which manages the cluster:

```
systemctl stop pve-ha-lrm
systemctl stop pve-ha-crm
systemctl disable pve-ha-lrm
systemctl disable pve-ha-crm
```
