# MXUserAllocSerialNumber: too many locks" (2149941)
![虚拟机自动关闭电源并显示错误“MXUserAllocSerialNumber: 锁定过多 (MXUserAllocSerialNumber: too many locks)” (2149941)](https://kb.vmware.com/s/article/2149941?lang=zh_cn)

在 /vmfs/volumes/datastore/virtual_machine/vmware.log 文件中，您会看到类似以下内容的条目：

2017-04-06T17:23:14.698Z| vcpu-0| I125: GuestMsg: Too many channels opened.
2017-04-06T17:23:14.699Z| vcpu-0| I125: GuestMsg: Too many channels opened.

在 vmware.log 文件中，您会看到类似以下内容的条目：
vcpu-0| E105: PANIC: MXUserAllocSerialNumber: too many locks!

vcpu-0| W115: A core file is available in "/vmfs/volumes/VOLUME_UUID/VM_NAME/vmx-zdump.000"

mks| W115: Panic in progress... ungrabbing

mks| I125: MKS: Release starting (Panic)

mks| I125: MKS: Release finished (Panic)

该问题在 ESXi 6.5 Update 1 上已解决

要在 ESXi 6.0 和 6.5 中临时解决该问题，请执行以下操作：


关闭虚拟机。

将 guest_rpc.rpci.usevsocket 参数添加到 virtual_machine.vmx 文件中的 false：

通过 SSH 会话连接到运行虚拟机的主机。有关详细信息，请参见 在 ESXi 5.x 和 6.0 中使用 ESXi Shell(2075199)。

导航到 /vmfs/volumes/virtual_machine_datastore/virtual_machine/virtual_machine.vmx 文件。

使用文本编辑器打开 virtual_machine.vmx 文件。

添加 guest_rpc.rpci.usevsocket = "FALSE" 参数。


打开虚拟机电源。

# ESXi6.5上的Ubuntu虚机在远程SSH时宕机
![Linux VM fails with the error "kernel BUG at drivers/net/vmxnet3/vmxnet3_drv.c:1413!" (2151480)](https://kb.vmware.com/s/article/2151480?lang=zh_cn)
Linux 虚拟机出现故障，并显示类似以下内容的错误：
BUG_ON：驱动程序/net/vmxnet3/vmxnet3_drv 的内核错误。 c:1413！
 
虚拟机未响应，并且您无法挂起
 
暂停虚拟机 vmx 进程不会有帮助

出现此问题的原因是，VMXNET3 vNIC 后端中的错误是 vmkernel 的一部分。如果满足以下条件，则会出现此问题：

+ Linux 虚拟机正在运行内核 > = 4。8
+ 虚拟机的硬件版本为 > = 13
+ ESXi 版本为6。5

Workaround
要解决此问题，如果您不希望升级，请使用以下选项之一：

在虚拟机的 vmx vmxnet3.rev.30 = FALSE vmxnet3 为 FALSE 参数：
关闭虚拟机电源
 
编辑 vmx 文件并添加以下参数：

vmxnet3.rev.30 = FALSE
 
打开虚拟机电源
如果您不希望关闭虚拟机的电源，请运行以下命令以禁用虚拟机上每个 VMXNET3 vNIC 的接收数据环：

ethtool -G ethX rx-mini 0
注意：将 Fp-ethx 替换为虚拟机的接口名称。