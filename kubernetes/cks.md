cks备考笔记

# 集群安装：10%


## 使用网络安全策略来限制集群级别的访问

## 使用CIS基准检查Kubernetes组件(etcd, kubelet, kubedns, kubeapi)的安全配置

## 正确设置带有安全控制的Ingress对象

## 保护节点元数据和端点

## 最小化GUI元素的使用和访问

## 在部署之前验证平台二进制文件



# 集群强化：15%


## 限制访问Kubernetes API

## 使用基于角色的访问控制来最小化暴露

## 谨慎使用服务帐户，例如禁用默认设置，减少新创建帐户的权限

## 经常更新Kubernetes



# 系统强化：15%

## 最小化主机操作系统的大小(减少攻击面)

## 最小化IAM角色

## 最小化对网络的外部访问

## 适当使用内核强化工具，如AppArmor, seccomp



# 微服务漏洞最小化：20%


## 设置适当的OS级安全域，例如使用PSP, OPA，安全上下文

## 管理Kubernetes机密

## 在多租户环境中使用容器运行时 (例如gvisor, kata容器)

## 使用mTLS实现Pod对Pod加密



# 供应链安全：20%

## 最小化基本镜像大小

## 保护您的供应链：将允许的注册表列入白名单，对镜像进行签名和验证

## 使用用户工作负载的静态分析(例如kubernetes资源，Docker文件)

## 扫描镜像，找出已知的漏洞



# 监控、日志记录和运行时安全：20%

## 在主机和容器级别执行系统调用进程和文件活动的行为分析，以检测恶意活动

## 检测物理基础架构，应用程序，网络，数据，用户和工作负载中的威胁

## 检测攻击的所有阶段，无论它发生在哪里，如何扩散

## 对环境中的不良行为者进行深入的分析调查和识别

## 确保容器在运行时不变

## 使用审计日志来监视访问
