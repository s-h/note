# set
参考[ruanyifeng.com/blog/2017/11/bash-set.html](ruanyifeng.com/blog/2017/11/bash-set.html)
这两种写法建议放在所有 Bash 脚本的头部。

    # 写法一
    set -euxo pipefail

    # 写法二
    set -eux
    set -o pipefail

-u 空变量停止执行
-x 输出执行语句
-e 遇到错误停止执行
-o pipefail 管道组合只要有一个子命令失败，停止执行