# 查看zxid并计算低32位使用率
Zxid 是 Zookeeper 中用于唯一标识事务的一个标识符，全称为 ZooKeeper Transaction ID。Zxid 是一个 64 位的（8 字节）整数，由两个部分组成：

高 32 位表示纪元（epoch），这是每次领导者选举后递增的一个计数器。
低 32 位表示计数器（counter），这是在一次纪元内的事务计数。
Zxid 是按照高 32 位和低 32 位组合成 64 位整数保存的。

```bash
echo  srvr | nc $zk_ip $zk_port |grep -i 'zxid'

# 低32位使用率
zxid=$(echo  srvr | nc $zk_ip $zk_port |grep -i 'zxid'| awk '{print $2}')
low_zxid=$(($zxid & 0xFFFFFFFF))
echo $(($low_zxid * 100 / 4294967295))
```