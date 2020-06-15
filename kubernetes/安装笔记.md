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
    sudo apt-get -y install docker-ce

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

    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

### 查看组件状态

    kubectl get cs

## node节点安装

    apt install kubelet kubectl kubead

    kubeadm join 192.168.162.71:6443 --token v9yil9.29mxuw3x8r0yj56t \
    --discovery-token-ca-cert-hash sha256:4b930bdffc81c3f301979c2ecc2a4167555799e36f897f015c4192eaf4d41ed1



