# Setting up a Linux ROCm enviroment for using with Radeon RX 6700 XT

The GPU Radeon RX 6700 XT is not officially supported by the ROCM under Linux.

However, several users report success in seting such environemnt, so I will be making here notes about my advances and steps regarding this.

My setup:

 - AMD Ryzen 5600
 - 32 GB RAM
 - Ubuntu 24.04 LTS
 - Gigabyte Radeon RX 6700 XT EAGLE 12G

Desired goals:

 - PyTorch 2.4
 - Jupyter notebook 7.2

Both should be running using the GPU.

## First steps

After a clean install of Ubuntu and installation of some basic (and unrelated) tools, we start by updating the system and checking python versions-

```
 sudo apt-get update && sudo apt-get upgrade
 python3 --version
```
