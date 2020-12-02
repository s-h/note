# nginx
## user
## worker_process 

    # 根据cpu自动
    worker_process auto;

## events
## http
### client_max_body_size 
上传文件大小限制
可以在http下全局配置，也可以在指定的location下配置

    client_max_body_size 100m;

### log_format
### access_log
### include
### upstream

    # 轮询（默认）
    upstream bakend {
        server 192.168.0.1:8080;
        server 192.168.0.2:8080;
    }

    # ip_hash 每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
    upstream bakend {
        ip_hash;
        server 192.168.0.1:8080;
        server 192.168.0.2:8080;
    }

    # weight  指定轮询权重
    upstream bakend {
        server 192.168.0.1:8080 weight=10;
        server 192.168.0.2:8080 weight=10;
    }

    1.down表示单前的server暂时不参与负载
    2.weight为weight越大，负载的权重就越大。
    3.max_fails：允许请求失败的次数默认为1.当超过最大次数时，返回proxy_next_upstream模块定义的错误
    4.fail_timeout:max_fails次失败后，暂停的时间。
    5.backup： 其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。

### server
#### listen
#### server_name
#### location

    location = /uri    = 表示精确匹配，只有完全匹配上才能生效
    location ^~ /uri    ^~ 开头对URL路径进行前缀匹配，并且在正则之前。
    location ~ pattern    开头表示区分大小写的正则匹配
    location ~* pattern    开头表示不区分大小写的正则匹配
    location /uri    不带任何修饰符，也表示前缀匹配，但是在正则匹配之后
    location /    通用匹配，任何未匹配到其它location的请求都会匹配到，相当于switch中的default

##### proxy_pass

    # 引用后端服务
    proxy_pass http://bakend/;