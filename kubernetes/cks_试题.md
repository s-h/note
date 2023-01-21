<!-- TOC -->

- [1. Pod 指定 ServiceAccount](#1-pod-指定-serviceaccount)
- [2. RBAC - RoleBinding](#2-rbac---rolebinding)
- [3. 启用 API server 认证](#3-启用-api-server-认证)
- [4. sysdig & falco](#4-sysdig--falco)
- [5. 容器安全，删除特权 Pod (1.25废弃)](#5-容器安全删除特权-pod-125废弃)
- [6. 沙箱运行容器 gVisor](#6-沙箱运行容器-gvisor)
- [7. Pod 安全策略-PSP (1.25废弃)](#7-pod-安全策略-psp-125废弃)
- [8. 创建 Secret](#8-创建-secret)
- [9. AppArmor](#9-apparmor)
- [10. kube-bench 修复不安全项](#10-kube-bench-修复不安全项)
- [11. 网络策略 NetworkPolicy](#11-网络策略-networkpolicy)
- [12. Dockerfile 检测](#12-dockerfile-检测)
- [13. ImagePolicyWebhook 容器镜像扫描](#13-imagepolicywebhook-容器镜像扫描)
- [14. Trivy 扫描镜像安全漏洞](#14-trivy-扫描镜像安全漏洞)
- [15. 默认网络策略](#15-默认网络策略)
- [16. 日志审计 log audit](#16-日志审计-log-audit)
- [17. pod安全](#17-pod安全)
- [18. tls安全](#18-tls安全)

<!-- /TOC -->



# 1. Pod 指定 ServiceAccount
1. 在现有 namespace qa 中创建一个名为 backend-sa 的新 ServiceAccount， 确保此
ServiceAccount 不自动挂载 API 凭据。 
2. 使用 /cks/sa/pod1.yaml 中的清单文件来创建一个 Pod。
3. 最后，清理 namespace qa 中任何未使用的 ServiceAccount。


搜索关键字 configure service account

    ---
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: qa
    automountServiceAccountToken: false
    ---

    创建service account
    $ kubectl apply -f sa.yaml  
    查看已创建的sa
    $ kubectl get sa backend-sa -n qa -o yaml

    # 修改/cks/sa/pod1.yaml中serviceAccountName为backend
    $ kubectl apply -f /cks/sa/pod1.yaml

    # 查看新创建的pod
    $ kubectl get pod -n qa backend -o yaml

    # 查看所有serviceaccount
    $ kubectl get sa -n qa
    # 查看除backend-sa、defaultd的其他serviceaccount


# 2. RBAC - RoleBinding
Context 绑定到 Pod 的 ServiceAccount 的 Role 授予过度宽松的权限。完成以下项目以减少权限集。 
Task:
1. 一个名为 web-pod 的现有 Pod 已在 namespace db 中运行。 编辑绑定到 Pod 的 ServiceAccount
service-account-web 的现有 Role， 仅允许只对 services 类型的资源 执行 get 操作。 
2. 在namespace db 中创建一个名为 role-2 ，并仅允许只对 namespaces 类型的资源执行 delete 操作的新
Role。
3. 创建一个名为 role-2-binding 的新 RoleBinding，将新创建的 Role 绑定到 Pod 的
ServiceAccount。 注意：请勿删除现有的 RoleBinding。


    1.
    # 查看web-pod对应的serviceaccount
    $ kubectl -n db get pod web-pod -o yaml |grep service
    # 查看ns db下的role
    $ kubectl -n db get role
    $ kubectl -n db get role role1 -o yaml
    # 此时role内容为空，搜索rbac, 编辑查询到的role
    $ kubectl -n db edit role role1
    rules:
    - apiGroups: [""] # "" 标明 core API 组
    resources: ["services"]
    verbs: ["get"]
    
    2.
    kubectl -n db create role role-2 --verb='delete' --resource='namespaces'

    3.
    kubectl -n db create rolebinding role-2-binding --role='role-2' --serviceaccount="db:service-account-web"


# 3. 启用 API server 认证
Context 由 kubeadm 创建的 cluster 的 Kubernetes API 服务器，出于测试目的，
临时配置允许未经身份验证和未经授权的访问，授予匿名用户 cluster-admin 的访问权限. 

Task: 
1.重新配置 cluster 的Kubernetes APl 服务器，以确保只允许经过身份验证和授权的 REST 请求。 使用授权模式 Node,RBAC 和准入控制器 NodeRestriction。 

2.删除用户 system:anonymous 的 ClusterRoleBinding 来进行清理。

注意：所有 kubectl 配置环境/文件也被配置使用未经身份验证和未经授权的访问。 你不必更改它，但请注意，一旦完成 cluster
的安全加固， kubectl 的配置将无法工作。 您可以使用位于 cluster 的 master 节点上，cluster 原本的
kubectl 配置文件 /etc/kubernetes/admin.conf ，以确保经过身份验证的授权的请求仍然被允许。

    2. （这个题先做第二步）
    $ kubectl get clusterrolebinding system:anonymous
    $ kubectl delete clusterrolebinding system:anonymous
    $ kubectl get clusterrolebinding system:anonymous
    1.
    编辑/etc/kubernetes/manifests/kube-apiserver.yaml
    - --authorization-mode=Node,RBAC
    - --enable-admission-plugins=NodeRestriction   (需要将AlwaysAdmit删除)
    - --anonymous-auth=true   #这行要删除，或者true改成false


# 4. sysdig & falco
Task： 使用运行时检测工具来检测 Pod tomcat 单个容器中频发生成和执行的异常进程。 有两种工具可供使用： sysdig、 falco 
(注： 这些工具只预装在 cluster 的工作节点，不在 master 节点。 **此处需要跳到node节点**)
使用工具至少分析 30 秒 ，使用过滤器检查生成和执行的进程，将事件写到 /opt/KSR00101/incidents/summary 文 件中，
其中包含检测的事件， 格式如下： [timestamp],[uid],[processName] 保持工具的原始时间戳格式不变。 

注：确保事件文件存储在集群的工作节点上。


    形式一：k8s集群使用containerd
    #本题可选用sysdig或者falco，解题使用sysdig
    # 打印sysdig可用参数
    $ sysdig -l 
    # 查看CRI 
    $ crictl info |grep sock
    # crontainerd要是用crictl命令找到容器
    $ crictl ps |grep tomcatxxx
    $ sysdig -M 30 -p '%evt.time,%user.uid,%proc.name' --cri=/run/containerd/containerd.sock container.name=backend >> xxx
    $ sysdig -M 30 -p '%evt.time,%user.name,%proc.name' --cri=/run/containerd/containerd.sock container.name=backend >> xxx
    # 如果考试的时候sysdig报错“Unable to load the driver”
    $ sysdig-probe-loader


    形式二：k8s集群使用docker
    #本题可选用sysdig或者falco，解题使用sysdig
    # 打印sysdig可用参数
    $ sysdig -l 
    # 查找对应docker的id
    $ docker ps |grep tomcatxxx
    $ sysdig -M 30 -p '%evt.time,%user.uid,%proc.name' container.id=xxx >> xxx
    $ sysdig -M 30 -p '%evt.time,%user.name,%proc.name' container.id=xxx >> xxx

# 5. 容器安全，删除特权 Pod (1.25废弃)
Task: 检查在 namespace production 中运行的 Pod，并删除任何非无状态或非不可变的 Pod。
使用以下对无状态和不可变的严格解释： 1 能够在容器内存储数据的 Pod 的容器必须被视为非无状态的。

注意：你不必担心数据是否实际上已经存储在容器中。 2 被配置为任何形式的特权 Pod 必须被视为可能是非无状态和非不可变的。

    $ kubectl -n production get pod
    $ kubectl -n production get pod podname -o yaml |grep -Ei 'hostpath|privileged: true'
    $ kubectl -n production delete pod podname
    
# 6. 沙箱运行容器 gVisor
Context 该 cluster 使用 containerd 作为 CRI 运行时。containerd 的默认运行时处理程序是
runc。 containerd 已准备好支持额外的运行时处理程序 runsc (gVisor)。 

Task: 使用名为runsc的现有运行时处理程序，创建一个名为 untrusted 的 RuntimeClass。 更新 namespace server 中的所有
Pod 以在 gVisor 上运行。 

您可以在 /cks/gVisor/rc.yaml 中找到一个模版清单

    # 搜索runtimeclass 编辑
    # RuntimeClass 定义于 node.k8s.io API 组

    apiVersion: node.k8s.io/v1
    kind: RuntimeClass
    metadata:
      name: untrusted
    handler: runsc

    $ kubectl apply -f /cks/gVisor/rc.yaml
    
    # 查看ns server中的pod
    $ kubectl -n server get deployment,pod
    
    # 修改所有pod对应的deployment
    $ kubectl -n server edit deployment xxx
    在spec.spec下面添加：(此处需确认)
      runtimeClassName: untrusted

# 7. Pod 安全策略-PSP (1.25废弃)
Context PodSecurityPolicy 应防在特定 namespace 中特权 Pod 的创建。 

Task: 
1. 创建一个名为restrict-policy 的新的 PodSecurityPolicy，以防止特权 Pod 的创建。 
2. 创建一个名为 restrict-access-role 并使用新创建的 PodSecurityPolicy restrict-policy 的 ClusterRole。
3. 在现有的 namespace staging 中创建一个名为 psp-denial-sa 的新ServiceAccount 。
4. 最后，创建一个名为 dany-access-bind 的 ClusterRoleBinding，将新创建的 ClusterRole restrict-access-role 绑定到新创建的 ServiceAccount psp-denial-sa。 

你可以在一下位置找到模版清单文件： /cks/psp/psp.yaml

    # 搜索podsecuritypolicy，版本切换为1.24
    0. 
    启动psp准入控制器（考试中默认已经启用，但以防万一，还是要检查一下）
    $ cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep PodSecurityPolicycat restrict-access-role
    确保
    - --enable-admission-plugins=NodeRestriction,PodSercurityPolicy
    
    1.
    编辑psp.yaml

    apiVersion: policy/v1beta1
    kind: PodSecurityPolicy
    metadata:
    name: restrict-policy
    spec:
    privileged: false  # 不允许提权的 Pod！
    # 以下内容负责填充一些必需字段。
    seLinux:
        rule: RunAsAny
    supplementalGroups:
        rule: RunAsAny
    runAsUser:
        rule: RunAsAny
    fsGroup:
        rule: RunAsAny
    volumes:
    - '*'

    $ kubectl apply -f psp.yaml
    $ kubectl get psp

    2. $ kubectl create clusterrole restrict-access-role --resource='psp' --resource-name='restrict-policy' --verb='use'

    3. $ kubectl create -n staging sa psp-denial-sa

    4. $ kubectl create clusterrolebinding dany-access-bind --clusterrole='restrict-access-role' --serviceaccount='staging:psp-denial-sa'

# 8. 创建 Secret
Task:

1. 在 namespace istio-system 中获取名为 db1-test 的现有 secret 的内容

将 username 字段存储在名为 /cks/sec/user.txt 的文件中，并将 password 字段存储在名为 /cks/sec/pass.txt 的文件中。
注意：你必须创建以上两个文件，他们还不存在。 

注意：不要在以下步骤中使用/修改先前创建的文件，如果需要，可以创建新的临时文件。

2. 在 istio-system namespace 中创建一个名为 db2-test 的新 secret，内容如下： 

username : production-instance 

password : KvLftKgs4aVH 

3. 最后，创建一个新的 Pod，它可以通过卷访问 secret db2-test ：

Pod名称: secret-pod 

Namespace: istio-system 

容器名: dev-container 

镜像: nginx 

卷名: secret-volume 

挂载路径: /etc/secret

    1. 
    $ kubectl -n istio-system get secret db1-test -o yaml
    $ echo xxx | base64 -d > /cks/sec/user.txt
    $ echo xxx | base64 -d > /cks/sec/pass.txt

    2.
    $ kubectl -n isotio-system create secret generic db2-test --from-literal=username=production-instance --from-literal=password=KvLftKgs4aVH 

    3. 搜索secret

    apiVersion: v1
    kind: Pod
    metadata:
      name: secret-pod
      namespace: isotio-system
    spec:
      containers:
      - name: dev-container
        image: nginx
        volumeMounts:
        - name: secret-volume
          mountPath: "/etc/secret"
      volumes:
      - name: secret-volume
        secret:
          secretName: db2-test

# 9. AppArmor
Context APPArmor 已在 cluster 的工作节点上被启用。一个 APPArmor 配置文件已存在，但尚未被实施 

Task: 在 cluster 的工作节点上，实施位于 /etc/apparmor.d/nginx_apparmor 的现有 APPArmor
配置文件。 编辑位于 /home/candidate/KSSH00401/nginx-deploy.yaml 的现有清单文件以应用
AppArmor 配置文件。 最后，应用清单文件并创建其中指定的 Pod 。

    # /etc/apparmor.d/nginx_apparmor 查看profile名字
    $ apparmor_parser nginx_apparmor
    # 编辑/home/candidate/KSSH00401/nginx-deploy.yaml
    metadata:
        annotations:
          container.apparmor.security.beta.kubernetes.io/nginx: localhost/nginx-profile-3
                                                        containrer名字      profile名字
    $ kubectl apply -f  /home/candidate/KSSH00401/nginx-deploy.yaml
    # 可以通过检查该配置文件的 proc attr 来验证容器是否实际使用该配置文件运行
    $ kubectl exec nginx -- cat /proc/1/attr/current
    # 如果我们尝试通过写入文件来违反配置文件会发生什么
    $ kubectl exec nginx -- touch /tmp/test                                   

# 10. kube-bench 修复不安全项
Context 针对 kubeadm 创建的 cluster 运行 CIS 基准测试工具时， 发现了多个必须立即解决的问题。 

Task:
通过配置修复所有问题并重新启动受影响的组件以确保新的设置生效。 

修复针对 API 服务器发现的所有以下违规行为： 
1.2.7 Ensure that the --authorization-mode argument is not set to AlwaysAllow 

1.2.8 Ensure that the --authorization-mode argument includes Node 

1.2.9 Ensure that the --authorization-mode argument includes RBAC 

1.2.18 Ensure that the --insecure-bind-address argument is not set 

1.2.19 Ensure that the --insecure-port argument is set to 0 


FAIL FAIL 修复针对 kubelet 发现的所有以下违规行为： 
Fix all of the following violations that were found against the kubelet: 

4.2.1 Ensure that the anonymous-auth argument is set to false 

4.2.2 Ensure that the –authorization-mode argument is not set to AlwaysAllow 


注意：尽可能使用 Webhook 身份验证/授权。 

修复针对 etcd 发现的所有以下违规行为： 
Fix all of the following violations that were found against etcd: 

2.2 Ensure that the –client-cert-auth argument is set to true 

    mkdir ~/bak
    cp /etc/kubernetes/manifestes/kube-apiserver.yaml ~/bak/
    cp /etc/kubernetes/manifestes/etcd.yaml ~/bak/
    cp /var/lib/kubelet/config.yaml ~/bak/
    
    编辑 /etc/kubernetes/manifestes/kube-apiserver.yaml
    修改 --authorization-mode=Node,RBAC
    删除 --insecure-bind-address
    删除 --insecure-port

    编辑 /etc/kubernetes/manifestes/etcd.yaml
    修改 --client-cert-auth=true

    编辑 /var/lib/kubelet/config.yaml
    authentication:
      anonymous:
        enabled: false

    authrizathion:
      mode: Webhook

    $ systemctl daemon-reload
    $ systemctl restart kubelet
    $ kubectl get node
    $ kubectl get pod -A

# 11. 网络策略 NetworkPolicy
Task
 创建一个名为 pod-restriction 的 NetworkPolicy 来限制对在 namespace dev-team 中运行的 Pod products-service 的访问。 
 只允许以下 Pod 连接到 Pod products-service :
1 namespace qa 中的 Pod 
2 位于任何 namespace，带有标签 environment: testing 的 Pod

注意：确保应用 NetworkPolicy。 你可以在/cks/net/po.yaml 找到一个模板清单文件。


    $ kubectl -n dev-team get pod --show-labels
    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
      name: pod-restriction
      namespace: dev-team
    spec:
      podSelector:
        matchLabels:
          environment: testing
      policyTypes:
        - Ingress
      ingress:
        - from:
            - namespaceSelector:
                matchLabels:
                  name: qa
        - from:
            - namespaceSelector: {}
              podSelector:
                matchLabels:
                  environment: testing 

# 12. Dockerfile 检测
Task 分析和编辑给定的 Dockerfile /cks/docker/Dockerfile（基于 ubuntu:16.04 镜像）， 并修复在文件中拥有的突出的安全/最佳实践问题的两个指令。 
分析和编辑给定的清单文件 /cks/docker/deployment.yaml ， 并修复在文件中拥有突出的安全/最佳实践问题的两个字段。

注意：请勿添加或删除配置设置；只需修改现有的配置设置让以上两个配置设置都不再有安全/最佳实践问题。
注意：如果您需要非特权用户来执行任何项目，请使用用户 ID 65535 的用户 nobody 。 

答题： 注意，本次的 Dockerfile 和 deployment.yaml 仅修改即可，无需部署。

    Dockerfile：
    latest修改为16.04
    CMD前面的USER root修改为nobody

    deployment:
    删除 'SYS_ADMIN' 字段，确保 'privileged': 为 False 。
    template.metadata.run改为app， 添加version: stable

# 13. ImagePolicyWebhook 容器镜像扫描
Context cluster 上设置了容器镜像扫描器，但尚未完全集成到 cluster 的配置中。
完成后，容器镜像扫描器应扫描并拒绝易受攻击的镜像的使用。 

Task 注意：你必须在 cluster 的 master 节点上完成整个考题，所有服务和文件都已被准备好并放置在该节点上。 
给定一个目录 /etc/kubernetes/epconfig 中不完整的配置以及具有 HTTPS 端点 https://acme.local:8082/image_policy 的功能性容器镜像扫描器：

+ 启用必要的插件来创建镜像策略 
+ 校验控制配置并将其更改为隐式拒绝（implicit deny） 
+ 编辑配置以正确指向提供的 HTTPS 端点 最后，通过尝试部署易受攻击的资源 /cks/img/web1.yaml 来测试配置是否有效。 

你可以在/var/log/imagepolicy/roadrunner.log 找到容器镜像扫描仪的日志文件。

    编辑/etc/kubernetes/epconfig/admission_configuration.json
    "defaultAllow": false

    编辑/etc/kubernetes/epconfig/kubeconfig.yml
    添加cluster.server: https://acme.local:8082/image_policy

    搜索imagepocliywebhook
    编辑/etc/kubernetes/manifests/kube-apiserver.yaml
    - --enable-admission-plugins=...,ImagePolicyWebhook
    - --admission-control-config-file=/etc/kubernetes/epconfig/dmission_configuration.json

    - hostPath:
    path: /etc/kubernetes/epconfig/
    type: DirectoryOrCreate
    name: imagepolicy

    - mountPath: /etc/kubernetes/epconfig/
    name: imagepolicy
    readOnly: true

    $ kubect apply -f  /cks/img/web1.yaml 


# 14. Trivy 扫描镜像安全漏洞
Task 使用 Trivy 开源容器扫描器检测 namespace kamino 中 Pod 使用的具有严重漏洞的镜像。 查找具有 High 或 Critical 严重性漏洞的镜像，并删除使用这些镜像的 Pod。 

注意：Trivy 仅安装在 cluster 的 master 节点上， 在工作节点上不可使用。 你必须切换到 cluster 的 master 节点才能使用 Trivy

    $ kubectl -n kamino get pod 
    $ kubectl -n kamino get pod xxx -o yaml |grep image:
    $ trivy image -s HIGH,CRITICAL nginx:1.18.0
    $ kubeclt -n kamino delete pod xxx

# 15. 默认网络策略
Context 一个默认拒绝（default-deny）的 NetworkPolicy 可避免在未定义任何其他 NetworkPolicy 的 namespace 中 意外公开 Pod。 

Task
为所有类型为 Ingress+Egress 的流量在 namespace testing 中创建一个名为 denypolicy 的新默认拒绝 NetworkPolicy。 
此新的 NetworkPolicy 必须拒绝 namespace testing 中的所有的 Ingress + Egress 流量。 
将新创建的默认拒绝 NetworkPolicy 应用与在 namespace testing 中运行的所有 Pod。 你可以在 /cks/net/p1.yaml 找到一个模板清单文件。

    kind: NetworkPolicy
    metadata:
      name: denypolicy
      namespace: testing
    spec:
      podSelector: {}
      policyTypes:
        - Ingress
        - Egress

    $ kubetctl -n testing describe networkpolicy denypolicy 
    # 如果不要求Ingress，不写Ingress即可

# 16. 日志审计 log audit
Task 在 cluster 中启用审计日志。为此，请启用日志后端，并确保：
+ 日志存储在 */var/log/kubernetes/audit-logs.txt*
+ 日志文件能保留 *10* 天
+ 最多保留 *2* 个旧审计日志文件

/etc/kubernetes/logpolicy/sample-policy.yaml 提供了基本策略。它仅指定不记录的内容。
注意：基本策略位于 cluster 的 master 节点上。 

编辑和扩展基本策略以记录：
+ RequestResponse 级别的 *cronjobs* 更改
+ namespace *front-apps* 中 *deployment* 更改的请求体
+ *Metadata* 级别的所有 namespace 中的 ConfigMap 和 Secret 的更改 
此外，添加一个全方位的规则以在 *Metadata* 级别记录所有其他请求。 
注意：不要忘记应用修改后的策略


    编辑/etc/kubernetes/logpolicy/sample-policy.yaml

    kind: Policy
    omitStages:
      - "RequestReceived"
    rules:
      - level: RequestResponse
        resources:
        - group: ""
          resources: ["cronjobs"]

      - level: Metadata
        resources:
        - group: ""
          resources: ["secrets", "configmaps"]

      - level: Request
        resources:
        - group: "" # core API 组
          resources: ["deployment"]
        namespaces: ["front-apps"]

      - level: Metadata
        omitStages:
          - "RequestReceived"

    cp /etc/kubernets/manifests/kube-apiserver.yaml ~/bak
    $ vi /etc/kubernets/manifests/kube-apiserver.yaml

    - --audit-log-path=/var/log/kubernetes/audit-logs.txt
    - --audit-log-maxage=10
    - --audit-log-maxbackup=2
    - --audit-policy-file=/etc/kubernetes/logpolicy/sample-policy.yaml

    volumes:
    - hostPath:
        path: /etc/kubernetes/logpolicy
        type: DirectoryOrCreate
      name: audit
    - hostPath:
        path: /var/log/kubernetes
        type: DirectoryOrCreate
      name: audit-log


    volumeMounts:
    - mountPath: /etc/kubernetes/logpolicy
      name: audit
      readOnly: true
    - mountPath: /var/log/kubernets
      name: audit-log
      readOnly: false

# 17. pod安全
task：
修改运行在namespace webapp，名为web-deployment的现有deployment，使其容器：
+ 使用用户ID 20000运行
+ 使用一个只读根文件系统
+ 禁止特权提升

    每个容器增加：
    securityContext:
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true

    template.spec下增加或修改：
    securityContext:
      runAsUser: 30000


# 18. tls安全
task：
修改API服务器和etcd之间通信的TLS配置。

1.对于API服务器，删除对除*TLS 1.3*及更高版本之外的所有TLS版本的支持。此外，删除堆除*TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256*之外的所有cipher suites的支持。
确保应用配置的更改

2.对于etcd，删除对除TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256之外的所有chpher suites的支持
确保应用配置的更改

    关键字kube-apiserver
    修改kube-api配置，/etc/kubernetes/manifests/kube-apiserver.yaml
    - --tls-min-version=VersionTLS13
    - --tls-cipher-suites=TLS_AES_128_GCM_SHA256

    修改etcd配置，/etc/kubernetes/manifests/etcd.yaml
    - --cipher-suites=TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256