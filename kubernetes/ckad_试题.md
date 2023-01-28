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

# 运行旧版应用程序
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

# 金丝雀部署
为了测试新的应用程序发布，您需要准备一个金丝雀部署。
Task:
namespace goshawk 中名为 chipmunk-service 的 Service 指向名为 current-chipmunk-deployment 的 Deployment 创建的 5 个 Pod。
你可以在/ckad/goshawk/中找到 current-chipmunk-deployment 的清单文件。
1、在同一 namespace 中创建一个相同的 Deployment，名为 canary-chipmunk-deployment
2、修改 Deployment，以便：
+ 在 namespace goshawk 中运行的 Pod 的最大数量为 10 个
+ chipmunk.service 流量的 40%流向 Pod canary-chipmunk-deployment