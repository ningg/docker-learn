# Docker Machine

目标：多种环境和 OS 上，快速安装 Docker 环境

* 物理机
* 虚拟机
* 云主机

具体操作系统：

* Linux
* Windows
* MacOS

## 常用操作

`docker-machine` 常用的操作：

```
# 查看 Docker 服务器列表
docker-machine ls

# 创建一个 Docker 服务器
# 以 virtualbox 驱动，创建一个 Docker 服务器，命名为 mananger
docker-machine create -d virtualbox manager

Running pre-create checks...
Creating machine...
(manager) Copying /Users/guoning/.docker/machine/cache/boot2docker.iso to /Users/guoning/.docker/machine/machines/manager/boot2docker.iso...
(manager) Creating VirtualBox VM...
(manager) Creating SSH key...
(manager) Starting the VM...
(manager) Check network to re-create if needed...
(manager) Waiting for an IP...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with boot2docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env manager


# 查看所有 Docker 服务器
docker-machine ls

# 以 ssh 方式登录 Docker 服务器
docker-machine ssh manager

# 终止 Docker 服务器
docker-machine stop manager

# 删除 Docker 服务器
docker-machine rm manager
```
