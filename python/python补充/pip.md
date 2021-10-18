# 使用镜像更新
## 临时修改

    pip install -i https://mirrors.aliyun.com/pypi/simple/ package_name

## 永久修改
~/.pip/pip.conf 

    [global]
    index-url = https://mirrors.aliyun.com/simple
    [install]
    trusted-host = https://mirrors.aliyun.com



