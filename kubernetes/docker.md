<!-- TOC -->

- [docker](#docker)
    - [安装](#安装)
        - [centos](#centos)
    - [基本命令](#基本命令)
        - [使用pull命令下载镜像](#使用pull命令下载镜像)
        - [使用image命令列出镜像目录](#使用image命令列出镜像目录)
        - [run创建容器](#run创建容器)
        - [ps命令查看](#ps命令查看)
        - [start启动容器](#start启动容器)
        - [stop停止容器](#stop停止容器)
        - [exec从外部运行容器内命令](#exec从外部运行容器内命令)
        - [rm删除容器](#rm删除容器)
        - [rmi删除镜像](#rmi删除镜像)
        - [查看容器进程](#查看容器进程)
            - [查看容器详情](#查看容器详情)
            - [查看统计信息](#查看统计信息)
        - [查看端口映射](#查看端口映射)
        - [查看日志](#查看日志)
        - [添加开机启动](#添加开机启动)
        - [cp拷贝文件](#cp拷贝文件)
    - [Dockerfile](#dockerfile)
        - [编写Dockerfile](#编写dockerfile)
            - [使用build命令创建镜像](#使用build命令创建镜像)
        - [使用history查看镜像历史](#使用history查看镜像历史)
        - [使用commit命令从容器的修改中创建镜像](#使用commit命令从容器的修改中创建镜像)
        - [使用diff命令检查容器文件的修改](#使用diff命令检查容器文件的修改)
    - [docker数据卷](#docker数据卷)
    - [msyql](#msyql)
        - [查看可用选项完整列表](#查看可用选项完整列表)
        - [官方mysql镜像作为客户端](#官方mysql镜像作为客户端)
        - [创建mysql容器](#创建mysql容器)
    - [zookeeper](#zookeeper)
    - [rabbitMQ 集群](#rabbitmq-集群)
        - [创建两个mq节点](#创建两个mq节点)
        - [同步两个节点erlang](#同步两个节点erlang)
        - [添加集群节点](#添加集群节点)
        - [添加用户](#添加用户)
    - [java](#java)
    - [redis](#redis)

<!-- /TOC -->
## docker
### 安装
#### centos

    yum install  yum-utils device-mapper-persistent-data lvm2
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    yum install docker-ce

### 基本命令
#### 使用pull命令下载镜像
#### 使用image命令列出镜像目录
#### run创建容器
命令格式 run <选项> <镜像名称> <要运行的文件>

    docker -i -t -d --name myos ubuntu /bin/bash

> -i、-t 选项可以在运行的bash中输入输出
> -d 后台运行
> -e 环境变量 如 -e PATH=$PATH:/opt/bin/
#### ps命令查看
> -a 列出全部
#### start启动容器
#### stop停止容器
#### exec从外部运行容器内命令

    docker exec -it xxx /bin/bash

#### rm删除容器
#### rmi删除镜像
#### 查看容器进程

    docker top <容器名称>

##### 查看容器详情

    docker container inspect <容器名称>

##### 查看统计信息
cpu、内存、存储、网络统计信息
   
    docker stats <容器名称>

#### 查看端口映射

    docker container port <容器名称>

#### 查看日志

    docker logs -f -t --tail=1000 xxx

#### 添加开机启动

    docker update --restart=always container_id

#### cp拷贝文件
拷贝容器时区文件

    docker cp  /usr/share/zoneinfo/Asia/Shanghai 容器ID:/etc/localtime

### Dockerfile
#### 编写Dockerfile
> FROM: 指定基础镜像
> MAINTAINER: 维护者信息
> RUN: 运行shell脚本或者命令
> VLUME: 与主机共享的目录，run时不指定该目录系统也会自动挂载
> WORKDIR: 为CMD中设置可执行文件的目录，包括run时运行命令的路径
> EXPOSE: 与主机相连的端口号
##### 使用build命令创建镜像
在保存Dockerfile的目录中执行

    docker build <选项> <Dockerfile路径>
    docker build --tag xx:xx .

#### 使用history查看镜像历史
#### 使用commit命令从容器的修改中创建镜像

     docker commit <选项> <容器名称> <镜像名称>:<标签>
     docker commit -a "Foo Bar <foo@bar.com>" -m "add" hello-nginx nginx:0.2

#### 使用diff命令检查容器文件的修改
### docker数据卷
-mount 使用数据卷，支持三种类型的数据卷：
> volume: 普通数据卷，映射到主机/var/lib/docker/volumes路径下
> bind: 绑定数据卷， 映射到主机指定路径下
> tmpfs: 临时数据卷，只存在内存中

    docker run -d -P --name web --mount type=bind,source=/webapp,destination=/opt/webbapp <镜像名> python app.py

上述命令等于旧的-v标记

     -v <主机目录>:<容器目录>
     docker run -d -P --name web -v /webapp:/opt/webapp <镜像名> python app.py

### msyql
#### 查看可用选项完整列表

    docker run -it --rm mysql --verbose --help

#### 官方mysql镜像作为客户端

    docker run -it --rm mysql mysql -hsome.mysql.host -usome-mysql user -p

#### 创建mysql容器

    docker run -p 3306:3306 --name mysql -e  TZ='Asia/Shanghai' \
    -v /data/docker/mysql/conf.d:/etc/mysql/conf.d \
    -v /data/docker/mysql/logs:/var/log/mysql \
    -v /data/docker/mysql/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=youpassword \
    -d mysql:5.7.28 
    # MySQL(5.7.19)的默认配置文件是 /etc/mysql/my.cnf 文件。如果想要自定义配置，建议向 /etc/mysql/conf.d 目录中创建 .cnf 文件。新建的文件可以任意起名，只要保证后缀名是 cnf 即可。新建的文件中的配置项可以覆盖 /etc/mysql/my.cnf 中的配置项。

### zookeeper

    docker network create --driver bridge --subnet=172.15.0.0/16 --gateway=172.15.1.1 netgroup

    docker run -d -p 2181:2181 --name zk1 --privileged --restart always --network netgroup --ip 172.15.0.10 -v /opt/zook/zk1/data:/data -v /opt/zook/zk1/datalog:/datalog -v /opt/zook/zk1/logs:/logs -e ZOO_MY_ID=1 -e "ZOO_SERVERS=server.1=172.15.0.10:2888:3888;2181 server.2=172.15.0.11:2888:3888;2181 server.3=172.15.0.12:2888:3888;2181" -e "ZOO_4LW_COMMANDS_WHITELIST=*" zookeeper 

    docker run -d -p 2182:2181 --name zk2 --privileged --restart always --network netgroup --ip 172.15.0.11 -v /opt/zook/zk2/data:/data -v /opt/zook/zk2/datalog:/datalog -v /opt/zook/zk2/logs:/logs -e ZOO_MY_ID=2 -e "ZOO_SERVERS=server.1=172.15.0.10:2888:3888;2181 server.2=172.15.0.11:2888:3888;2181 server.3=172.15.0.12:2888:3888;2181" -e "ZOO_4LW_COMMANDS_WHITELIST=*" zookeeper

    docker run -d -p 2183:2181 --name zk3 --privileged --restart always --network netgroup --ip 172.15.0.12 -v /opt/zook/zk3/data:/data -v /opt/zook/zk3/datalog:/datalog -v /opt/zook/zk3/logs:/logs -e ZOO_MY_ID=3 -e "ZOO_SERVERS=server.1=172.15.0.10:2888:3888;2181 server.2=172.15.0.11:2888:3888;2181 server.3=172.15.0.12:2888:3888;2181" -e "ZOO_4LW_COMMANDS_WHITELIST=*" zookeeper

### rabbitMQ 集群
#### 创建两个mq节点

    docker run -i -d --name mq1 --hostname mq1 --add-host=mq2:172.18.0.6 -p 5672:5672 -p 15672:15672 rabbitmq:3.8.9-management rabbitmq-server
    docker run -i -d --name mq2 --hostname mq2 --add-host=mq1:172.18.0.5 -p 5673:5672 -p 15673:15672 rabbitmq:3.8.9-management rabbitmq-server

#### 同步两个节点erlang

    docker cp mq1:/var/lib/rabbitmq/.erlang.cookie .
    docker cp .erlang.cookie mq2:/var/lib/rabbitmq/

#### 添加集群节点
在mq2节点操作

    rabbitmqctl stop_app
    rabbitmqctl join_cluster rabbit@mq1
    rabbitmqctl start_app
    rabbitmqctl cluster_status

#### 添加用户

    rabbitmqctl add_user admin password
    rabbitmqctl set_user_tags admin administrator

### java
使用Dockerfile创建镜像

    # cat Dockerfile
    FROM openjdk:14
    EXPOSE 8888
    COPY demo-0.0.1-SNAPSHOT.jar /opt/demo-0.0.1-SNAPSHOT.jar
    WORKDIR /opt/
    ENTRYPOINT ["java", "-jar", "demo-0.0.1-SNAPSHOT.jar"]
    #  docker run -it -d -p 8888:8888 --name myapp myapp:1.0

### redis

    mkdir -p /data/docker/redis/conf/
    mkdir -p /data/docker/redis/logs/
    touch /data/docker/redis/conf/redis.conf
    touch /data/docker/redis/logs/redis.log

    docker run -it -d --name redis -p 6379:6379 -v /data/docker/redis/conf/redis.conf:/etc/redis/redis.conf -v /data/docker/redis/logs/redis.log:/var/log/redis.log -v  /data/docker/redis/data/:/data redis:5.0.7 redis-server /etc/redis/redis.conf --appendonly yes