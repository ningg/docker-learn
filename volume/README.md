数据卷管理：

```
# 1.  创建数据卷
docker volume create my-vol

# 2. 查看数据卷列表
docker volume ls

# 3. 查看数据卷详情
docker volume inspect my-vol

# 4. 删除数据卷
docker volume rm my-vol

# 5. 删除容器的时候，删除数据卷
docker rm -v

# 6. 删除未使用的数据卷（没有挂载在任一容器）
docker volume prune

```

数据卷挂载到容器：

```
# 启动容器时，挂载数据卷(下述方式，只是示意操作，暂时无法执行)
$ docker run \
  --name web \
  --mount type=bind,source=/src/webapp,target=/opt/webapp \
  training/webapp

# 查看容器的挂载点
docker inspect web
```
