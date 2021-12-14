# nacos
## 安装
下载地址https://github.com/alibaba/nacos/releases

    wget https://github.com/alibaba/nacos/releases/download/1.4.0/nacos-server-1.4.0.tar.gz
    tar zxvf nacos-server-1.4.0.tar.gz

## 配置

    cd nacos/conf
    修改application.properties一下配置
    
    ### If user MySQL as datasource:
    spring.datasource.platform=mysql
    ### Count of DB:
    db.num=1
    ### Connect URL of DB:
    db.url.0=jdbc:mysql://127.0.0.1:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
    db.user=dbuser
    db.password=dbpassword

    修改cluster.conf，修改为本机ip

##  数据库初始化

    sql> create database nacos character set utf8 collate utf8_bin; 

    mysql -uroot -p < nacos/conf/nacos-mysql.sql

##  启动服务

    ./nacos/bin/startup.sh

