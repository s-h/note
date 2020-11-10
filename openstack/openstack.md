## 网络
### 网桥
cetnos使用bride-utils管理网桥

    # 查看网桥
    brctl show  
    # 创建网桥
    brctl addbr br1
    # 加入网桥
    brctl addif em1