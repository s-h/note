## consumer 与 分区关系
topic下的一个分区只能被同一个consumer group下的一个consumer线程来消费。

如果你的分区数是N，那么最好线程数也保持为N，这样通常能够达到最大的吞吐量。超过N的配置只是浪费系统资源，因为多出的线程不会被分配到任何分区。

## 命令
### 查看所有topic

    ./kafka-topics.sh --bootstrap-server x.x.x.x:9092 --list

### 查看topic详情

    ./kafka-topics.sh --bootstrap-server x.x.x.x:9092 --topic topic_name --describe

### 查看所有消费组

    ./kafka-consumer-groups.sh --bootstrap-server x.x.x.x:9092 --list

### 查看消费组详情

    ./kafka-consumer-groups.sh --bootstrap-server x.x.x.x:9092 --describe --group group_name

#### 创建topic

    ./kafka-topics.sh --create --bootstrap-server x.x.x.x:9092  --replication-factor 2 --partitions 3 --topic topic_name

#### 删除topic

    ./kafka-topics.sh --delete --bootstrap-server x.x.x.x:9092 --topic topic_name