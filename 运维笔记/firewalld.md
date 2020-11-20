# firewalld
## 查看规则

    firewall-cmd --list-all

## 添加规则
### 根据端口添加

    firewall-cmd --zone=public --add-port=80/tcp --permanent

### 根据源地址添加/删除
允许192.168.1.1访问9200端口

    firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.1.1" port protocol="tcp"  port="11300" accept"
    firewall-cmd --permanent --remove-rich-rule="rule family="ipv4" source address="192.168.1.1" port protocol="tcp"  port="11300" accept"

### 更新规则

    firewall-cmd --reload