<!-- TOC -->

- [1. 介绍](#1-介绍)
    - [1.1. MaxCompute介绍](#11-maxcompute介绍)
        - [1.1.1. 数据通道](#111-数据通道)
            - [1.1.1.1. 批量历史数据通道](#1111-批量历史数据通道)
            - [1.1.1.2. 试试增量数据通道](#1112-试试增量数据通道)
        - [1.1.2. 计算及分析任务](#112-计算及分析任务)
            - [1.1.2.1. SQL](#1121-sql)
            - [1.1.2.2. DUF（用户自定义函数）](#1122-duf用户自定义函数)
            - [1.1.2.3. MapReduce](#1123-mapreduce)
            - [1.1.2.4. Graph](#1124-graph)
        - [1.1.3. SDK](#113-sdk)
        - [1.1.4. 安全](#114-安全)
- [2. odps客户端（odps）安装](#2-odps客户端odps安装)
- [基本概念](#基本概念)
    - [分区](#分区)
        - [使用限制](#使用限制)
        - [分区的数据类型](#分区的数据类型)
        - [示例](#示例)
    - [资源](#资源)
    - [函数](#函数)

<!-- /TOC -->
# 1. 介绍
大数据计算服务MaxCompute（原名ODPS）是一种快速、完全托管的EB级数据仓库解决方案。
## 1.1. MaxCompute介绍
### 1.1.1. 数据通道
#### 1.1.1.1. 批量历史数据通道
**Tunnel**是MaxCompute提供数据传输服务。
#### 1.1.1.2. 试试增量数据通道
针对实时数据上传的场景，MaxCompute踢动**DataHub**服务，支持如Logstash、Flume、Fuentd、Sqoop等
### 1.1.2. 计算及分析任务
#### 1.1.2.1. SQL
MaxComput以表的形式存储数据，支持多种数据版本工具，提供SQL查询功能。
#### 1.1.2.2. DUF（用户自定义函数）
#### 1.1.2.3. MapReduce
#### 1.1.2.4. Graph
### 1.1.3. SDK
### 1.1.4. 安全
# 2. odps客户端（odps）安装
下载客户端进入conf文件夹，配置odps_config.ini文件

    project_name=访问目标MaxCompute项目名称
    access_id=
    access_key=
    end_point=
    log_view_host=
    https_check=是否开启HTTPS访问机制，对访问MaxCompute项目的请求进行加密，默认False
    # confirm threshold for query input size(unit: GB)
    data_size_confirm=输入数据量的最大值，单位为GB。取值范围无限制。推荐设置为100 GB。
    # this url is for odpscmd update
    update_url=
    # download sql results by instance tunnel
    use_instance_tunnel=
    # the max records when download sql results by instance tunnel
    instance_tunnel_max_record=
    # IMPORTANT:
    #   If leaving tunnel_endpoint untouched, console will try to automatically get one from odps service, which might charge networking fees in some cases.
    #   Please refer to https://help.aliyun.com/document_detail/34951.html
    # tunnel_endpoint=

    # use set.<key>=
    # e.g. set.odps.sql.select.output.format=

# 基本概念
## 分区
分区Partition是指一张表下，根据分区字段（一个或多个字段的组合）对数据存储进行划分。如果表没有分区，数据是直接放在表所在的目录下。如果表有分区，每个分区对应表下的一个目录，数据是分别存储在不同的分区目录下

MaxCompute将分区列的每个值作为一个分区（目录），可以指定多级分区，即将表的多个字段作为表的分区，分区之间类似多级目录的关系。

分区表的意义在于优化查询。查询表时通过WHERE子句查询指定所需查询的分区，避免全表扫描，提高处理效率，降低计算费用。使用数据时，如果指定需要访问的分区名称，则只会读取相应的分区

### 使用限制
+ 单表分区层级最多为6级。
+ 单表分区数最大值为60000个。
+ 单次查询允许查询最多的分区个数为10000个。
+ STRING分区类型的分区值不支持使用中文。

### 分区的数据类型
MaxCompute 2.0数据类型版本支持的分区字段为TINYINT、SMALLINT、INT、BIGINT、VARCHAR、STRING

### 示例

    ---创建表parttest。
    create table parttest (a bigint) partitioned by (pt bigint);
    ---向表中插入数据。
    insert into parttest partition(pt)(a,pt) values (1, 1);
    insert into parttest partition(pt)(a,pt) values (1, 10);
    ---查询表中字段pt大于等于2的行。
    select * from parttest where pt >= '2';

    --创建一个二级分区表，以日期为一级分区，地域为二级分区
    CREATE TABLE src (shop_name string, customer_id bigint) PARTITIONED BY (pt string,region string);

    --正确使用方式。MaxCompute在生成查询计划时只会将'20170601'分区下region为'hangzhou'二级分区的数据纳入输入中。
    select * from src where pt='20170601'and region='hangzhou'; 
    --错误的使用方式。在这样的使用方式下，MaxCompute并不能保障分区过滤机制的有效性。pt是STRING类型，当STRING类型与BIGINT（20170601）比较时，MaxCompute会将二者转换为DOUBLE类型，此时有可能会有精度损失。
    select * from src where pt = 20170601;

## 资源
资源（Resource）是MaxCompute的特有概念，如果您想使用MaxCompute的自定义函数（UDF）或MapReduce功能需要依赖资源来完成

## 函数
包含内建函数、自定义函数（UDF）

自定义函数（UDF）包含：标量值函数（UDF），自定义聚合函数（UDAF）和自定义表值函数（UDTF）