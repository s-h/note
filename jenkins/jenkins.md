# jenkins
## 启动
可以通过java命令或tomcat启动
通过环境变量配置jenkins主目录，默认在~/.jenkins，如是tomcat需修改配置文件

    export JENKINS_HOME=/data/jenkins
    java -jar jenkins.war --httpPort=8080 --daemon

## gitlab
### gitlab传入的变量
使用${env.gitlabBranch}访问

    gitlabBranch
    gitlabSourceBranch
    gitlabActionType
    gitlabUserName
    gitlabUserEmail
    gitlabSourceRepoHomepage
    gitlabSourceRepoName
    gitlabSourceNamespace
    gitlabSourceRepoURL
    gitlabSourceRepoSshUrl
    gitlabSourceRepoHttpUrl
    gitlabMergeRequestTitle
    gitlabMergeRequestDescription
    gitlabMergeRequestId
    gitlabMergeRequestIid
    gitlabMergeRequestState
    gitlabMergedByUser
    gitlabMergeRequestAssignee
    gitlabMergeRequestLastCommit
    gitlabMergeRequestTargetProjectId
    gitlabTargetBranch
    gitlabTargetRepoName
    gitlabTargetNamespace
    gitlabTargetRepoSshUrl
    gitlabTargetRepoHttpUrl
    gitlabBefore
    gitlabAfter
    gitlabTriggerPhrase