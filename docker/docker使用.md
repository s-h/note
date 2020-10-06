#### 基本命令
##### 使用pull命令下载镜像
##### 使用image命令列出镜像目录
##### run创建容器
命令格式 run <选项> <镜像名称> <要运行的文件>

    docker -i -t --name myos ubuntu /bin/bash

> -i、-t 选项可以在运行的bash中输入输出
##### ps命令查看
