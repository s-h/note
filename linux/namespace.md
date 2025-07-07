# 查找所有net namespace的特定的网络连接
使用netstat只能查看默认的网络命名空间的连接，看不到使用其他网络命名空间容器内的连接
```bash
:# 查找所有网络命名空间
lsns -t net

NS TYPE NPROCS   PID USER     NETNSID NSFS                                                COMMAND
4026531840 net     185     1 root  unassigned                                                     /sbin/init
4026532347 net       3 42194 65535          0 /run/netns/cni-c82190f1-9c7f-0d7c-0faf-3025d27b6d22 /pause
4026532433 net       2  4526 65535          2 /run/netns/cni-403f47f5-4ee9-ba87-6625-a9c9d30185bf /pause

:# 排除pid为1的，其他均为容器的
:# 根据pid使用nsenter查找网络连接

nsenter -t <pid> -n netstat -anp |grep xxx
```

# 批量查询一个主机所有网络命名空间连接
netstat查看网络连接查看的是默认的命名空间，如果有程序如容器使用其他命名空间则需要配合nsenter
```bash
while ture do; do
    date
    pid=$(lsns -t net |grep -v PID | awk '{print $4}')
    for i in $pid;do
        echo $i
        nsenter -t $i -n netstat -nap |grep xxx
        echo "==="
    done 
    sleep 60
done
```
也可使用此工具https://github.com/s-h/netnsrun