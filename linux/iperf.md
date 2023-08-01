# 测试包转发率

## 服务端

    iperf -s -i 1 -u -p 20000

## 客户端

    iperf -c 服务端ip地址 -u -i 1 -t 3600 -p 20000  -l 16  -P 16

## 查看包转发率

    sar -n DEV 1 1000