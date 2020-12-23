# apache
## 代理
<VirtualHost *:80>
    ServerName  foo.cn
	ProxyPass / http://localhost:8000/
	ProxyPassReverse / http://localhost:8000/
	ProxyPreserveHost On
    # 相当于nginx的proxy_set_header Host $host; 及访问源站是添加http头部host
</VirtualHost>

