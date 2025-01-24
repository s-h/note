[Control Group v2内核参考文档](https://www.kernel.org/doc/html/v5.10/admin-guide/cgroup-v2.html)

[[译]Control Group v2（cgroupv2 权威指南）](https://arthurchiao.art/blog/cgroupv2-zh/)

# 引言
## cgroup是什么
cgroup 是一种以 hierarchical（树形层级）方式组织进程的机制（a mechanism to organize processes hierarchically），以及在层级中以受控和 可配置的方式（controlled and configurable manner）分发系统资源 （distribute system resources）。

## cgroup组成部分
+ 核心（croe）：主要负责层级化的组织进程
+ 控制器（controllers）: 大部分控制器负责 cgroup 层级中 特定类型的系统资源的分配，少部分 utility 控制器用于其他目的。

## 进程、线程与cgroup的关系
所有 cgroup 组成一个树形结构（tree structure），

+ 系统中的每个进程都属于且只属于某一个 cgroup；
+ 一个进程的所有线程属于同一个 cgroup；
+ 创建子进程时，继承其父进程的 cgroup；
+ 一个进程可以被迁移到其他 cgroup；
+ 迁移一个进程时，子进程（后代进程）不会自动跟着一起迁移；

# 基础操作
## 挂载
与 v1 不同，cgroup v2 只有单个层级树（single hierarchy）。 用如下命令挂载 v2 hierarchy：
```bash
: # mount -t <fstype> <device> <dir>
$ mount -t cgroup2 none $MOUNT_POINT
```

###  控制器与 v1/v2 绑定关系
+ 所有支持 v2 且未绑定到 v1 的控制器，会被自动绑定到 v2 hierarchy，出现在 root 层级中。
+ v2 中未在使用的控制器（not in active use），可以绑定到其他 hierarchies。 

这说明我们能以完全后向兼容的方式，混用 v2 和 v1 hierarchy。

## 组织进程和线程
### 进程：创建/删除/移动/查看 cgroup
初始状态下，只有 root cgroup，所有进程都属于这个 cgroup

1. 创建sub-cgroup：只需要创建一个子目录
```bash
$ mkdir $CGROUP_NAME
```
+ 一个 cgroup 可以有多个子 cgroup，形成一个树形结构；
+ 同一 PID 可能出现多次：进程先移出再移入该 cgroup，或读文件期间 PID 被重用了，都可能发生这种情况

## 管理控制器
### 启用和禁用
每个 cgroup 都有一个 cgroup.controllers 文件， 其中列出了这个 cgroup 可用的所有控制器：