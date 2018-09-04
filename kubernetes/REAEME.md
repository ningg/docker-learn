# Kubernetes 试用

几个场景：

1. 简单试用：命令行方式，启动 Deployment
2. 标准实例：定义 Deployment，再操作启动

## 简单试用

准备镜像：

```
eval $(minikube docker-env)

cd hellonode

docker build -t hello-node:v1 .
```

启动 Deployment：

```
kubectl run hello-node --image=hello-node:v1 --port=8080
```

查看当前 Pod 信息:

```
kubectl get deployments

kubectl get pods

# 查看整个过程执行的 Event
kubectl get events
```

删除 Deployment：

```
kubectl delete deployment hello-node
```


## 标准实例

创建 Deployment 资源定义的文件：

```
cd deployment

vim nginx-deployment.yml
```

启动 Deployment：

```
kubectl create -f nginx-deployment.yml
```

查看细节：

```
# 查看 Deployment
# 刚执行时，显示的AVAILABLE数量时0
kubectl get deployments

# 滚动升级
kubectl set image deployment/nginx-deployment nginx=nginx:1.12.2

# 查看 Replica Set（RS）
# 每次升级 Deployment，都会生成一个新的 RS
kubectl get rs
```





