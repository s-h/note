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