<!-- TOC -->

- [mysql](#mysql)
    - [维护命令](#维护命令)
        - [查看mysql版本](#查看mysql版本)
        - [查看引擎](#查看引擎)
        - [查看binlog](#查看binlog)
        - [binlog刷新时机](#binlog刷新时机)
        - [查看连接](#查看连接)
        - [查看正在执行的语句](#查看正在执行的语句)
        - [锁表](#锁表)
    - [问题处理](#问题处理)
        - [解决MySQL非聚合列未包含在GROUP BY子句报错问题](#解决mysql非聚合列未包含在group-by子句报错问题)
    - [索引](#索引)
        - [查看未使用索引语句](#查看未使用索引语句)
    - [mysqludmp](#mysqludmp)
    - [database](#database)
        - [创建数据库](#创建数据库)
        - [赋权](#赋权)
        - [刷新权限](#刷新权限)
    - [导入数据](#导入数据)
        - [导入gz格式数据](#导入gz格式数据)
    - [查询数据结构](#查询数据结构)
    - [查看数据大小](#查看数据大小)
    - [查看所有表大小](#查看所有表大小)

<!-- /TOC -->

# mysql

## 维护命令

### 查看mysql版本

    SELECT version();

### 查看引擎
suport字段为DEFAULT的为默认引擎

    show engines;

### 查看binlog

    # 是否开启binlog
    show variables like '%log_bin%';
    # binlog详细信息
    show global variables like '%log%';

### binlog刷新时机
如果设置为0，则表示MySQL不控制binlog的刷新，由文件系统去控制它缓存的刷新；
如果设置为不为0的值，则表示每 sync_binlog 次事务，MySQL调用文件系统的刷新操作刷新binlog到磁盘中。
设为1是最安全的，在系统故障时最多丢失一个事务的更新，但是会对性能有所影响。

### 查看连接

    SELECT substring_index(host, ':',1) AS host_name,state,count(*) FROM information_schema.processlist GROUP BY state,host_name;

### 查看正在执行的语句

    show full processlist;

### 锁表

    # 查看锁表 
    # 返回Table_locks_immediate结果，意思是表被锁了总数
    # Table_locks_waited表示有多少请求等待表锁
    SHOW STATUS LIKE 'Table_locks%';

    # InnoDB_row_lock行锁的争夺情况
    # show status like 'innodb_row_lock%';

## 问题处理

### 解决MySQL非聚合列未包含在GROUP BY子句报错问题
执行类似以下mysql查询，

    SELECT id, name, count(*) AS cnt FROM case_table GROUP BY name

报错，如下：

    服务器内部错误 (1055, "Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'case_table.id' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by")

解决，修改mysql配置，添加：

    sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION 
## 索引
### 查看未使用索引语句

    set @@global.log_queries_not_using_indexes=ON;

## mysqludmp

## database

### 创建数据库

    create database dbname character set utf8 collate utf8_bin; 

### 赋权

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

    # mysql 8.0
    CREATE USER 'user_name'@'%' IDENTIFIED BY 'user_password';
    grant all privileges on *.* TO 'user_name'@'%' WITH GRANT OPTION;

    ALTER USER 'user_name'@'%' IDENTIFIED WITH mysql_native_password BY 'user_password';

### 刷新权限

    FLUSH PRIVILEGES;

## 导入数据

### 导入gz格式数据
    zcat create.sql.gz | mysql -uroot -p zabbixdb

## 查询数据结构

    SELECT
        TABLE_SCHEMA 库名,
        TABLE_NAME 表名,
        COLUMN_NAME 列名,
        COLUMN_TYPE 数据类型,
        DATA_TYPE 字段类型,
        CHARACTER_MAXIMUM_LENGTH 长度,
        IS_NULLABLE 是否为空,
        COLUMN_DEFAULT 默认值,
        COLUMN_COMMENT 备注
    FROM
        INFORMATION_SCHEMA. COLUMNS 
    WHERE
        -- -- database_name为数据库名称，到时候只需要修改成你要导出表结构的数据库即可
        table_schema ='database_name'

## 查看数据大小

    SELECT
        table_schema AS '数据库',
        sum(table_rows) AS '记录数',
        sum(
            TRUNCATE (data_length / 1024 / 1024, 2)
        ) AS '数据容量(MB)',
        sum(
            TRUNCATE (index_length / 1024 / 1024, 2)
        ) AS '索引容量(MB)'
    FROM
        information_schema. TABLES
    GROUP BY
        table_schema
    ORDER BY
        sum(data_length) DESC,
        sum(index_length) DESC;

## 查看所有表大小

    SELECT
        table_schema AS '数据库',
        table_name AS '表名',
        table_rows AS '记录数',
        TRUNCATE (data_length / 1024 / 1024, 2) AS '数据容量(MB)',
        TRUNCATE (index_length / 1024 / 1024, 2) AS '索引容量(MB)'
    FROM
        information_schema. TABLES
    ORDER BY
        data_length DESC,
        index_length DESC;