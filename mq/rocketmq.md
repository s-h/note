# rocketmq
帮助文档https://github.com/apache/rocketmq/blob/master/docs/cn/operation.md
## 安装
下载地址http://rocketmq.apache.org/release_notes/

## 单master模式
### 配置

### 首先启动Name Server
    $ nohup sh mqnamesrv &
    
    ### 验证Name Server 是否启动成功
    $ tail -f ~/logs/rocketmqlogs/namesrv.log
    The Name Server boot success...

### 启动 Broker

    ### 启动Broker
    $ nohup sh bin/mqbroker -n localhost:9876 &

    ### 验证Name Server 是否启动成功，例如Broker的IP为：192.168.1.2，且名称为broker-a
    $ tail -f ~/logs/rocketmqlogs/Broker.log 
    The broker[broker-a, 192.169.1.2:10911] boot success...

    