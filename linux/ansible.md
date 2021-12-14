# ansible
## playbook
### 目录结构
.
└── ansible
    ├── site.yaml ansilbe统一入口
    └── roles 角色目录
        ├── common 公共roles目录
        │   ├── tasks 主要任务列表
        │   ├── handlers 包含处理程序，可以由此角色使用，甚至可以在此角色之外的任何位置使用
        │   ├── defaults 角色默认变量
        │   ├── vars 角色其他变量
        │   ├── files 包含可以通过此角色部署的文件
        │   ├── templates 包含可以通过此角色部署的模板
        │   └── meta 角色元数据
        └── nginx
            ├── tasks
            ├── handlers
            ├── defaults
            ├── vars
            ├── files
            ├── templates
            └── meta
#### site.yaml
hosts定义哪些主机执行，roles定义执行哪些角色脚本

    cat backup/site.yml 

    ---
    - name: backup
    hosts: "{{ myserver }}"
    remote_user: root

    roles:
        - backup

#### copy 拷贝文件
src不指定绝对路径则拷贝files目录下文件，否则按绝对路径拷贝

    ---
    - name: copy server status shell
      copy: src="server_state.sh" dest="/tmp/server_state.sh" mode="a+x"

#### file 操作文件
创建文件或目录、删除文件或目录、修改文件权限、创建连接等。
path: 指定要操作的目录或文件
state: directory/touch(文件)/link/hard/absent(删除)
src: 指定link/hard链接源文件 force：强制创建链接文件
owner: 属主
group：属组
mode： 权限
recurse: 递归修目录文件属性

    - name: rm file 
      file: path=/nginx/html/web state=absent 

#### unarchive 解压
copy：默认为yes，当copy=yes，那么拷贝的文件是从ansible主机复制到远程主机上的，如果设置为copy=no，那么会在远程主机上寻找src源文件
src：源路径，可以是ansible主机上的路径，也可以是远程主机上的路径，如果是远程主机上的路径，则需要设置copy=no
dest：远程主机上的目标路径
mode：设置解压缩后的文件权限

    - name: update zip
      unarchive: src=/ansibleServerPath/dist.zip dest=/nginx/html/ copy=yes

#### synchronize

    - name: sync jars2
    synchronize: src=/data/file.zip dest=/data/
    when: is_jars == "true"
