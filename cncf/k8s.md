<!-- vscode-markdown-toc -->
* 1. [基本命令](#)
	* 1.1. [cluster](#cluster)
		* 1.1.1. [查看集群状态:](#:)
		* 1.1.2. [查看节点状态：](#-1)
		* 1.1.3. [查看支持api](#api)
	* 1.2. [pod](#pod)
		* 1.2.1. [使用kubectl进行故障排除](#kubectl)
		* 1.2.2. [kubectl get pod指定信息](#kubectlgetpod)
		* 1.2.3. [kubectl top pod](#kubectltoppod)
		* 1.2.4. [修改节点roles](#roles)
		* 1.2.5. [允许pod调度到master节点](#podmaster)
		* 1.2.6. [创建pod技巧](#pod-1)
	* 1.3. [deployment](#deployment)
		* 1.3.1. [发布应用](#-1)
		* 1.3.2. [查看应用](#-1)
	* 1.4. [service](#service)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->
# 运维
##  1. <a name=''></a>基本命令
###  1.1. <a name='cluster'></a>cluster
####  1.1.1. <a name=':'></a>查看集群状态:

    kubectl cluster-info

####  1.1.2. <a name='-1'></a>查看节点状态：

    kubectl get nodes

####  1.1.3. <a name='api'></a>查看支持api

```bash
kubectl api-versions
```
###  1.2. <a name='pod'></a>pod
Pod是k8s抽象出来的一组一个多个容器，这些容器共享一下资源：
+ 共享存储，当做卷
+ 网络，唯一的集群ip地址
+ 有关每个容易如何运行的信息
Pod 为特定于应用程序的“逻辑主机”建模，并且可以包含相对紧耦合的不同应用容器。
####  1.2.1. <a name='kubectl'></a>使用kubectl进行故障排除

    kubectl get 列出资源
    kubectl describe 显示资源的详细信息
    kubectl logs 打印pod和其中容器的日志
    kubectl exec 在pod中的容器上执行命令
    kubectl exec -ti $POD_NAME bash 进入容器shell
    kubectl api-resources 列出所有API资源类型及其简写、组名、版本和命名空间范围

####  1.2.2. <a name='kubectlgetpod'></a>kubectl get pod指定信息
    
    # 查看pod及容器名称
    kubectl get pod my-pod -o custom-columns=NAME:.metadata.name,CONTAINERS:.spec.containers[*].name
    # 根据标签查看pod
    kubectl get pod -l foo=bar

####  1.2.3. <a name='kubectltoppod'></a>kubectl top pod

    # 这个命令会显示指定命名空间下特定 Pod 内所有容器的 CPU 和内存使用情况。
    kubectl top pod <pod-name> --containers -n <namespace>

####  1.2.4. <a name='roles'></a>修改节点roles
默认node节点roles为none，指定为work：

    kubectl label node k8s-node01(节点名称) node-role.kubernetes.io/worker=worker

####  1.2.5. <a name='podmaster'></a>允许pod调度到master节点

 kubectl taint nodes --all node-role.kubernetes.io/master-


####  1.2.6. <a name='pod-1'></a>创建pod技巧

    #生成pod.yaml文件
    kubectl run podName -- image nginx --image-pull-policy IfNotPresent --dry-run=client -o yaml > pod.yaml
    #查询pod.sepc中的参数
    kubectl explain pod.spec


###  1.3. <a name='deployment'></a>deployment
####  1.3.1. <a name='-1'></a>发布应用
通过创建deployment发布应用

    kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1

+ 寻找可以运行实例的合适节点
+ 在该节点运行应用程序
+ configured the cluster to reschedule the instance on a new Node when needed配置集群在新节点需要时重新调度实例?

获取deployment

    kubectl get deployment

####  1.3.2. <a name='-1'></a>查看应用
kubectl命令可以创建一个代理，该代理会将通信转发到集群范围的专用网络。通过该Control-C终止代理，并且在运行时不显示任何输出。

    kubectl  proxy


获取pod并格式化输出结果

    kubectl get pods -o go-template --template '{{range .items}} {{.metadata.name}}{{"\n"}}{{end}}'


###  1.4. <a name='service'></a>service
**服务类型**
+ ClusterIP: 通过集群的内部IP暴露服务，服务只能够在集群内部可以访问，默认的ServiceType
+ NodePort：通过每个Node的IP和静态端口（NodePort）暴露服务。NodePort服务会路由到ClusterIP服务，这个ClusterIP服务会自动创建。通过请求<NodeIP><NodePort>，可以从集群的外部访问一个NodePort服务。
+ LoadBalancer: 使用云提供的负载均衡器，可以向外部暴露服务。外部负载均衡器可以路由到NodePort服务和ClusterIP服务。
+ Externalname：通过返回CNAME和它的值，可以将服务映射到externalname字段内容

**NodePort**默认端口30000-32767，每个节点分配相同的端口，代理到服务中。

**LoadBalancer**负载均衡是异步创建的，通过Service的status.loadbalancer字段发布出去
