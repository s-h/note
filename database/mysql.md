# mysql
## 维护命令
### 查看mysql版本

    SELECT version();

### 查看引擎
suport字段为DEFAULT的为默认引擎

    show engines；

### 查看binlog

    show variables like '%log_bin%';

## mysqludmp
