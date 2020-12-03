<!-- TOC -->

- [1. mysql](#1-mysql)
    - [1.1. 维护命令](#11-维护命令)
        - [1.1.1. 查看mysql版本](#111-查看mysql版本)
        - [1.1.2. 查看引擎](#112-查看引擎)
        - [1.1.3. 查看binlog](#113-查看binlog)
        - [1.1.4. 查看连接](#114-查看连接)
        - [1.1.5. 锁表](#115-锁表)
    - [1.2. 问题处理](#12-问题处理)
        - [1.2.1. 解决MySQL非聚合列未包含在GROUP BY子句报错问题](#121-解决mysql非聚合列未包含在group-by子句报错问题)
    - [1.3. mysqludmp](#13-mysqludmp)
    - [1.4. database](#14-database)
        - [1.4.1. 创建数据库](#141-创建数据库)
        - [1.4.2. 赋权](#142-赋权)
        - [1.4.3. 刷新权限](#143-刷新权限)
    - [1.5. 导入数据](#15-导入数据)
        - [1.5.1. 导入gz格式数据](#151-导入gz格式数据)

<!-- /TOC -->
# 1. mysql
## 1.1. 维护命令
### 1.1.1. 查看mysql版本

    SELECT version();

### 1.1.2. 查看引擎
suport字段为DEFAULT的为默认引擎

    show engines;

### 1.1.3. 查看binlog

    show variables like '%log_bin%';

### 1.1.4. 查看连接

    SELECT substring_index(host, ':',1) AS host_name,state,count(*) FROM information_schema.processlist GROUP BY state,host_name;

### 1.1.5. 锁表

    # 查看锁表 
    # 返回Table_locks_immediate结果，意思是表被锁了总数
    # Table_locks_waited表示有多少请求等待表锁
    SHOW STATUS LIKE 'Table_locks%';

    # InnoDB_row_lock行锁的争夺情况
    # show status like 'innodb_row_lock%';

## 1.2. 问题处理
### 1.2.1. 解决MySQL非聚合列未包含在GROUP BY子句报错问题
执行类似以下mysql查询，

    SELECT id, name, count(*) AS cnt FROM case_table GROUP BY name

报错，如下：

    服务器内部错误 (1055, "Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'case_table.id' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by")

解决，修改mysql配置，添加：

    sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION 

## 1.3. mysqludmp

## 1.4. database
### 1.4.1. 创建数据库

    create database dbname character set utf8 collate utf8_bin; 

### 1.4.2. 赋权

    # 指定数据库、用户、远程地址、密码
    grant all privileges on dbname.* to user_name@localhost identified by 'user_password'; 

    # 所有库、所有远程地址
    grant all privileges on *.* to user_name@'%' identified by 'user_password'; 

    GRANT：执行命令词，一般为动词
    ALL：赋予权限名，参考下面的权限列表
    *.*：前者表示数据库名，后者表示数据表名
    databasename.*：表示在databasename中的所有表
    databasename.tablename：表示在databasename中的tablename表
    'username'@'localhost'：前者为用户名，后者为接入的IP
    'username'@'%'：可以从任何地点接入
    'username'@'192.168.1.%' ：192.168.1 IP下的局域网都可接入
    'username'@'%.website.com'：可以从http://website.com接入
    'username'@'localhost': 只可以本机登录

### 1.4.3. 刷新权限

    FLUSH PRIVILEGES;

## 1.5. 导入数据
### 1.5.1. 导入gz格式数据
    zcat create.sql.gz | mysql -uroot -p zabbixdb
