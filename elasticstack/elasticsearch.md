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
创建名为my_bakcup的仓库，仓库的类型为共享文件系统，并设置挂载点。共享文件需要所有节点都可访问。

    PUT _snapshot/my_backup/
    {
        "type": "fs",
        "settings": {
            "location": "/mount/backups/my_backup"
        }
    }

还需要在es配置文件配置path.repo参数，并重启es节点：

    path.repo:["/mount/backups/my_bakcup"]

修改仓库流量控制参数，必须在仓库未使用时配置：

    POST _snapshot/my_backup/
    {
        "type": "fs",
        "settings": {
            "location": "/mount/backups/my_backup",
            "max_snapshot_bytes_per_sec" : "50mb",
            "max_restore_bytes_per_sec" : "50mb"
        }
    }

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

## 数据处理
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

### 删除数据

    GET test/_delete_by_query
    {
        "query" : {
            "match" : {
                "first_name" : "jianghao"
            }
        }
    }
