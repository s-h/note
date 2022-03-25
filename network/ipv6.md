# linux
## 查看ipv6路由

    ip -6 show 

## 添加默认路由

    ip -6 route add default via xxxx:xxxx:xxxx:xxxx:xxxx dev eth0

## 获取路由

    dhcpclient -6 eth0