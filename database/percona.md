# percona
## install




## percona-toolkit

## percona-xtrabackup
### install

    apt-get install percona-xtrabackup-24

### pt-query-digest
分析慢查询

    pt-query-digest  --since=24h /xxx/mysql-slow.log

### pt-mysql-summary
mysql概述

    pt-mysql-summary --user=xxx --password=xxx