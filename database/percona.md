# percona
## install
## percona-xtrabackup
### install

    apt-get install percona-xtrabackup-24

## percona-toolkit
### install

### pt-query-digest
#### 分析慢查询

    pt-query-digest  --since=24h /xxx/mysql-slow.log

#### 从网络抓包分析

    tcpdump -s 65535 -x -nn -q -tttt -i any -c 50000 port 3306 > mysql.tcp.txt
    pt-query-digest --type tcpdump mysql.tcp.txt > pt-query-digest-report.txt

### pt-mysql-summary
mysql概述

    pt-mysql-summary --user=xxx --password=xxx

### pt-config-diff
对不不同数据库配置

    pt-config-diff h=192.168.1.x,u=db_user,p=db_password  h=localhost,u=db_user,p=db_password,S=/xxx/mysql.sock,P=3306