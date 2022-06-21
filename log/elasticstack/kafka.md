## 安装维护
### zookeeper配置
### kafka配置
### 启动

    /kafkaPatch/bin/zookeeper.sh -daemon /kafkaPatch/config/zookeeper.properties
    /kafkaPatch/bin/kafka-server-start.sh -daemon /kafkaPatch/config/server.properties
## topic
### 查看所有topic

    ./kafka-topics.sh --list --zookeeper localhost:2181

### 查看topic

    ./kafka-topics.sh --describe --zookeeper localhost:2181 --topic topic_name

### 创建topic

    ./kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic  topic_name

### 查看队列内容

    ./kafka-console-consumer.sh --bootstrap-server 10.24.0.212:9092 --topic topic_name --from-beginning


### 删除队列

    ./kafka-topics.sh --delete --zookeeper 127.0.0.1:2181 --topic xxx
    ./zookeeper-shell.sh localhost:2181
    >ls /brokers/topics/[topic name]
    >rmr /brokers/topics/[topic name]

### 查看所有消费组

    ./kafka-consumer-groups.sh --all-groups --bootstrap-server localhost:9092  --list

### 查看消费组

    ./kafka-consumer-groups.sh --bootstrap-server localhost:9092  --group groupname --describe

### 修改partitions

    ./kafka-topics.sh --alter --zookeeper localhost:2181 --topic topicname --partitions 12

