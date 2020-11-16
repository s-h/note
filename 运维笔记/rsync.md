# rsync

## server
编辑[服务端配置文件rsyncd.conf](conf/rsync/rsyncd.conf)

auth users #认证用户
secrets file #用户密码文件，格式 用户名:密码

## client

    # password-file 权限必须为600，格式 密码
    rsync -azvR --delete data rsync_user@x.x.x.x::test --password-file=rsync.password