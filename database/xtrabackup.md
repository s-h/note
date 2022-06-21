# xtrabackup
(参考)[https://www.modb.pro/db/45783]
xtrabackup是Percona公司的一款用于MySQL在线备份的工具，支持在线备份，全量备份和增量备份，单库单表备份。

# 非压缩备份、恢复
## MYSQL5.6、5.7
### 备份

    innobackupex --defaults-file=/etc/my.cnf  --user=root --password=123456  /path/to/BACKUP-DIR/

### 恢复

    innobackupex --apply-log /backups/2020-02-27_11-04-55/
    innobackupex --defaults-file=/etc/my.cnf --copy-back   /backups/2020-02-27_11-04-55/
    这里可以不使用innobackup直接copy
    $ rm /mysql_data_dir/*
    $ mv /backups/2020-02-27_11-04-55/* /mysql_data_dir
    
    $ chown -R mysql:mysql /path/the/datadir

## MYSQL8.0
### 备份

    xtrabackup --backup --target-dir=/data/backups/ --user=root --password=123456

### 恢复

    xtrabackup --prepare --target-dir=/data/backups/
    xtrabackup --copy-back --target-dir = /data/backups /
    这里未验证

    $ chown -R mysql:mysql /path/the/datadir

# 压缩方式备份、恢复
## MYSQL5.6、5.7
### 备份

    innobackupex  --defaults-file=/etc/my5.7-6670.cnf  --compress   --compress-threads=4 --user=root /tmp/2020-02-28-11-05

### 恢复

    innobackupex  --defaults-file=/etc/my.cnf  --decompress --user=root /tmp/2020-02-28-11-05/2021-02-28_11-05-01/
    innobackupex  --apply-log /tmp/2020-02-28-11-05/2021-02-28_11-05-01/
    innobackupex --defaults-file=/etc/my.cnf --copy-back   /tmp/2020-02-28-11-05/2021-02-28_11-05-01/
    这里可以不使用innobackup直接mv
    $ rm /mysql_data_dir/*
    $ mv /tmp/2020-02-28-11-05/2021-02-28_11-05-01/* /mysql_data_dir/

    $ chown -R mysql:mysql /path/the/datadir

## MYSQL8.0

### 备份
### 恢复
