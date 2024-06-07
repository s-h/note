# 挂载磁盘到wsl
[挂载磁盘](https://learn.microsoft.com/zh-cn/windows/wsl/wsl2-mount-disk)

## 装载分区磁盘
```
# 1. 标识磁盘 列出windows可用磁盘
GET-CimInstance -query "SELECT * from Win32_DiskDrive"

# 2. 挂载
wsl --mount <DiskPath> --bare

# 3. wsl2中查看

lsblk
```

