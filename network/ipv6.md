# linux
## 查看ipv6路由

    ip -6 route show 

## 添加默认路由

    ip -6 route add default via xxxx:xxxx:xxxx:xxxx:xxxx dev eth0

## 获取路由

    dhcpclient -6 eth0

## 临时添加ipv6地址

    ip -6 addr add xxx/64 dev eth0