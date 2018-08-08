# swarm mode 构造 Docker 原生集群



## 创建 Docker swarm 集群

创建 docker 集群，涵盖 1 个 manager 和 2 个 worker：

```
# 1. 创建一个 Docker 节点，命名为 manager
docker-machine create -d virtualbox manager

# 2. 登录 manager 节点，完成 swarm 模式初始化
docker-machine ssh manager
# 在 manager 节点上，完成下述 swarm 初始化
docker swarm init --advertise-addr 192.168.99.100

...
Swarm initialized: current node (j9iopsxdcrwm0ayughex405zh) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-4jp7adsqdwyjw1kkijnstkvj1t3xkcmmmzr6oqgmahz0tmqkv7-12mk3k2zv97hju7gafpqlia7f 192.168.99.100:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
...

# 3. 创建 2 个 worker 节点
docker-machine create -d virtualbox worker1
docker-machine create -d virtualbox worker2

# 4. 分别 ssh 登录两个 worker 节点，并执行 swarm join 命令
docker-machine ssh worker1
docker-machine ssh worker2

...
docker swarm join --token SWMTKN-1-4jp7adsqdwyjw1kkijnstkvj1t3xkcmmmzr6oqgmahz0tmqkv7-12mk3k2zv97hju7gafpqlia7f 192.168.99.100:2377
...

# 5. 查看 Docker 集群状态（ssh 登录到 manager 节点，执行下述命令）
$ docker node ls

ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
j9iopsxdcrwm0ayughex405zh *   manager             Ready               Active              Leader              18.06.0-ce
ecx168drw3tc0ct4hzb9kf90j     worker1             Ready               Active                                  18.06.0-ce
sco7gy7tzx9ciu9gb5pk8zu0r     worker2             Ready               Active                                  18.06.0-ce
```


## 创建 Docker 服务

两种方式：

1. 使用镜像
2. 使用 docker-compose 文件

### 使用镜像

连接到 manager 节点，单次创建一个服务：

* docker service create


### 使用 docker compose

基于 docker-compose.yml 文件，单次创建一组服务





