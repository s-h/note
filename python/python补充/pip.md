# 使用镜像更新
## linux
### 临时修改

    pip install -i https://mirrors.aliyun.com/pypi/simple/ package_name

### 永久修改
~/.pip/pip.conf 

    [global]
    index-url = https://mirrors.aliyun.com/simple
    [install]
    trusted-host = https://mirrors.aliyun.com

### 修复
使用apt-get安装的pip与当前版本不匹配，可以使用以下命令安装pip

    wget https://bootstrap.pypa.io/get-pip.py --no-check-certificate
    python3 get-pip.py

## windows
### 永久修改
先在 windows “文件资源管理器” 地址栏 输入 %APPDATA% 
在刚才创建好的 pip 文件夹中，新建 名为 pip.ini 的配置文件

    [global]
    index-url=http://mirrors.aliyun.com/pypi/simple/

    [install]
    trusted-host=mirrors.aliyun.com

# 依赖包导出

    pip freeze > requirements.txt