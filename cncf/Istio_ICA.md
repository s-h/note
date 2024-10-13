<!-- TOC -->

- [Istio介绍与安装](#istio介绍与安装)
    - [自动注入](#自动注入)
        - [命名空间指定标签](#命名空间指定标签)
        - [有些命名空间不会自动注入](#有些命名空间不会自动注入)
        - [有些命名空间不会自动注入](#有些命名空间不会自动注入)
        - [单独给pod进行注入](#单独给pod进行注入)
- [istio配置管理](#istio配置管理)
- [istio配置管理](#istio配置管理)
    - [gateway](#gateway)
    - [virtualService](#virtualservice)
    - [DestinationRule](#destinationrule)
        - [流量发送到不同pod](#流量发送到不同pod)
        - [是否启用tls](#是否启用tls)
        - [是否启用tls](#是否启用tls)
        - [转发算法](#转发算法)

<!-- /TOC -->
# Istio介绍与安装
## 自动注入
### 命名空间指定标签
给命名空间添加标签，指示 Istio 在部署应用的时候，自动注入 Envoy Sidecar 代理
```bash
: #添加标签
kubectl label namespace default istio-injection=enabled
```
### 有些命名空间不会自动注入
+ kube-system命名空间里的pod
+ 使用宿主机网络的pod不会自动注入（hostNetwork: true）
+ 手动给pod指定了一个标签sidecar.istio.io/inject: "false"

### 单独给pod进行注入
```bash
istioctl kube-inject -f pod.yaml > pod-injected.yaml; kubectl apply -f pod-injected.yaml
: #或者
istioctl kube-inject -f pod.yaml | kubectl apply -f - -n namespace
```
# istio配置管理
## gateway
**网关**管理网格的入站和出站流量
+ istio-ingressagteway 入口网关
+ istio-egressgateway 出口网关
## virtualService
**VirtualService**及**虚拟服务**作用在svc上，可以指定路由规则，可以指定负载均衡策略。
## DestinationRule
**DestinationRule**及**目标规则**作用在svc和pod之间
### 流量发送到不同pod
```yaml
apiVersion: networking.istio.io/v1
kind: VirtualService
metadata:
  name: my-virtual-service
spec:
  hosts:
    - "*.jianghao.tech"
  http:
  - route:
    - destination:
        host: my-svc  # 两个destination转发到同一个svc，但是子集不同。和后面DR配置的子集对应
        subset: v1
      weight: 80
    - destination:
        host: my-svc
        subset: v2
      weight: 20
---
apiVersion: networking.istio.io/v1
kind: DestinationRule
metadata:
  name: my-destination-rule
spec:
  host: my-svc # 要作用的svc
  subsets:     # 定义两个子集
  - name: v1
    labels:     # pod的标签
      version: v1
  - name: v2
    labels:
      version: v2
```
### 是否启用tls
```yaml
apiVersion: networking.istio.io/v1
kind: DestinationRule
metadata:
  name: my-destination-rule
spec:
  host: my-svc # 要作用的svc
  trafficPolicy:
    tls:
      mode: DISABLE # 禁用; SIMPLE; MUTUAL; ISTIO_MUTUAL
```
+ DISABLE：不启用TLS
+ SIMPLE：启用TLS，但客户端不验证服务端证书
+ MUTUAL：启用TLS，并且客户端也验证服务端
+ ISTIO_MUTUAL：启用 mutual TLS，并且客户端也验证服务端。与Mutual模式相比，此模式使用Istio自动生成证书进行mTLS身份验证。

### 转发算法
默认k8s的service是轮询，istio的service是随机。可以指定转发算法。
