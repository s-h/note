<!-- TOC -->

- [1. CronJob-1](#1-cronjob-1)
- [2. CronJob-2](#2-cronjob-2)
- [3. Dockerfile](#3-dockerfile)
- [4. 限制 CPU 和内存 - 1](#4-限制-cpu-和内存---1)
- [5. 限制 CPU 和内存 - 1](#5-限制-cpu-和内存---1)
- [6. 运行旧版应用程序](#6-运行旧版应用程序)
- [7. 金丝雀部署](#7-金丝雀部署)
- [8. 配置 Container 安全上下文](#8-配置-container-安全上下文)
- [9. 创建 Deployment 并指定环境变量](#9-创建-deployment-并指定环境变量)
- [10. RBAC 授权](#10-rbac-授权)
- [11. ConfigMap](#11-configmap)
- [12. Secret](#12-secret)
- [13. Pod 健康检查 livenessProbe](#13-pod-健康检查-livenessprobe)
- [14. Pod 健康检查 readinessProbe](#14-pod-健康检查-readinessprobe)

<!-- /TOC -->
# 1. CronJob-1
Task
1、创建一个名为 ppi 并执行一个运行以下单一容器的 Pod 的 CronJob：
- name: pi
 image: perl:5
 command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(2000)"]
配置 CronJob 为：
+ 每隔 5 分钟执行一次
+ 保留 2 个已完成的 Job
+ 保留 4 个失败的 Job
+ 永不重启 Pod
+ 在 8 秒后终止 Pod
2、为测试目的，从 CronJob ppi 中手动创建并执行一个名为 ppi-test 的 Job。

    $ vim cronjob.yaml
    apiVersion: batch/v1
    kind: CronJob
    metadata:
    name: ppi
    spec:
    schedule: "*/5 * * * *"
    successfulJobsHistoryLimit: 2
    failedJobsHistoryLimit: 4
    jobTemplate:
        spec:
        activeDeadlineSeconds: 8
        template:
            spec:
            containers:
            - name: pi
                image: perl:5
                imagePullPolicy: IfNotPresent
                command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(2000)"]
            restartPolicy: Never
    $ kubectl apply -f cronjob.yaml
    $ kubectl get cronjob
    $ kubectl create job --from=cronjob/ppi
    $ kubectl get jobs

# 2. CronJob-2
Contex
您需要创建一个将在预定时间运行的 Pod。
Task
1、在清单文件 /ckad/CKAD00016/periodic.yaml 中定义此 Pod
2、在一个 busybox:stable 容器中运行命令 date ，该命令必须每分钟运行一次，并且必须在 10 秒内完成运行，或者被 Kubernetes 终止运行。
 注意：CronJob 名称和容器名称都必须为 hello
3、在上述清单文件中创建此资源，并验证此 Job 至少成功执行一次。

    $ vim /ckad/CKAD00016/periodic.yaml
    apiVersion: batch/v1
    kind: CronJob
    metadata:
    name: hello
    spec:
    schedule: "*/1 * * * *"
    jobTemplate:
        spec:
        template:
            spec:
            activeDeadlineSeconds: 10
            containers:
            - name: hello
                image: busybox:stable
                imagePullPolicy: IfNotPresent
                command:
                - /bin/sh
                - -c
                - date;
            restartPolicy: Never
    $ kubectl apply -f /ckad/CKAD00016/periodic.yaml
    $ kubectl get cronjob
    $ kubectl get jobs

# 3. Dockerfile
task: 一个 Dockerfile 已经存在于 /ckad/DF/Dockerfile
1、使用已存在的 Dockerfile，构建一个名为 centos 和标签为 8.2 的容器镜像。
您可以安装和使用您选择的工具。
> 原本的系统中预装了多个符合OCI标准的镜像构建器和工具，包括：docker、skopeo、buildah、img和podman
2.使用您选择的工具，以 OCI 格式导出构建的容器镜像，并将其存储在 /ckad/DF/centos-8.2.tar

    $ cd /ckad/DF/
    $ sudo docker build -t centos:8.2 .
    $ sudo docker image ps |grep 8.2
    $ sudo docker save centos:8.2 > centos-8.2.tar

# 4. 限制 CPU 和内存 - 1
task:在现有的 namespace pod-resources 中创建一个名为 nginx-resources 的 Pod。用 nginx:1.16 的镜像来指定一个容器.
为其容器指定 40m 的 CPU 和 50Mi 的内存的资源请求

    $ vim pod.yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: nginx-resources
    namespace: pod-resources
    spec:
    containers:
    - name: nginx-resources
        image: nginx:1.16
        resources:
        requests:
            memory: "40Mi"
            cpu: "50m"
    $ kubectl apply -f pod.yaml

# 5. 限制 CPU 和内存 - 1
Task:
haddock namespace 中名为 nosql 的 Deployment 的 Pod 因其容器已用完资源而无法启动。
请更新 nosql Deployment，使 Pod：
+ 为其容器请求 15Mi 的内存
+ 将内存限制为 haddock namespace 设置的最大内存容量的一半。
您可以在 /ckad/chief-cardinal/nosql.yaml 找到 nosql Deployment 的配置清单。

    $ kubectl describe ns haddock 
    # 查看max最大限制
    $ kubectl -n haddock edit deployments.apps nosql
    resources: #没有则追加下面几行，如果考试时，它已经提前给你了，只是数字不对，则修改相应数字，比如改成 15Mi 和 20Mi。
      requests:
        memory: "15Mi"
      limits:
        memory: "20Mi"  #根据namespace实际填写

# 6. 运行旧版应用程序
Task
1、修复清单文件 /ckad/credible-mite/www.yaml 中的任何 API 弃用问题，以便可以将应用程序部署在 k8s cluster 上。
 注意：该应用程序是为 Kubernetes v1.15 开发的。
 k8s cluster 运行着 Kubernetes v1.25
2、请在 garfish namespace 中部署更新后的清单文件/ckad/credible-mite/www.yaml 中指定的应用程序。

    $ vim /ckad/credible-mite/www.yaml
    api 版本修改为 apps/v1
    并在 spec 下添加或修改 selector 标签，这个标签要和下面的 template 里的标签一致
    apiVersion: apps/v1

    selector:
      matchLabels:
        app: nginx

# 7. 金丝雀部署
为了测试新的应用程序发布，您需要准备一个金丝雀部署。
Task:
namespace goshawk 中名为 chipmunk-service 的 Service 指向名为 current-chipmunk-deployment 的 Deployment 创建的 5 个 Pod。
你可以在/ckad/goshawk/中找到 current-chipmunk-deployment 的清单文件。
1、在同一 namespace 中创建一个相同的 Deployment，名为 canary-chipmunk-deployment
2、修改 Deployment，以便：
+ 在 namespace goshawk 中运行的 Pod 的最大数量为 10 个
+ chipmunk.service 流量的 40%流向 Pod canary-chipmunk-deployment

    $ cp /ckad/goshawk/current-chipmunk-deployment.yaml bak.yaml
    $ vi /ckad/goshawk/current-chipmunk-deployment.yaml
    metadata:
    name: canary-chipmunk-deployment #这个根据题目要求修改
    namespace: goshawk
    spec:
    replicas: 1 #这里也先修改为 1
    selector:
    matchLabels:
    app: canary-chipmunk-deployment
    run: dep-svc #确保 current-chipmunk-deployment 和 canary-chipmunk-deployment 都有这个公用的标签。
    template:
    metadata:
    labels:
    app: canary-chipmunk-deployment
    run: dep-svc #确保 current-chipmunk-deployment 和 canary-chipmunk-deployment 都有这个公用的标签

    $ kubectl apply -f /ckad/goshawk/current-chipmunk-deployment.yaml
    $ kubectl scale deployment current-chipmunk-deployment --replicas=6 -n goshawk
    $ kubectl scale deployment canary-chipmunk-deployment --replicas=4 -n goshawk
    # 注意，如果考试时，有可能考将 20%流量给金丝雀版本 Pod，那就是原先为 8 个，金丝雀为 2 个
    $ kubectl get pod -n goshawk

# 8. 配置 Container 安全上下文
Task
修改运行在 namespace quetzal 名为 broker-deployment 的现有 Deployment，使其容器
+ 以用户 30000 运行
+ 禁止特权提升。
您可以在/ckad/daring-moccasin/broker-deployment.yaml 找到 broker-deployment 的清单文件。（模拟环境无此文件，做题也不需要此文件）

    $ kubectl -n quetzal edit deployments.apps broker-deployment
    .spec.template.spec.containers下增加：
    securityContext:
      allowPrivilegeEscalation: false
      runAsUser: 30000

# 9. 创建 Deployment 并指定环境变量
在现有的 namespace ckad00014 中创建一个运行 6 个 Pod 副本，名为 api 的 Deployment。用 nginx:1.16 的镜像来指定一个容器。
将名为 NGINX_PORT 且值为 8000 的环境变量添加到容器中，然后公开端口 80

    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: api
    namespace: ckad00014
    labels:
        app: nginx
    spec:
    replicas: 6
    selector:
        matchLabels:
        app: nginx
    template:
        metadata:
        labels:
            app: nginx
        spec:
        containers:
        - name: api
            image: nginx:1.16
            ports:
            - containerPort: 80
            env:
            - name: NGINX_PORT 
            value: '8000'

# 10. RBAC 授权
Task
在名为 honeybee-deployment 的 Deployment 和 namespace gorilla 中的一个 Pod 正在记录错误.
1 查看日志以识别错误消息。
找出错误，包括 User “system:serviceaccount:gorilla:default ”cannot list resource “serviceaccounts” […] in the namespace “gorilla”
2 更新 Deployment honeybee-deployment 以解决 Pod 日志中的错误。
您可以在/ckad/prompt-escargot/honeybee-deployment.yaml 中找到 honeybee-deployment 的清单文件


    1、通过 logs 打印错误日志
    $ kubectl -n gorilla get pod
    $ kubectl -n gorilla logs honeybee-deployment-***

    2、检查现有的 serviceaccount
    $ kubectl -n gorilla get sa

    查看 rolebinding 和 role 的绑定
    $ kubectl -n gorilla get rolebinding
    通过 rolebinding，查看 role 和 serviceaccount 的绑定
    $ kubectl -n gorilla describe rolebinding
    检查 role，寻找有 get 或 list 权限的 role
    $ kubectl -n gorilla describe role

    3、设置 honeybee-deployment 的 serviceaccount
    $ kubectl -n gorilla set serviceaccount deployments honeybee-deployment gorilla-sa
    等 2 分钟，会自动生成一个新 pod，再次检查，不报错了
    $ kubectl -n gorilla logs honeybee-deployment-***

# 11. ConfigMap
Task
1 在 namespace default 中创建一个名为 some-config 并存储着以下键/值对的 Configmap：
key3: value4
2 在 namespace default 中创建一个名为 nginx-configmap 的 Pod。用 nginx:stable 的镜像来指定一个容器。
用存储在 Configmap some-config 中的数据来填充卷，并将其安装在路径/some/path

    $ kubectl create configmap some-config --from-literal=key3=value4
    $ vim pod.yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: nginx-configmap
    spec:
    containers:
        - name: nginx-configmap
        image: nginx:stable
        volumeMounts:
        - name: config
            mountPath: "/some/path"
    volumes:
    - name: config
        configMap:
        name: some-config
    
    $ kubectl apply -f pod.yaml


# 12. Secret
Task
1、在 namespace default 中创建一个名为 another-secret 并包含以下单个键/值对的 Secret：
key1: valuel2
2、在 namespace default 中创建一个名为 nginx-secret 的 Pod。用 nginx:1.16 的镜像来指定一个容器。
添加一个名为 COOL_VARIABLE 的环境变量来使用 secret 键 key1 的值。

    $ kubectl create secret  generic another-secret --from-literal=key1=valuel2
    $ vim pod.yaml
    apiVersion: v1
    kind: Pod
    metadata:
    name: nginx-secret
    spec:
    containers:
    - name: nginx-secret
        image: nginx:1.16
        env:
        - name: COOL_VARIABLE
            valueFrom:
            secretKeyRef:
                name: another-secret
                key: key1
                optional: false # 此值为默认值；意味着 "mysecret"
                                # 必须存在且包含名为 "username" 的主键

# 13. Pod 健康检查 livenessProbe
Task
由于 Liveness Probe 发生了问题，您无法访问一个应用程序。该应用程序可能在任何 namespace 中运行。
1 找出对应的 Pod 并将其名称和 namespace 写入文件 /ckad/CKAD00011/broken.txt 。使用以下格式：
<namespaceName>/<podName>
文件/ckad/CKAD00011/broken.txt 已存在
2 用 kubectl get events 来获取相关错误事件井将其写入文件 /ckad/CKAD00011/error.txt 。请使用输出格式 wide 。
文件/ckad/CKAD00011/error.txt 已存在。
3 修复故障的 Pod 的 Liveness Probe 问题。

    1、
    检查集群 dk8s 下的所有命令空间里的 Pods，找出 Liveness probe 问题的 Pod。
    kubectl get pods -A
    对所有 namespace 下的 pod 逐一检查，考试中会有 5 个 pod 需要你检查。
    kubectl describe pod probe-demo -n probe-ns |tail    

    echo probe-ns/probe-demo >/ckad/CKAD00011/broken.txt
    2、
    kubectl get events -n probe-ns -o wide |grep probe-demo > /ckad/CKAD00011/error.txt 3、
    kubectl get pods probe-demo -n probe-ns -o yaml > probe.yaml
    cp probe.yaml bak-probe.yaml
    kubectl delete -f probe.yaml

    3.vi probe.yaml
    #修改livenessProbe:字段下的port为8443
    kubectl apply -f probe.yaml
    检查
    kubectl describe pod probe-demo -n probe-ns |tail


# 14. Pod 健康检查 readinessProbe

修改现有的 deployment probe-http ，增加 readinessProbe 探测器，规格如下：
使用 httpGet 进行探测
探测路径为 /healthz/return200
探测端口为 80
在执行第一次探测前应该等待 15 秒
执行探测的时间间隔为 20 秒

    $ kubectl edit deployment probe-http
    deployment.spec.template.spec.containers
    livenessProbe:
        httpGet:
          path: /healthz/return200
          port: 80
        initialDelaySeconds: 15
        periodSeconds: 20
