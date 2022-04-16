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


