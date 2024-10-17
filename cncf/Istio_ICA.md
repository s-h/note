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
        - [LB算法](#lb算法)
        - [哈希一致性算法](#哈希一致性算法)
        - [连接池 connectionPool](#连接池-connectionpool)
        - [异常处理（熔断） outlierDetection](#异常处理熔断-outlierdetection)
    - [serviceEntry](#serviceentry)
    - [Sidecar](#sidecar)

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
禁用tls
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

### LB算法
默认k8s的service是轮询，istio的service是随机。可以指定转发算法。
```yaml
apiVersion: networking.istio.io/v1
kind: DestinationRule
metadata:
  name: bookinfo-ratings
spec:
  host: ratings.prod.svc.cluster.local
  trafficPolicy:
    loadBalancer:
      simple: LEAST_REQUEST
```
+ UNSPECIFIED 用户未指定负载均衡算法。Istio 将选择合适的默认值。
+ RANDOM 随机负载均衡选择一个随机健康主机。如果没有配置健康检查策略，随机负载均衡器的性能通常比轮询更好。
+ PASSTHROUGH 此选项会将连接转发到调用者请求的原始IP地址，而不进行任何形式的负载均衡。必须小心使用此选项，它适用于高级用例。
+ ROUND_ROBIN 一个基本的循环负载均衡平衡策略。这对于许多场景（例如，使用断电加权时）通常是不安全的，因为它会使端点负担过重。
+ LAST_REQUSULT 最少请求负载均衡器将负载分散到终端节点，优先支持未完成请求最少的终端节点。这通常更安全，并且在几乎所有情况下都优于 ROUND_ROBIN。 
+ LAST_CONN 已弃用

### 哈希一致性算法
https://istio.io/latest/zh/docs/reference/config/networking/destination-rule/#LoadBalancerSettings-ConsistentHahttp
```yaml
apiVersion: networking.istio.io/v1
kind: DestinationRule
metadata:
  name: bookinfo-ratings
spec:
  host: ratings.prod.svc.cluster.local
  trafficPolicy:
    loadBalancer:
      consistentHash:
        httpCookie:
          name: user
          ttl: 10s
```
cookie中含有name=user的cookie，则在ttl时间内转发到同一个pod
```bash
curl -H "Cookie: user=jianghao" http://ratings.prod.svc.cluster.local/ratings/1
```

### 连接池 connectionPool
**连接数** 客户端想服务端发请求，需要建立TCP连接，那么建立TCP连接的数量就是连接数
**并发连接数（SBC）** 每秒建立的TCP连接数
**请求数** 客户端建立连接后，先服务端发送GET/POST/HEAD数据包，服务器返回结果两种情况：
+ http数据包头有close字段，关闭连接
+ http数据包头有keep-live字段，本次连接不关闭，可以下一次继续发送请求，减少TCP连接

**并发请求数（QPS）** 每秒钟处理的请求数

定义连接池
https://istio.io/latest/zh/docs/reference/config/networking/destination-rule/#ConnectionPoolSettings-HTTPSettings
```yaml
apiVersion: networking.istio.io/v1
kind: DestinationRule
metadata:
  name: bookinfo-redis
spec:
  host: myredissrv.prod.svc.cluster.local
  trafficPolicy:
    connectionPool:
      tcp:
        maxConnections: 100   #最大TCP连接数
        connectTimeout: 30ms  #TCP连接超时，最小值需要大于1ms
        tcpKeepalive:
          time: 7200s
          interval: 75s
      http:
        http1MaxPendingRequests: 1024 #针对一个目标的HTTP请求最大排队数，默认1024
        http2MaxRequests: 1024 #对一个后端的最大请求数
        maxRequestsPerConnection:
        maxRetries: 3 #在给定的时间，集群所有主机最大重试数，默认值3
```

### 异常处理（熔断） outlierDetection
https://istio.io/latest/zh/docs/reference/config/networking/destination-rule/#OutlierDetection
```yaml
apiVersion: networking.istio.io/v1
kind: DestinationRule
metadata:
  name: reviews-cb-policy
spec:
  host: reviews.prod.svc.cluster.local
  trafficPolicy:
    connectionPool:
      tcp:
        maxConnections: 100
      http:
        http2MaxRequests: 1000
        maxRequestsPerConnection: 10
    outlierDetection:
      consecutive5xxErrors: 7
      interval: 5m               # 驱逐检查时间
      baseEjectionTime: 15m     # 指定来一个实例被踢掉以后，最少多长时间之后加回来，如果连续触发熔断，熔断的时长会乘以响应的倍数，时间默认为30秒
```

## serviceEntry

配置外部服务, 网格内访问网格外
```yaml
apiVersion: networking.istio.io/v1
kind: ServiceEntry
metadata:
  name: mysw
spec:
  hosts:
  - www.jianghao.tech
  ports:
  - number: 443
    name: https
    protocol: HTTPS
  location: MESH_EXTERNAL  # https://istio.io/latest/zh/docs/reference/config/networking/service-entry/#ServiceEntry-Location
  resolution: STATIC # https://istio.io/latest/zh/docs/reference/config/networking/service-entry/#ServiceEntry-Resolution
  endpoints:
  - address: 2.2.2.2
  - address: 3.3.3.3
---
apiVersion: networking.istio.io/v1
kind: VirtualService
metadata:
  name: my-virtual-service
spec:
  hosts:
    - www.jianghao.tech
  http:
  - route:
    - destination:
        host: www.jianghao.tech
  ```

## Sidecar
**Sidecar** 默认重置所有pod里的envoy设置，也可以设置影响特定的pod。 一个命名空间里只能有一个sidecar。