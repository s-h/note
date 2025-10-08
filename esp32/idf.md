# 安装

[vscode安装esp-idf](https://github.com/espressif/vscode-esp-idf-extension/blob/master/README_CN.md)，使用vscode扩展安装esp-idf（Configure ESP-IDF Extension）

# idf.py命令介绍
[常用命令介绍](https://duruofu.github.io/ESP32-Guide/docs/guide/01.%E8%AE%A4%E8%AF%86ESP32/1.3-%E5%88%9D%E8%AF%95ESP32-idf.py%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8/idf.py%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.html)

| 功能             | 命令                                                    | 备注                                                |
| ---------------- | ------------------------------------------------------- | --------------------------------------------------- |
| 创建新工程       | `idf.py create-project <project name>`                  | `<project name>` 为项目名称                         |
| 创建新组件       | `idf.py -C components create-component {componentName}` | `{componentName}` 为组件名称                        |
| 打开串口监视器   | `idf.py -p /dev/ttyUSB0 monitor`                        | `/dev/ttyUSB0` 为目标串口，根据实际情况修改         |
| 打开文档         | `idf.py docs`                                           |                                                     |
| 构建、烧录并监视 | `idf.py -p /dev/ttyUSB0 flash monitor`                  | `/dev/ttyUSB0` 为目标串口，根据实际情况修改         |
| 构建工程         | `idf.py build`                                          | 编译生成固件                                        |
| 启动图形配置工具 | `idf.py menuconfig`                                     | 配置项目的菜单选项                                  |
| 清除构建输出     | `idf.py clean`                                          | 清除中间文件                                        |
| 删除所有构建内容 | `idf.py fullclean`                                      | 清除所有生成的文件                                  |
| 烧录工程         | `idf.py -p /dev/ttyUSB0 flash`                          | `/dev/ttyUSB0` 为目标串口，根据实际情况修改         |
| 选择目标芯片     | `idf.py set-target <target>`                            | `<target>` 为芯片型号，不输入参数会列出所有可用型号 |
