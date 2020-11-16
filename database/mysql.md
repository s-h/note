# mysql
## 维护命令
### 查看mysql版本

    SELECT version();

### 查看引擎
suport字段为DEFAULT的为默认引擎

    show engines;

### 查看binlog

    show variables like '%log_bin%';

### 锁表

    # 查看锁表 
    # 返回Table_locks_immediate结果，意思是表被锁了总数
    # Table_locks_waited表示有多少请求等待表锁
    SHOW STATUS LIKE 'Table_locks%';

    # InnoDB_row_lock行锁的争夺情况
    # show status like 'innodb_row_lock%';

## mysqludmp