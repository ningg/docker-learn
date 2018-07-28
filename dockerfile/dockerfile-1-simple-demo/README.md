目标：

* 观察基于 Dockerfile，构建 image 的过程

操作：

```
# 构建镜像
docker build -t nginx:v3 .

# 基于构建的镜像，启动容器
docker run nginx:v3

# 新的窗口中，登录容器内环境，观察
docker exec -it [container_hashcode] /bin/bash
```
