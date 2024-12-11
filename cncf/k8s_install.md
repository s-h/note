<!-- vscode-markdown-toc -->
* 1. [环境](#)
* 2. [前期准备](#-1)
	* 2.1. [源配置](#-1)
		* 2.1.1. [apt源](#apt)
		* 2.1.2. [docker-ce](#docker-ce)
		* 2.1.3. [kubernetes](#kubernetes)
		* 2.1.4. [docker镜像](#docker)
* 3. [master节点安装](#master)
	* 3.1. [查看组件状态](#-1)
	* 3.2. [忽略报错](#-1)
* 4. [node节点安装](#node)
* 5. [网络插件](#-1)
	* 5.1. [flannel](#flannel)
	* 5.2. [weave](#weave)
* 6. [ingress安装](#ingress)
	* 6.1. [nginx ingress](#nginxingress)
* 7. [使用 Helm 安装 NFS-client Provisioner](#HelmNFS-clientProvisioner)
	* 7.1. [添加 Helm 仓库](#Helm)
	* 7.2. [安装 NFS-client Provisioner](#NFS-clientProvisioner)
	* 7.3. [验证安装](#-1)
* 8. [命令补全](#-1)
* 9. [报错排查](#-1)
	* 9.1. [unknown service runtime.v1alpha2.RuntimeService](#unknownserviceruntime.v1alpha2.RuntimeService)
	* 9.2. [\"systemd\" is different from docker cgroup driver: \"cgroupfs\""](#systemdisdifferentfromdockercgroupdriver:cgroupfs)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->
##  1. <a name=''></a>环境
+ os: Ubuntu 20.04 LTS
+ master: 192.168.162.71
+ node01: 192.168.162.72
+ node02: 192.168.162.73
+ pod-network: 10.244.0.0/16
+ service: 10.96.0.0/16

##  2. <a name='-1'></a>前期准备
###  2.1. <a name='-1'></a>源配置
全部节点
####  2.1.1. <a name='apt'></a>apt源
   /etc/apt/sources.list 替换默认的 http://archive.ubuntu.com/ 为 mirrors.aliyun.com
####  2.1.2. <a name='docker-ce'></a>docker-ce

    # step 1: 安装必要的一些系统工具
    sudo apt-get update
    sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
    # step 2: 安装GPG证书
    curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
    # Step 3: 写入软件源信息
    sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
    # Step 4: 更新并安装Docker-CE
    sudo apt-get -y update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

####  2.1.3. <a name='kubernetes'></a>kubernetes

    apt-get update && apt-get install -y apt-transport-https
    curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add -
    cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
    deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
    EOF
    apt-get update
    apt-get install -y kubelet kubeadm kubectl

####  2.1.4. <a name='docker'></a>docker镜像

    #使用阿里云docker镜像加速服务
    sudo mkdir -p /etc/docker
    sudo tee /etc/docker/daemon.json <<-'EOF'
    {
      "registry-mirrors": ["https://xxxx.mirror.aliyuncs.com"]
    }
    EOF
    sudo systemctl daemon-reload
    sudo systemctl restart docker

##  3. <a name='master'></a>master节点安装

    apt install kubelet kubectl kubeadm
    kubeadm init --kubernetes-version=v1.18.3 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/12 --image-repository=registry.aliyuncs.com/google_containers


    #To start using your cluster, you need to run the following as a regular user:

    #mkdir -p $HOME/.kube
    #sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    #sudo chown $(id -u):$(id -g) $HOME/.kube/config

    export KUBECONFIG=/etc/kubernetes/admin.conf


###  3.1. <a name='-1'></a>查看组件状态

    kubectl get cs

###  3.2. <a name='-1'></a>忽略报错
--ignore-preflight-errors strings   忽略前置检查错误，被忽略的错误将被显示为警告. 

例子: 'IsPrivilegedUser,Swap,NumCPU'. Value 'all' ignores errors from all checks.

##  4. <a name='node'></a>node节点安装

    apt install kubelet kubectl kubeadm

    kubeadm join 192.168.162.71:6443 --token v9yil9.29mxuw3x8r0yj56t \
    --discovery-token-ca-cert-hash sha256:4b930bdffc81c3f301979c2ecc2a4167555799e36f897f015c4192eaf4d41ed1

##  5. <a name='-1'></a>网络插件
###  5.1. <a name='flannel'></a>flannel
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

安装flannel插件
docker pull registry.aliyuncs.com/google_containers/flannel:v1.18.2-amd64
###  5.2. <a name='weave'></a>weave

    kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
    kubectl get pods --all-namespaces

##  6. <a name='ingress'></a>ingress安装
###  6.1. <a name='nginxingress'></a>nginx ingress
https://github.com/kubernetes/ingress-nginx/blob/main/README.md#readme

[快速安装](https://kubernetes.github.io/ingress-nginx/deploy/)

##  7. <a name='HelmNFS-clientProvisioner'></a>使用 Helm 安装 NFS-client Provisioner
###  7.1. <a name='Helm'></a>添加 Helm 仓库
```bash
helm repo add rainbond https://openchart.goodrain.com/goodrain/rainbond
helm repo update
```
###  7.2. <a name='NFS-clientProvisioner'></a>安装 NFS-client Provisioner
```bash
helm install nfs-client-provisioner rainbond/nfs-client-provisioner \
  --set nfs.server=YOUR_NFS_SERVER_IP \
  --set nfs.path=/YOUR/NFS/PATH \
  --version 1.2.8
```
###  7.3. <a name='-1'></a>验证安装
```bash
: # 安装完成后，可以检查Pod的状态以确保其正确运行
kubectl get pods -l app=nfs-client-provisioner

: # 当你使用 Helm 安装 nfs-client-provisioner 后，通常 Helm Chart 会自动为你创建一个或多个 StorageClass
kubectl get sc

: # 创建pvc进行验证
: # pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: test-pvc
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: managed-nfs-storage   # 填写实际的sc
  resources:
    requests:
      storage: 1Gi
```

##  8. <a name='-1'></a>命令补全
source <(kubectl completion bash)

##  9. <a name='-1'></a>报错排查
###  9.1. <a name='unknownserviceruntime.v1alpha2.RuntimeService'></a>unknown service runtime.v1alpha2.RuntimeService
版本：
+ OS Ubuntu 20.04.4 LTS 
+ docker docker-ce 20.10.15~3-0~ubuntu-focal
+ kubeadm 1.24.0
报错信息

    [preflight] Some fatal errors occurred:
	[ERROR CRI]: container runtime is not running: output: time="2022-05-07T06:51:48Z" level=fatal msg="getting status of runtime: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService"
    , error: exit status 1

解决：
https://github.com/kubernetes-sigs/cri-tools/issues/710

    rm /etc/containerd/config.toml
    systemctl restart containerd

###  9.2. <a name='systemdisdifferentfromdockercgroupdriver:cgroupfs'></a>\"systemd\" is different from docker cgroup driver: \"cgroupfs\""
版本:
+ OS Ubuntu 20.04.4 LTS
+ docker docker-c 5:20.10.21~3-0~ubuntu-focal
+ kubelet kubelet 1.23.14-00

报错信息：

    kubelet无法启动：
    "Failed to run kubelet" err="failed to run Kubelet: misconfiguration: kubelet cgroup driver: \"systemd\" is different from docker cgroup driver: \"cgroupfs\""

解决：
https://www.cnblogs.com/hellxz/p/kubelet-cgroup-driver-different-from-docker.html

    /etc/docker/daemon.json中，添加"exec-opts": ["native.cgroupdriver=systemd"]
    systemctl daemon-reload
    systemctl restart docker
