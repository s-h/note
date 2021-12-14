# systemed
## target
runlevel是一个旧的概念，systemd引入相似功能的target。不像数字表示启动级别，每个target都有名字和独特的功能，并且能同时启用多个。

#### 获取当前target

    systemctl list-untis --type=target

#### Sysv启动级别对应Systemd target
|Sysv启动级别  |Systemd目标                     |注释                   |
|-------------|-------------------------------|-----------------------|
|0            |runlevel0.target,poweroff.target|中断系统(halt)     |
|1,s,single   |runlevel1.target,rescue.target  |单用户      |
|2,4          |runlevel2.target,runlevel4,multi-user.target|用户自定义级别，通常识别为级别3     |
|3            |runlevel3.target,multi-user.target|多用户，无图形，有网络。     |
|5            |runlevel5.target,graphical.target|继承3并启动图形     |
|6            |runleve6.target,reboot.target|重启     |
|emergency    |emergency.target|急救模式（Emergency shell)     |

#### 切换启动target

    To switch to Emergency target, simply run following command as root:
    # systemctl emergency
    Broadcast message from root@geeklab on pts/1 (Mon 2016-08-17 00:44:58 EDT):
    The system is going down to emergency mode NOW!

    To prevent systemd from sending informative message:
    # systemctl --no-wall emergency
    # systemctl isolate emergency.target

##### 修改启动级别
###### 通过systemctl修改

    systemctl enable multi-user.target

###### 通过grub2修改
在grub2启动界面，按e在linux16开头行最后增加后ctrl+x

    systemd.unit=emergency.target

