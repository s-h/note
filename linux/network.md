# 网络丢包排查
[网络丢包如何排查](https://zhuanlan.zhihu.com/p/502027581)
## 硬件网卡丢包排查
通过ethtool或/proc/net/dev可以查看因Ring Buffer满而丢弃的包统计，在统计项中以fifo标识

    cat /proc/net/dev
    find /sys -name eth0 |xargs cd ;cd statistics; grep . *

###  查看eth0网卡Ring Buffer最大值和当前设置

    ethtool -g eth0

### 修改网卡eth0接收与发送硬件缓存区大小

    ethtool -G eth0 rx 4096 tx 4096


## 网卡驱动丢包

## udp丢包
### 查看网卡udp统计

    netstat -i -udp


### 查看内核udp统计

    cat /proc/net/snmp |grep -w Udp
    cat /proc/net/udp