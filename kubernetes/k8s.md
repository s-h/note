## 基本命令
### 查看pod和工作节点
#### 使用kubectl进行故障排除

    kubectl get 列出资源
    kubectl describe 显示资源的详细信息
    kubectl logs 打印pod和其中容器的日志
    kubectl exec 在pod中的容器上执行命令
    kubectl exec -ti $POD_NAME bash 进入容器shell

#### 修改节点roles
默认node节点roles为none，指定为work：

    kubectl label node k8s-node01(节点名称) node-role.kubernetes.io/worker=worker

#### 允许pod调度到master节点

 kubectl taint nodes --all node-role.kubernetes.io/master-

### cluster details
查看集群状态:

    kubectl cluster-info

查看节点状态：

    kubectl get nodes

### Deploy an app
#### 发布应用
通过创建deployment发布应用

    kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1

+ 寻找可以运行实例的合适节点
+ 在该节点运行应用程序
+ configured the cluster to reschedule the instance on a new Node when needed配置集群在新节点需要时重新调度实例?

获取deployment

    kubectl get deployment

#### 查看应用
kubectl命令可以创建一个代理，该代理会将通信转发到集群范围的专用网络。通过该Control-C终止代理，并且在运行时不显示任何输出。

    kubectl  proxy


获取pod并格式化输出结果

    kubectl get pods -o go-template --template '{{range .items}} {{.metadata.name}}{{"\n"}}{{end}}'

### 查看pod和工作节点
#### pod
Pod是k8s抽象出来的一组一个多个容器，这些容器共享一下资源：
+ 共享存储，当做卷
+ 网络，唯一的集群ip地址
+ 有关每个容易如何运行的信息
Pod 为特定于应用程序的“逻辑主机”建模，并且可以包含相对紧耦合的不同应用容器。

#### work node
一个pod总是运行在**工作节点**, 由**主节点**管理

## 服务
### 服务类型
+ ClusterIP: 通过集群的内部IP暴露服务，服务只能够在集群内部可以访问，默认的ServiceType
+ NodePort：通过每个Node的IP和静态端口（NodePort）暴露服务。NodePort服务会路由到ClusterIP服务，这个ClusterIP服务会自动创建。通过请求<NodeIP><NodePort>，可以从集群的外部访问一个NodePort服务。
+ LoadBalancer: 使用云提供的负载均衡器，可以向外部暴露服务。外部负载均衡器可以路由到NodePort服务和ClusterIP服务。
+ Externalname：通过返回CNAME和它的值，可以将服务映射到externalname字段内容

#### NodePort
默认端口30000-32767，每个节点分配相同的端口，代理到服务中。

#### LoadBalancer
负载均衡是异步创建的，通过Service的status.loadbalancer字段发布出去
