# rsync

## server
编辑[服务端配置文件rsyncd.conf](conf/rsync/rsyncd.conf)

auth users #认证用户

secrets file #用户密码文件，格式 用户名:密码

## client

    # password-file 权限必须为600，格式 密码
    # -pgo 保持文件权限
    # --delete 删除文件，不加该参数本地删除后远程不删除
    rsync -azvR -pgo --delete /data rsync_user@118.89.248.41::test --password-file=rsync.password

## 过滤规则
rsync的include和exclude可以说是最令人困惑的地方之一了，我的建议源文件以及exclude的路径都使用绝对路径。
详细请参考[https://developer.aliyun.com/article/428319](https://developer.aliyun.com/article/428319)

每个**include/exclude规则**指定一个模式来匹配传输文件。模式可以有以下几种形式：
#### "/"出现在开头
 "/"出现在开头，如果源文件使用绝对路径，那么表示include/exclude使用的是和系统一直的绝对路径；如果源文件使用的相对路径，表示相当于源文件进行了chroot的绝对路径。
#### 不以"/"开头
如 --exclude foo 将匹配任何位置的foo，因为算法是自上而下递归生效。再比如sub/foo会匹配任何位置的sub/foo的foo文件。
#### "/"出现在结尾
如果”/”出现在模式的结尾，那么它只匹配目录，而不匹配常规文件、链接，或设备。
#### 通配符
+ * ：匹配路径的任何部分，遇到斜杠终止
+ ** ：匹配任何东西，包括斜杠
+ ? ：匹配任何单个字符，斜杠(“/”)除外
+ [ ：匹配一个字符集，如[a-z]，或[[:alpha:]]