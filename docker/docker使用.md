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


### Dockerfile
#### 编写Dockerfile
> FROM: 指定基础镜像
> MAINTAINER: 维护者信息
> RUN: 运行shell脚本或者命令
> VLUME: 与主机共享的目录
> WORKDIR: 为CMD中设置可执行文件的目录
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

### zookeeper

    docker network create --driver bridge --subnet=172.15.0.0/16 --gateway=172.15.1.1 netgroup

    docker run -d -p 2181:2181 --name zk1 --privileged --restart always --network netgroup --ip 172.15.0.10 -v /opt/zook/zk1/data:/data -v /opt/zook/zk1/datalog:/datalog -v /opt/zook/zk1/logs:/logs -e ZOO_MY_ID=1 -e "ZOO_SERVERS=server.1=172.15.0.10:2888:3888;2181 server.2=172.15.0.11:2888:3888;2181 server.3=172.15.0.12:2888:3888;2181" -e "ZOO_4LW_COMMANDS_WHITELIST=*" zookeeper 

    docker run -d -p 2182:2181 --name zk2 --privileged --restart always --network netgroup --ip 172.15.0.11 -v /opt/zook/zk2/data:/data -v /opt/zook/zk2/datalog:/datalog -v /opt/zook/zk2/logs:/logs -e ZOO_MY_ID=2 -e "ZOO_SERVERS=server.1=172.15.0.10:2888:3888;2181 server.2=172.15.0.11:2888:3888;2181 server.3=172.15.0.12:2888:3888;2181" -e "ZOO_4LW_COMMANDS_WHITELIST=*" zookeeper

    docker run -d -p 2183:2181 --name zk3 --privileged --restart always --network netgroup --ip 172.15.0.12 -v /opt/zook/zk3/data:/data -v /opt/zook/zk3/datalog:/datalog -v /opt/zook/zk3/logs:/logs -e ZOO_MY_ID=3 -e "ZOO_SERVERS=server.1=172.15.0.10:2888:3888;2181 server.2=172.15.0.11:2888:3888;2181 server.3=172.15.0.12:2888:3888;2181" -e "ZOO_4LW_COMMANDS_WHITELIST=*" zookeeper
    