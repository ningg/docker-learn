# 在 Kubernetes 上部署应用

场景描述：

* 本地编写应用
* 创建镜像
* 使用 Kubernetes 部署
* 在 Kubernetes 进行简单的管理

## 创建 minikube 集群

参照其他地方：

1. 安装 minikube
2. 启动 minikube


## 创建 NodeJS 应用程序

本地编写 `server.js` 文件：

```
var http = require('http');

var handleRequest = function(request, response) {
  console.log('Received request for URL: ' + request.url);
  response.writeHead(200);
  response.end('Hello World!');
};
var www = http.createServer(handleRequest);
www.listen(8080);
```

上面是一个 JS 文件，使用 Node 可以启动：

```
node server.js
```

然后在浏览器中，可以查看 http://localhost:8080 ，会显示 `Hello World!`.


## 创建 Docker 容器镜像

在 `hellonode` 文件夹中创建一个 `Dockerfile` 命名的文件，用于描述镜像：

```
FROM node:6.9.2
EXPOSE 8080
COPY server.js .
CMD node server.js
```

此时使用 Minikube, 而不是将 Docker 镜像 push 到 Registry。因此，需要使用 Minikube VM 相同的 Docker 主机，来构建镜像，使得 minikube 能够找到镜像。具体，需要在本机设置 Docker 相关的环境变量：

```
# 将本机的 Docker 相关环境变量，指向 Minikube VM
eval $(minikube docker-env)
```

Note: 

> 不再使用 minikube VM 时，采用 `eval $(minikube docker-env -u)` 命令，来取消环境变量的设置。

使用 Minikube 的 Docker 守护进程，build Docker 镜像：

```
docker build -t hello-node:v1 .
```

## Kubernetes 集群

Kubernetes 集群，几个方面：

1. Deployment：创建 Deployment
2. Service：创建 Service
3. 更新
4. 删除


### Deployment：创建 Deployment

Kubernetes Pod 是一个 or 一组容器的集合，用于共享资源，可以看做一个逻辑主机。

Kubernetes Deployment 管理 Pod 的创建和扩展，如果 Pod 终止，则，重新启动一个（组） Pod 对应的容器.

当前 Case 中，Pod 只是一个容器，具体，使用 `kubectl run` 命令创建 Deployemnt 来管理 Pod：

```
kubectl run hello-node --image=hello-node:v1 --port=8080
```

查看 Deployment：

```
kubectl get deployments

NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-node   1         1         1            1           5m
```

查看 Pods：

```
kubectl get pods

NAME                          READY     STATUS    RESTARTS   AGE
hello-node-658d8f6754-kwv4h   1/1       Running   0          6m
```

查看整个执行过程中的 Events：

```
kubectl get events

LAST SEEN   FIRST SEEN   COUNT     NAME                                           KIND         SUBOBJECT                     TYPE      REASON                  SOURCE                  MESSAGE
7m          7m           1         hello-node-658d8f6754-kwv4h.154ccfbb6eab8038   Pod                                        Normal    Scheduled               default-scheduler       Successfully assigned hello-node-658d8f6754-kwv4h to minikube
7m          7m           1         hello-node-658d8f6754-kwv4h.154ccfbb7e1cfde0   Pod                                        Normal    SuccessfulMountVolume   kubelet, minikube       MountVolume.SetUp succeeded for volume "default-token-f482p"
7m          7m           1         hello-node-658d8f6754-kwv4h.154ccfbb98e8629d   Pod          spec.containers{hello-node}   Normal    Pulled                  kubelet, minikube       Container image "hello-node:v1" already present on machine
7m          7m           1         hello-node-658d8f6754-kwv4h.154ccfbb9b2ea1c6   Pod          spec.containers{hello-node}   Normal    Created                 kubelet, minikube       Created container
7m          7m           1         hello-node-658d8f6754-kwv4h.154ccfbba16ef31b   Pod          spec.containers{hello-node}   Normal    Started                 kubelet, minikube       Started container
7m          7m           1         hello-node-658d8f6754.154ccfbb69b860c0         ReplicaSet                                 Normal    SuccessfulCreate        replicaset-controller   Created pod: hello-node-658d8f6754-kwv4h
7m          7m           1         hello-node.154ccfbb67bc8996                    Deployment                                 Normal    ScalingReplicaSet       deployment-controller   Scaled up replica set hello-node-658d8f6754 to 1
```

查看 kubectl 配置：

```
kubectl config view

apiVersion: v1
clusters:
- cluster:
    certificate-authority: /Users/guoning/.minikube/ca.crt
    server: https://192.168.99.100:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /Users/guoning/.minikube/client.crt
    client-key: /Users/guoning/.minikube/client.key

```


细节参考：

* [Kubernetes 系列：使用 minikube 部署应用](http://ningg.top/kubernetes-series-02-deploy-an-application/)




## 附录

关于 minikube 的 自带Docker 环境变量说明：

```
$ minikube docker-env
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/guoning/.minikube/certs"
export DOCKER_API_VERSION="1.35"
# Run this command to configure your shell:
# eval $(minikube docker-env)

$ minikube docker-env -u
unset DOCKER_TLS_VERIFY
unset DOCKER_HOST
unset DOCKER_CERT_PATH
unset DOCKER_API_VERSION
# Run this command to configure your shell:
# eval $(minikube docker-env)
```

