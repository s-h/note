<!-- vscode-markdown-toc -->
* 1. [es6.8.23安装](#es6.8.23)
	* 1.1. [系统初始化](#)
	* 1.2. [配置elasticsearch](#elasticsearch)
	* 1.3. [生成证书](#-1)
	* 1.4. [开启服务](#-1)
* 2. [数据迁移](#-1)
	* 2.1. [reindex](#reindex)
	* 2.2. [snapshot](#snapshot)
		* 2.2.1. [配置nfs](#nfs)
		* 2.2.2. [设置挂载点](#-1)
		* 2.2.3. [设置仓库](#-1)
		* 2.2.4. [生成快照](#-1)
		* 2.2.5. [恢复快照](#-1)
* 3. [cluster管理](#cluster)
	* 3.1. [ 副本磁盘空间检查](#-1)
	* 3.2. [水位控制：](#-1)
	* 3.3. [处理副本异常](#-1)
	* 3.4. [查看集群默认配置](#-1)
* 4. [数据管理](#-1)
	* 4.1. [计算集群文档数量](#-1)
	* 4.2. [数据插入](#-1)
	* 4.3. [查询数据](#-1)
		* 4.3.1. [指标聚合](#-1)
	* 4.4. [删除数据](#-1)
* 5. [分片、索引管理](#-1)
	* 5.1. [移动分片](#-1)
	* 5.2. [修复索引只读](#-1)
* 6. [curl](#curl)
	* 6.1. [使用用户名密码访问](#-1)
* 7. [默认分片副本设置（6.x）](#6.x)
* 8. [ES故障排查](#ES)
	* 8.1. [集群状态](#-1)
	* 8.2. [查看集群状态](#-1)
	* 8.3. [查找异常索引](#-1)
		* 8.3.1. [查看索引并按照主分片大小排序](#-1)
	* 8.4. [计算doc](#doc)
	* 8.5. [查看异常信息](#-1)
	* 8.6. [查看分片状态](#-1)
		* 8.6.1. [统计节点主分片数量](#-1)
	* 8.7. [查看异常信息](#-1)
	* 8.8. [尝试重新分配](#-1)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->
##  1. <a name='es6.8.23'></a>es6.8.23安装
###  1.1. <a name=''></a>系统初始化
磁盘分区挂载 略
关闭swap 略
JDK安装 略

    echo 'echo 'vm.max_map_count = 262144' >> /etc/sysctl.conf'
    sysctl -p
    cat /etc/security/limits.d/20-nproc.conf

    * soft nofile 204800
    * hard nofile 204800
    * soft nproc 65535
    * hard nproc 65535
    * soft memlock unlimited
    * hard memlock unlimited

###  1.2. <a name='elasticsearch'></a>配置elasticsearch
解压elasticsearch

    tar zxvf elasticsearch-6.8.23.tar.gz; cd elasticsearch-6.8.23

配置使用内存

    vim config/jvm.options

配置es

    vim config/elasticsearch.yml
    # 集群名称，集群统一
    cluster.name: mycluster
    # 节点名称，节点唯一
    node.name: mynode01
    # 存储目录, 可以配置多个，节点之间存储目录数量、大小最好保持一致
    path.data: ["/data01", "/data02"]
    # 禁用swap
    bootstrap.memory_lock: true
    # 服务器ip
    network.host: 192.168.1.1
    # 集群所有节点ip , 集群主机数量应为单数防止脑裂
    discovery.zen.ping.unicast.hosts: ["192.168.1.1", "192.168.1.2", "192.168.1.3"]
    # 达到恢复任务节点数
    gateway.recover_after_nodes: 2
    # 集群最小数量
    discovery.zen.minimum_master_nodes: 2
    # 安全相关配置，启用密码, es6.8及7以后，xpack密码功能免费
    xpack.security.enabled: true
    xpack.ml.enabled: true
    xpack.license.self_generated.type: trial
    # 启用ssl传输安全性
    xpack.security.transport.ssl.enabled: true
    xpack.security.transport.ssl.verification_mode: certificate
    xpack.security.transport.ssl.keystore.path: certs/elastic-certificates.p12
    xpack.security.transport.ssl.truststore.path: certs/elastic-certificates.p12


###  1.3. <a name='-1'></a>生成证书

    mkdir config/certs; cd config/certs
    ../../bin/elasticsearch-certutil ca
    ../../bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12

###  1.4. <a name='-1'></a>开启服务

    cat /etc/systecat /etc/systemd/system/elasticsearch.service 

    [Unit]
    Description=elasticsearch
    After=network.target

    [Service]
    Type=simple
    User=elastic
    Group=elasitc
    LimitNOFILE=100000
    LimitNPROC=100000
    LimitMEMLOCK=infinity
    Restart=no
    ExecStart=/opt/elasticsearch/bin/elasticsearch
    PrivateTmp=true

    [Install]
    WantedBy=multi-user.target

    # 开启服务
    systemctl start elasticsearch
    systemctl enable elasticsearch

    # 生成密码 
    ./bin/elasticsearch-setup-passwords interactive


##  2. <a name='-1'></a>数据迁移
###  2.1. <a name='reindex'></a>reindex
新集群需要配置：
reindex.remote.whitelist: ["192.168.1.1:9200","192.168.21.2:9200","192.168.21.3:9200"]

    POST _reindex?wait_for_completion=false
    {
      "source": {
        "remote": {
          "host": "http://192.168.1.1:9200"
        },
        "index": "nginx-access-2019-04-17",
        "size": 6000
      },
      "dest": {
        "index": "nginx-access-2019-04-17"
      }
    }

wait_for_completion 将参数设置为false会执行一些预执行检查，启动请求，然后返回一个任务，改任务可以有api取消或查看

    GET _tasks?detailed=true&actions=*reindex
    POST _tasks/taskId1/_cancel

###  2.2. <a name='snapshot'></a>snapshot
快照适用于数据备份以及大量数据迁移
####  2.2.1. <a name='nfs'></a>配置nfs
服务端：

    apt-get install nfs-kernerl-server nfs-common
    #编辑配置文件sudo vi /etc/exports
    /es_snapshot x.x.x.x/24(rw,sync,all_squash)  
    chown nobody.nogroup /es_snapshot
    /etc/init.d/nfs-kernel-server restart

客户端：

    apt-get install nfs-common
    mount -t nfs x.x.x.x/es_snapshot /nfs_share

####  2.2.2. <a name='-1'></a>设置挂载点
所有节点挂载文件系统/nfs_share(名称自定义)
编辑elasticsearch.yml增加:

    path.repo: ["/nfs_share"]

####  2.2.3. <a name='-1'></a>设置仓库
创建名为my_bakcup的仓库，仓库的类型为共享文件系统，并设置挂载点。


    PUT _snapshot/my_backup/
    {
        "type": "fs",
        "settings": {
            "location": "/nfs_share/backups/my_backup"
        }
    }

修改仓库流量控制参数，必须在仓库未使用时配置：

    POST _snapshot/my_backup/
    {
        "type": "fs",
        "settings": {
            "location": "/nfs_share/backups/my_backup",
            "max_snapshot_bytes_per_sec" : "50mb",
            "max_restore_bytes_per_sec" : "50mb"
        }
    }
####  2.2.4. <a name='-1'></a>生成快照
生成快照并制定索引

    PUT _snapshot/my_backup/snapshot_2
    {
        "indices": "index_1,index_2"
    }

获取快照信息

    GET _snapshot/my_backup/snapshot_2
    GET _snapshot/my_backup/snapshot_2/_status
    GET _snapshot/my_backup/_all

删除快照

    DELETE _snapshot/my_backup/snapshot_2

####  2.2.5. <a name='-1'></a>恢复快照
恢复快照/指定索引

    POST _snapshot/my_backup/snapshot_1/_restore
    POST /_snapshot/my_backup/snapshot_1/_restore
    {
        "indices": "index_1",
        "rename_pattern": "index_(.+)",
        "rename_replacement": "restored_index_$1"
    }

监控恢复操作

    GET restored_index_3/_recovery

##  3. <a name='cluster'></a>cluster管理
###  3.1. <a name='-1'></a> 副本磁盘空间检查
超过85%(默认)将不在该节点分配副本，直接关闭

    GET _cluster/settings
    PUT _cluster/settings
    {
      "transient": {
      "cluster.routing.allocation.disk.threshold_enabled": false
      }
    }

###  3.2. <a name='-1'></a>水位控制：

```bash
    : #临时
    curl -H "Content-type: application/json" -XPUT x.x.x.x:9200/_cluster/settings -d '{
      "transient": {
        "cluster.routing.allocation.disk.watermark.low": "100gb",
        "cluster.routing.allocation.disk.watermark.high": "50gb",
        "cluster.routing.allocation.disk.watermark.flood_stage": "10gb"
      }
    }'

    : #持久化配置，重启依然生效
    curl -H "Content-type: application/json" -XPUT x.x.x.x:9200/_cluster/settings -d '{
      "persisitent": {
        "cluster.routing.allocation.disk.watermark.low": "100gb",
        "cluster.routing.allocation.disk.watermark.high": "50gb",
        "cluster.routing.allocation.disk.watermark.flood_stage": "10gb"
      }
    }'
```

###  3.3. <a name='-1'></a>处理副本异常

  GET _cluster/allocation/explain
  POST /_cluster/reroute?retry_failed=true

###  3.4. <a name='-1'></a>查看集群默认配置

  GET _cluster/settings?include_defaults=true&pretty


##  4. <a name='-1'></a>数据管理
###  4.1. <a name='-1'></a>计算集群文档数量

    GET _count?pretty
    {
        "query": {
            "match_all": {}
        }
    }

###  4.2. <a name='-1'></a>数据插入

    PUT /indexname/typename/id
    {
        "first_name" : "John",
        "last_name" :  "Smith",
        "age" :        25,
        "about" :      "I love to go rock climbing",
        "interests": [ "sports", "music" ]
    }

###  4.3. <a name='-1'></a>查询数据

    GET filebeat-6.2.4-2019.08.19/_search
    {
      "query": {
        "match": {
          "source": "/var/log/vmware-network.log"
        }
      }
    }

    使用curl数据查询

    curl  -H "Content-Type: application/json" -XGET http://x.x.x.x:9200/indexname/_search?pretty -d ' {"query":{"match_all":{}}}'


####  4.3.1. <a name='-1'></a>指标聚合
相当于msyql聚合函数

	max
    {
      "size": 0,
      "aggs": {
    	"max_id": {
    	  "max": {
    		"field": "id"
    	  }
    	}
      }
    }

	min
    {
      "size": 0,
      "aggs": {
    	"min_id": {
    	  "min": {
    		"field": "id"
    	  }
    	}
      }
    }

###  4.4. <a name='-1'></a>删除数据

    GET test/_delete_by_query
    {
        "query" : {
            "match" : {
                "first_name" : "jianghao"
            }
        }
    }

##  5. <a name='-1'></a>分片、索引管理
###  5.1. <a name='-1'></a>移动分片

    _cluster/reroute
    {
    "commands":[{
        "move":{
            "index":"indexName",
                "shard":0,
                "from_node":"node-1",
                "to_node":"node-4"
                }
                }]
    }

###  5.2. <a name='-1'></a>修复索引只读

```bash
    : #检查索引是否有只读
    GET _all/_settings |python -m json.tool |grep read_only 
    : #修改指定索引只读
    PUT apm*/_settings  
    {
        "index.blocks.read_only_allow_delete": null
    }
    : #修改全部索引只读
    PUT _all/_settings  
    {
        "index.blocks.read_only_allow_delete": null
    }
##  6. <a name='curl'></a>curl
###  6.1. <a name='-1'></a>使用用户名密码访问

    curl --user ops:user@password http://192.168.1.100:9200/_cat/

##  7. <a name='6.x'></a>默认分片副本设置（6.x）

    GET _template/default_template

    POST _template/default_template
    {
    "index_patterns": ["*"],
    "settings": {
        "number_of_shards": 3,
        "number_of_replicas": 1
    }
    }

##  8. <a name='ES'></a>ES故障排查
[Elasticsearch集群异常状态（RED、YELLOW）原因分析](https://cloud.tencent.com/developer/article/1803943?from_column=20421&from=20421)
###  8.1. <a name='-1'></a>集群状态
+ GREEN 主分片和副本分片都已分配，Elasticsearch集群是100%可用的
+ YELLOW 主分片可用，但是副本分片不可用, 但至少还有一个副本是未分配的。不会有数据丢失
+ RED 存在不可用的主分片。此时执行查询虽然部分数据仍然可以查到，但实际上已经影响到索引读写，需要重点关注。这种情况Elasticsearch集群至少一个主分片（以及它的全部副本）都在缺失中。这意味着索引已缺少数据，搜索只能返回部分数据，而分配到这个分片上的请求都返回异常

###  8.2. <a name='-1'></a>查看集群状态

    _cluster/health

返回以下信息：
重点看**集群状态**、**未分配的分片数**

+ cluster_name	集群的名称
+ status	集群的运行状况
+ timed_out	如果false响应在timeout参数指定的时间段内返回（30s默认情况下）
+ number_of_nodes	集群中的节点数
+ number_of_data_nodes	作为专用数据节点的节点数
+ active_primary_shards	活动主分区的数量
+ active_shards	活动主分区和副本分区的总数
+ relocating_shards	正在重定位的分片的数量
+ initializing_shards	正在初始化的分片数
+ unassigned_shards	未分配的分片数
+ delayed_unassigned_shards	其分配因超时设置而延迟的分片数
+ number_of_pending_tasks	尚未执行的集群级别更改的数量
+ number_of_in_flight_fetch	未完成的访存数量
+ task_max_waiting_in_queue_millis	自最早的初始化任务等待执行以来的时间（以毫秒为单位）
+ active_shards_percent_as_number	群集中活动碎片的比率，以百分比表示

###  8.3. <a name='-1'></a>查找异常索引

    GET _cat/indices

####  8.3.1. <a name='-1'></a>查看索引并按照主分片大小排序

    curl -XGET -u<username:password> 'http://ip:port/ /_cat/indices?v&s=pri.store.size:desc'

###  8.4. <a name='doc'></a>计算doc

    curl -XGET -u<username:password> 'http://ip:port/_cat/indices?h=docs.count&s=docs.count:desc' | awk '{ sum += $1 } END { print sum }'

###  8.5. <a name='-1'></a>查看异常信息

    GET /_cluster/allocation/explain

###  8.6. <a name='-1'></a>查看分片状态

    GET _cat/shards/indexname?v

+ index：所有名称
+ shard：分片数
+ prirep：分片类型，p=pri=primary为主分片，r=rep=replicas为复制分片
+ state：分片状态，STARTED为正常分片，INITIALIZING为异常分片
+ docs：记录数
+ store：存储大小
+ ip：es节点ip
+ node：es节点名称

####  8.6.1. <a name='-1'></a>统计节点主分片数量

    curl -s <ip>:9200/_cat/shards |grep -w "p"|awk '{print $7}'|sort |uniq -c

###  8.7. <a name='-1'></a>查看异常信息

    GET /_cluster/allocation/explain

unassigned_info.reason 为分片未分配原因
+ INDEX_CREATED	索引创建，由于API创建索引而未分配的
+ CLUSTER_RECOVERED	集群恢复，由于整个集群恢复而未分配
+ INDEX_REOPENED	索引重新打开
+ DANGLING_INDEX_IMPORTED	导入危险的索引
+ NEW_INDEX_RESTORED	重新恢复一个新索引
+ EXISTING_INDEX_RESTORED	重新恢复一个已关闭的索引
+ REPLICA_ADDED	添加副本
+ ALLOCATION_FAILED	分配分片失败
+ NODE_LEFT	集群中节点丢失
+ REROUTE_CANCELLED	reroute命令取消
+ REINITIALIZED	重新初始化
+ REALLOCATED_REPLICA	重新分配副本

###  8.8. <a name='-1'></a>尝试重新分配

    POST /_cluster/reroute?retry_failed=true