<!-- TOC -->

- [环境](#环境)
- [前期准备](#前期准备)
    - [源配置](#源配置)
        - [apt源](#apt源)
        - [docker-ce](#docker-ce)
        - [kubernetes](#kubernetes)
        - [docker镜像](#docker镜像)
- [master节点安装](#master节点安装)
    - [查看组件状态](#查看组件状态)
    - [忽略报错](#忽略报错)
- [node节点安装](#node节点安装)
- [网络插件](#网络插件)
    - [flannel](#flannel)
    - [weave](#weave)
- [ingress安装](#ingress安装)
    - [nginx ingress](#nginx-ingress)
- [命令补全](#命令补全)
- [报错排查](#报错排查)
    - [unknown service runtime.v1alpha2.RuntimeService](#unknown-service-runtimev1alpha2runtimeservice)
    - [\"systemd\" is different from docker cgroup driver: \"cgroupfs\""](#\systemd\-is-different-from-docker-cgroup-driver-\cgroupfs\)

<!-- /TOC -->
## 环境
+ os: Ubuntu 20.04 LTS
+ master: 192.168.162.71
+ node01: 192.168.162.72
+ node02: 192.168.162.73
+ pod-network: 10.244.0.0/16
+ service: 10.96.0.0/16

## 前期准备
### 源配置
全部节点
#### apt源
   /etc/apt/sources.list 替换默认的 http://archive.ubuntu.com/ 为 mirrors.aliyun.com
#### docker-ce

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

#### kubernetes

    apt-get update && apt-get install -y apt-transport-https
    curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add -
    cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
    deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
    EOF
    apt-get update
    apt-get install -y kubelet kubeadm kubectl

#### docker镜像

    #使用阿里云docker镜像加速服务
    sudo mkdir -p /etc/docker
    sudo tee /etc/docker/daemon.json <<-'EOF'
    {
      "registry-mirrors": ["https://xxxx.mirror.aliyuncs.com"]
    }
    EOF
    sudo systemctl daemon-reload
    sudo systemctl restart docker

## master节点安装

    apt install kubelet kubectl kubeadm
    kubeadm init --kubernetes-version=v1.18.3 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/12 --image-repository=registry.aliyuncs.com/google_containers


    #To start using your cluster, you need to run the following as a regular user:

    #mkdir -p $HOME/.kube
    #sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    #sudo chown $(id -u):$(id -g) $HOME/.kube/config

    export KUBECONFIG=/etc/kubernetes/admin.conf


### 查看组件状态

    kubectl get cs

### 忽略报错
--ignore-preflight-errors strings   忽略前置检查错误，被忽略的错误将被显示为警告. 

例子: 'IsPrivilegedUser,Swap,NumCPU'. Value 'all' ignores errors from all checks.

## node节点安装

    apt install kubelet kubectl kubeadm

    kubeadm join 192.168.162.71:6443 --token v9yil9.29mxuw3x8r0yj56t \
    --discovery-token-ca-cert-hash sha256:4b930bdffc81c3f301979c2ecc2a4167555799e36f897f015c4192eaf4d41ed1

## 网络插件
### flannel
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

安装flannel插件
docker pull registry.aliyuncs.com/google_containers/flannel:v1.18.2-amd64
### weave

    kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
    kubectl get pods --all-namespaces

## ingress安装
### nginx ingress
https://github.com/kubernetes/ingress-nginx/blob/main/README.md#readme

[快速安装](https://kubernetes.github.io/ingress-nginx/deploy/)

## 命令补全
source <(kubectl completion bash)

## 报错排查
### unknown service runtime.v1alpha2.RuntimeService
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

### \"systemd\" is different from docker cgroup driver: \"cgroupfs\""
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
