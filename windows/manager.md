# 系统管理
## 组策略

    gpedit.msc        # 组策略编辑器
    gpupdate /force   # 刷新组策略

### 远程桌面数量
“组策略” -> “计算机配置” -> “管理模板” -> “windows组件” -> “远程桌面服务” -> “远程桌面会话主机” -> “连接” 修改 “限制连接的数量”
## 命令行
### 查看远程桌面端口

    tasklist /svc | find "Ter"
    netstat -ano | find "xxx"

## 用户管理

    lusrmgr.msc
