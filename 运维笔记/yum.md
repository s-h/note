# yum
## yum变量
通过python获取变量

    python -c 'import yum, pprint; yb = yum.YumBase(); pprint.pprint(yb.conf.yumvar, width=1)'

### $arch
作用：标识cpu的架构，如i386，i486，i586等
默认取值：默认根据cpu架构自动取值
设置方法：
在/etc/yum/vars/arch文件写入一个值，这个值就是这个变量的值（优先级高）
### $basearch
作用：标识cpu的基本架构。例如i486和i586等使用一个基本架构i386，AMD64和Intel64有一个基本的架构x86_64。
默认取值：默认根据cpu架构自动取值
设置方法：在/etc/yum/vars/basearch文件写入一个值，这个值就是这个变量的值（优先级高）
### $releasever
作用：标识操作系统的版本号。
默认取值：先查找/etc/yum.conf配置文件中distroverpkg配置的value，然后取得value对应的rpm包名，最后获取到这个rpm包的release版本号就是这个变量的值（如果是centos系统，默认情况下distroverpkg的value为centos-release，再取centos-release这个包的release号）
设置方法：在/etc/yum/vars/releasever文件写入一个值，这个值就是这个变量的值（优先级高）