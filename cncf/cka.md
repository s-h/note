# cka考试学习笔记
根据2021-2[考试内容](https://training.linuxfoundation.cn/certificates/1)

+ 集群架构，安装和配置：25%
+ 工作负载和调度：15%
+ 服务和网络：20%
+ 存储：10%
+ 故障排除：30%

## 集群架构，安装和配置：25%

### 管理基于角色的访问控制（RBAC）
[使用 RBAC 鉴权](https://kubernetes.io/zh/docs/reference/access-authn-authz/rbac/)
基于角色（Role）的访问控制（RBAC）是一种基于组织中用户的角色来调节控制对 计算机或网络资源的访问的方法。
RBAC API 声明四种Kubernlletes对象Role、ClusterRole、RoleBinding、ClusterRoleBinding
#### role、ClusterRole
Role 总是用来在某个名字空间 内设置访问权限；在你创建 Role 时，你必须指定该 Role 所属的名字空间。
与之相对，ClusterRole 则是一个集群作用域的资源。这两种资源的名字不同（Role 和 ClusterRole）是因为 Kubernetes 对象要么是名字空间作用域的，要么是集群作用域的， 不可两者兼具。

  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    namespace: default
    name: pod-reader
  rules:
  - apiGroups: [""] # "" 标明 core API 组
    resources: ["pods"]
    verbs: ["get", "watch", "list"]

#### RBAC试题
Task
创建一个名为deployment-clusterrole且仅允许创建一下资源类型的新ClusterRole：
+ Deployment
+ StatefulSet
+ DaemonSet
在现有的namespace app-team1中创建一个名为cicd-token的新ServiceAccount。

限于namespace app-team1，将新的ClusterRole deployment-clusterrole绑定到新的ServiceAccount cicd-token。

  kubectl create clusterrole deployment-clusterrole --verb=create --resource=deployments,statefulsets,daemonsets
  kubectl -n app-team1 create serviceaccount cicd-token
  kubectl -n app-team1 create rolebinding cicd-token-binding --clusterrole=deployment-clusterrole --serviceaccount=app-team1:cicd-token
  kubectl -n app-team1 describe rolebindings.rbac.authorization.k8s.io cicd-token-binding

### 使用Kubeadm安装基本集群
### 管理高可用性的Kubernetes集群
### 设置基础架构以部署Kubernetes集群
### 使用Kubeadm在Kubernetes集群上执行版本升级
### 实施etcd备份和还原

## 工作负载和调度：15%

### 了解部署以及如何执行滚动更新和回滚
### 使用ConfigMaps和Secrets配置应用程序
### 了解如何扩展应用程序
### 了解用于创建健壮的、自修复的应用程序部署的原语
### 了解资源限制如何影响Pod调度
### 了解清单管理和通用模板工具

## 服务和网络：20%

### 了解集群节点上的主机网络配置
### 理解Pods之间的连通性
### 了解ClusterIP、NodePort、LoadBalancer服务类型和端点
### 了解如何使用入口控制器和入口资源
### 了解如何配置和使用CoreDNS
### 选择适当的容器网络接口插件

## 存储：10%

### 了解存储类、持久卷
### 了解卷模式、访问模式和卷回收策略
### 理解持久容量声明原语
### 了解如何配置具有持久性存储的应用程序

## 故障排除：30%

### 评估集群和节点日志
### 了解如何监视应用程序
### 管理容器标准输出和标准错误日志
### 解决应用程序故障
### 对群集组件故障进行故障排除
### 排除网络故障
