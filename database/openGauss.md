# 基本命令
## 查询所有schema

    SELECT nspname FROM pg_namespace;

## 切换schema

    SET search_path TO <Schema名>;