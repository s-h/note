## 创建虚拟环境
python3.3之后venv已经作为标准库

    python3 -m venv 安装路径

--system-site-packages代表使用全局环境中的第三方库（否则虚拟环境直接是纯洁的第三方库）
--without-pip代表不安装pip（一般都是要装的，所以默认就行）
## 激活虚拟环境
### windows

    已管理员打开powershell，允许powershell运行脚本：
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned

    进入虚拟环境环境安装目录Scripts
    .\Activate.ps1

### linux

    进入虚拟环境环境安装目录Scripts
    source activate 虚拟环境名字

## 保存虚拟环境

    pip freeze >requirements.txt
    