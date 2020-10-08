#### 基本命令
##### 使用pull命令下载镜像
##### 使用image命令列出镜像目录
##### run创建容器
命令格式 run <选项> <镜像名称> <要运行的文件>

    docker -i -t -d --name myos ubuntu /bin/bash

> -i、-t 选项可以在运行的bash中输入输出
> -d 后台运行
> -e 环境变量 如 -e PATH=$PATH:/opt/bin/
##### ps命令查看
> -a 列出全部
##### start启动容器
##### stop停止容器
##### exec从外部运行容器内命令

    docker exec -it xxx /bin/bash

##### rm删除容器
##### rmi删除镜像

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
#### docker数据卷
-mount 使用数据卷，支持三种类型的数据卷：
> volume: 普通数据卷，映射到主机/var/lib/docker/volumes路径下
> bind: 绑定数据卷， 映射到主机指定路径下
> tmpfs: 临时数据卷，只存在内存中

    docker run -d -P --name web --mount type=bind,source=/webapp,destination=/opt/webbapp <镜像名> python app.py

上述命令等于旧的-v标记

     -v <主机目录>:<容器目录>
     docker run -d -P --name web -v /webapp:/opt/webapp <镜像名> python app.py
#### 查看容器进程

    docker top <容器名称>

##### 查看容器详情

    docker container inspect <容器名称>

##### 查看统计信息
cpu、内存、存储、网络统计信息
   
    docker stats <容器名称>

#### 查看端口映射

    docker container port <容器名称>


    
