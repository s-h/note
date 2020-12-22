# logrotate
配置放在/etc/logrotate.d/xx.conf

/data/docker/nginx/logs/*log {
    daily                 //每天
    rotate 10             //保留10份
    missingok
    notifempty
    compress
    sharedscripts
    create 0664 root root
    postrotate            //执行命令
        docker container kill 1a7a -s USR1
    endscript
}

# 其他参数
## copytruncate
可以理解为把内容拷贝走作为备份，然后清空当前文件。但是这有一个问题就是拷贝和截断之间会有时间差，存在丢数据的可能。
