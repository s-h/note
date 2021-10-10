# nat
conntrack默认最大跟踪65536个连接，查看当前系统设置最大连接数

    /proc/sys/net/netfilter/nf_conntrack_max

查看连接跟踪有多少条目

    cat /proc/sys/net/netfilter/nf_conntrack_count

查看conntrack记录

    cat /proc/net/nf_conntrack
