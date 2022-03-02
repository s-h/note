## 数据迁移
### reindex
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

### snapshot
快照适用于数据备份以及大量数据迁移
#### 配置nfs
服务端：

    apt-get install nfs-kernerl-server nfs-common
    #编辑配置文件sudo vi /etc/exports
    /es_snapshot x.x.x.x/24(rw,sync,all_squash)  
    chown nobody.nogroup /es_snapshot
    /etc/init.d/nfs-kernel-server restart

客户端：

    apt-get install nfs-common
    mount -t nfs x.x.x.x/es_snapshot /nfs_share

#### 设置挂载点
所有节点挂载文件系统/nfs_share(名称自定义)
编辑elasticsearch.yml增加:

    path.repo: ["/nfs_share"]

#### 设置仓库
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
#### 生成快照
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

#### 恢复快照
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

## cluster settings
###  副本磁盘空间检查
超过85%(默认)将不在该节点分配副本，直接关闭

    GET _cluster/settings
    PUT _cluster/settings
    {
      "transient": {
      "cluster.routing.allocation.disk.threshold_enabled": false
      }
    }

精确控制：
low

    PUT _cluster/settings
    {
      "transient": {
        "cluster.routing.allocation.disk.watermark.low": "100gb",
        "cluster.routing.allocation.disk.watermark.high": "50gb",
        "cluster.routing.allocation.disk.watermark.flood_stage": "10gb",
        "cluster.info.update.interval": "1m"
      }
    }

## 数据管理
### 计算集群文档数量

    GET _count?pretty
    {
        "query": {
            "match_all": {}
        }
    }

### 数据插入

    PUT /indexname/typename/id
    {
        "first_name" : "John",
        "last_name" :  "Smith",
        "age" :        25,
        "about" :      "I love to go rock climbing",
        "interests": [ "sports", "music" ]
    }

### 查询数据

    GET filebeat-6.2.4-2019.08.19/_search
    {
      "query": {
        "match": {
          "source": "/var/log/vmware-network.log"
        }
      }
    }

#### 指标聚合
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

### 删除数据

    GET test/_delete_by_query
    {
        "query" : {
            "match" : {
                "first_name" : "jianghao"
            }
        }
    }

## 分片、索引管理
### 移动分片

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
