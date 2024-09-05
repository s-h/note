# 新建终端

    screen -S 终端名

# 列出终端

    screen -ls

# 连接终端

    screen -r xxx   (xxx为ls看到的数字id)

# 常见问题
有时在恢复 screen 时会出现 There is no screen to be resumed matching ****，遇到这种情况输入命令

    screen -d xxx
    然后在使用-r

# 删除会话

    screen -X -S xxx quit