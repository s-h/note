# Maven
**Maven**是一个项目管理工具，对java进行构建、依赖管理
## POM
POM(Project Object Model, 项目对象模型)是一个XML文件，包含项目的基本信息，用于描述项目如何构建，声明项目依赖。

    <project xmlns = "http://maven.apache.org/POM/4.0.0"
    xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/xsd/maven-4.0.0.xsd">
 
    <!-- 模型版本 -->
    <modelVersion>4.0.0</modelVersion>
    <!-- 公司或者组织的唯一标志，并且配置时生成的路径也是由此生成， 如com.companyname.project-group，maven会将该项目打成的jar包放本地路径：/com/companyname/project-group -->
    <groupId>com.companyname.project-group</groupId>
 
    <!-- 项目的唯一ID，一个groupId下面可能多个项目，就是靠artifactId来区分的 -->
    <artifactId>project</artifactId>
 
    <!-- 版本号 -->
    <version>1.0</version>
    </project>

## 生命周期
|阶段|处理|描述|
|-|-|-|
|验证|validate|验证项目|验证项目是否正确且所有必须信息是可用的|
|编译|compile|执行编译|源代码编译在此阶段完成|
|测试|Test|测试|使用适当的单元测试框架（例如JUnit）运行测试。|
|包装|package|打包|创建JAR/WAR包如在|pom.xml|中定义提及的包|
|检查|verify|检查|对集成测试的结果进行检查，以保证质量达标|
|安装|install|安装|安装打包的项目到本地仓库，以供其他项目使用|
|部署|deploy|部署|拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程|