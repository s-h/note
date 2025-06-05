# 硬链接
```bash
ls -l xxx
-rwxr-xr-x 1 
:# 其中第二列为硬链接数量，若数值大于1，则存在硬链接

stat xxx
:# 可以查看indoe信息，以及硬链接数量Links

find / -xdev -inum inode_num 
:# 查看相同inode文件，其中inode_num为stat查询到的
```

