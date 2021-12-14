# jenkins
## 启动
可以通过java命令或tomcat启动
通过环境变量配置jenkins主目录，默认在~/.jenkins，如是tomcat需修改配置文件

    export JENKINS_HOME=/data/jenkins
    java -jar jenkins.war --httpPort=8080 --daemon

