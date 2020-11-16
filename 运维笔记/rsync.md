# rsync

## server
编辑[服务端配置文件rsyncd.conf](conf/rsync/rsyncd.conf)

auth users #认证用户

secrets file #用户密码文件，格式 用户名:密码

## client

    # password-file 权限必须为600，格式 密码
    # -pgo 保持文件权限
    # --delete 删除文件，不加该参数本地删除后远程不删除
    rsync -azvR -pgo --delete data rsync_user@118.89.248.41::test --password-file=rsync.password