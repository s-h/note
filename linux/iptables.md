# conntrack
conntrack用于跟踪连接状， nat根据连接状态进行回包。
## 安装

    apt-get install conntrack

## 使用

    conntrack -E  显示事件
    conntrack -L  查看连接跟踪条目

## udp连接超时时间
conntrack 状态：
[ASSURED]: 在两个方面（即请求和响应）方向都看到了流量
[UNREPLIED]: 尚未在响应方向上看到流量。如果连接跟踪缓存溢出，则首先删除这些连接

net.netfilter.nf_conntrack_udp_timeout = 30
net.netfilter.nf_conntrack_udp_timeout_stream = 180

### 测试机器
Ubuntu 16.04.7 LTS  4.4.0-190-generic
192.168.9.11 (后端服务端口25000)
192.168.9.12 (nat)
192.168.9.13 (客户机)

### 测试规则

    iptables -t nat -A PREROUTING -d 192.168.9.12/32 -p udp -m udp --dport 25000 -j DNAT --to-destination 192.168.9.11:25000
    iptables -t nat -A POSTROUTING -d 192.168.9.11 -p udp -m udp --dport 25000 -j SNAT --to-source 192.168.9.12

### 测试结果
客户端端只有2次发送才算ASSURED, 超时时间变为net.netfilter.nf_conntrack_udp_timeout_stream设置时间, 否则为net.netfilter.nf_conntrack_udp_timeout

    #步骤一： 192.168.9.13执行：(使用12345源端口向192.168.9.12 25000发送一个udp包)
        echo -n "hello" | nc -4u -w1 192.168.9.12 25000 -p 12345
    #步骤二： 192.168.9.11:(模拟服务端相应给iptables)
        echo -n "hello" | nc -4u -w1 192.168.9.12 12345 -p 25000
    #步骤三： 192.168.9.13:(模拟客户端再次请求，无需响应此时iptables连接表已变更为[ASSURED])
        echo -n "hello" | nc -4u -w1 192.168.9.12 25000 -p 12345

    # iptables查看：
    $ conntrack -E 
    [NEW] udp      17 30 src=192.168.9.13 dst=192.168.9.12 sport=12345 dport=25000 [UNREPLIED] src=192.168.9.11 dst=192.168.9.12 sport=25000 dport=12345
    [UPDATE] udp      17 30 src=192.168.9.13 dst=192.168.9.12 sport=12345 dport=25000 src=192.168.9.11 dst=192.168.9.12 sport=25000 dport=12345
    [UPDATE] udp      17 180 src=192.168.9.13 dst=192.168.9.12 sport=12345 dport=25000 src=192.168.9.11 dst=192.168.9.12 sport=25000 dport=12345 [ASSURED]

