# nginx 配置
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
##### try_files
可应用的上下文：server，location段
格式1：try_files file ... uri;  格式2：try_files file ... =code;
关键点1：按指定的file顺序查找存在的文件，并使用第一个找到的文件进行请求处理
关键点2：查找路径是按照给定的root或alias为根路径来查找的
关键点3：如果给出的file都没有匹配到，则重新请求最后一个参数给定的uri，就是新的location匹配
关键点4：如果是格式2，如果最后一个参数是 = 404 ，若给出的file都没有匹配到，则最后返回404的响应码

    location ^~ / {
                root  /data/nginx/html/;
                index  index.html index.htm;
                try_files $uri $uri/ /index.html last;
        }
    适合vue spa应用配置


##### proxy_pass

    # 引用后端服务
    proxy_pass http://bakend/;

# nging遇到问题
## location pass_porxy 代理站点路径跳转问题
location xxx配置代理后，页面中的链接、跳转都不带location，因为网页中的链接使用了绝对路径如/static/1.css，而不是相对路径如static/1.css

结果办法：使用相对路径或者使用不同域名直接使用location /
