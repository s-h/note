# rabbitmq
## 创建用户
rabbitmqctl add_user <username> <password>
## 设置密码
rabbitmqctl change_password <username> <password>
## 设置用户角色
rabbitmqctl set_user_tags <username> <administrator|monitoring|management>
## 赋权
rabbitmqctl set_permissions -p / <username> ".*" ".*" ".*"
## 查看队列
rabbitmqctl list_queues