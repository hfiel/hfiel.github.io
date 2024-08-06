# Setting up a Linux ROCm enviroment for using with Radeon RX 6700 XT

The GPU Radeon RX 6700 XT is not officially supported by the ROCM under Linux.

However, several users report success in seting such environemnt, so I will be making here notes about my advances and steps regarding this.

My setup:

 - AMD Ryzen 5600
 - 32 GB RAM
 - Ubuntu 24.04 LTS
 - Gigabyte Radeon RX 6700 XT EAGLE 12G

Desired goals:

 - All inside a container (to avoid messing with the native OS versions)
 - PyTorch 2.4
 - Jupyter notebook 7.2

Both programs should be running using the GPU.

## First steps

After a clean install of Ubuntu and installation of some basic (and unrelated) tools, we start by updating the system and checking python versions-

```
 sudo apt-get update && sudo apt-get upgrade
 python3 --version
```

Next we must install docker (I recommend following the instructions for [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/). This is a copy of the current instructions as the time of writing:

Set up apt repo:
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

Install the docker packages:
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Verify docker works:

```
sudo docker run hello-world
```

Now we configure the docker group, and add our own user to that group (note that the first command will probably fail, since you should already have a docker group):

```
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

Verify that you can run docker without sudo:

```
docker run hello-world
```
