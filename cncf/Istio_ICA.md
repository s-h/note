# Istio介绍与安装
## 自动注入
### 命名空间指定标签
给命名空间添加标签，指示 Istio 在部署应用的时候，自动注入 Envoy Sidecar 代理
```bash
kubectl label namespace default istio-injection=enabled
```
### 有些命名空间不会自动注入
+ kube-system命名空间里的pod
+ 使用宿主机网络的pod不会自动注入（hostNetwork: true）
+ 手动给pod指定了一个标签sidecar.istio.io/inject: "false"

### 单独给pod进行注入
```bash
istioctl kube-inject -f pod.yaml > pod-injected.yaml; kubectl apply -f pod-injected.yaml
# 或者
istioctl kube-inject -f pod.yaml | kubectl apply -f - -n namespace
```


