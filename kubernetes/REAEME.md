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

## 标准实例


