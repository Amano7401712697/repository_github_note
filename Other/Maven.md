# Maven

## Maven 概述

​	**Maven是 Apache 下的一个纯 Java 开发的开源项目。基于项目对象模型（缩写：POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤。Maven 是一个项目管理工具，可以对 Java 项目进行构建、依赖管理。**



## Maven POM

​	**POM( Project Object Model，项目对象模型 ) 是 Maven 工程的基本工作单元，是一个XML文件，包含了项目的基本信息，用于描述项目如何构建，声明项目依赖，等等。执行任务或目标时，Maven 会在当前目录中查找 POM。它读取 POM，获取所需的配置信息，然后执行目标。**

- POM 中可以指定以下配置：
  - 项目依赖
  - 插件
  - 执行目标
  - 项目构建 profile
  - 项目版本
  - 项目开发者列表
  - 相关邮件列表信息

```xml
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
```

**所有 POM 文件都需要 project 元素和三个必需字段：groupId，artifactId，version。**

- project ： 工程的根标签。
- groupId :　这是工程组的标识。它在一个组织或者项目中通常是唯一的。
- artifactId ：这是工程的标识。它通常是工程的名称。
- version：这是工程的版本号。在 artifact 的仓库中，它用来区分不同的版本。



## Maven 构建的生命周期

Maven 构建生命周期定义了一个项目构建跟发布的过程。

| 阶段          | 处理     | 描述                                                     |
| ------------- | -------- | -------------------------------------------------------- |
| 验证 validate | 验证项目 | 验证项目是否正确且所有必须信息是可用的                   |
| 编译 compile  | 执行编译 | 源代码编译在此阶段完成                                   |
| 测试 Test     | 测试     | 使用适当的单元测试框架（例如JUnit）运行测试。            |
| 包装 package  | 打包     | 创建JAR/WAR包如在 pom.xml 中定义提及的包                 |
| 检查 verify   | 检查     | 对集成测试的结果进行检查，以保证质量达标                 |
| 安装 install  | 安装     | 安装打包的项目到本地仓库，以供其他项目使用               |
| 部署 deploy   | 部署     | 拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程 |

### Maven的三个标准生命周期

- **clean**：项目清理的处理
- **default(或 build)**：项目部署的处理
- **site**：项目站点文档创建的处理



## Maven 构建配置文件(POM)

​	构建配置文件是一系列的配置项的值，可以用来设置或者覆盖 Maven 构建默认值。使用构建配置文件，你可以为不同的环境，比如说生产环境（Producation）和开发（Development）环境，定制构建方式。配置文件在 pom.xml 文件中使用 activeProfiles 或者 profiles 元素指定，并且可以通过各种方式触发。配置文件在构建时修改 POM，并且用来给参数设定不同的目标环境（比如说，开发（Development）、测试（Testing）和生产环境（Producation）中数据库服务器的地址）。

## Maven 仓库

​	Maven 仓库是项目中依赖的第三方库，这个库所在的位置叫做仓库。在 Maven 中，任何一个依赖、插件或者项目构建的输出，都可以称之为构件。Maven 仓库能帮助我们管理构件（主要是JAR），它就是放置所有JAR文件（WAR，ZIP，POM等等）的地方。

**三种类型的Maven仓库**

-  本地(local)
- 中央(central)
- 远程(remote)

**本地仓库**

​	Maven 的本地仓库，在安装 Maven 后并不会创建，它是在第一次执行 maven 命令的时候才被创建。运行 Maven 的时候，Maven 所需要的任何构件都是直接从本地仓库获取的。如果本地仓库没有，它会首先尝试从远程仓库下载构件至本地仓库，然后再使用本地仓库的构件。

```xml
<!-- 在maven的配置文件中一下代表本地仓库的位置,Maven 将下载依赖的文件到你指定的路径中。 -->
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 
   http://maven.apache.org/xsd/settings-1.0.0.xsd">
      <localRepository>C:/MyLocalRepository</localRepository>
</settings>
```

**中央仓库**

​	Maven 中央仓库是由 Maven 社区提供的仓库，其中包含了大量常用的库。中央仓库包含了绝大多数流行的开源Java构件，以及源码、作者信息、SCM、信息、许可证信息等。中央仓库由Maven社区管理,不需要配置,并且需要网络才可以连接.

**远程仓库**

​	如果 Maven 在中央仓库中也找不到依赖的文件，它会停止构建过程并输出错误信息到控制台。为避免这种情况，Maven 提供了远程仓库的概念，它是开发人员自己定制仓库，包含了所需要的代码库或者其他工程中用到的 jar 文件。

### Maven 依赖搜索顺序

当我们执行 Maven 构建命令时，Maven 开始按照以下顺序查找依赖的库：

1.  在本地仓库中搜索，如果找不到，执行步骤 2，如果找到了则执行其他操作。
2. 在中央仓库中搜索，如果找不到，并且有一个或多个远程仓库已经设置，则执行步骤 4，如果找到了则下载到本地仓库中以备将来引用。
3. 如果远程仓库没有被设置，Maven 将简单的停滞处理并抛出错误（无法找到依赖的文件）。
4.  在一个或多个远程仓库中搜索依赖的文件，如果找到则下载到本地仓库以备将来引用，否则 Maven 将停止处理并抛出错误（无法找到依赖的文件）。

### Maven 阿里云(Aliyun)仓库

```xml
<mirrors>
    <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>        
    </mirror>
</mirrors>
```

## Maven插件

​	每个生命周期中都包含着一系列的阶段(phase)。这些 phase 就相当于 Maven 提供的统一的接口，然后这些 phase 的实现由 Maven 的插件来完成。 Maven 实际上是一个依赖插件执行的框架，每个任务实际上是由插件完成。

Maven 插件通常被用来：

- 创建 jar 文件
- 创建 war 文件
- 编译代码文件
- 代码单元测试
- 创建工程文档
- 创建工程报告



### 插件的类型

| 类型              | 描述                                               |
| ----------------- | -------------------------------------------------- |
| Build plugins     | 在构建时执行，并在 pom.xml 的 元素中配置。         |
| Reporting plugins | 在网站生成过程中执行，并在 pom.xml 的 元素中配置。 |

常用插件列表: 

| 插件     | 描述                                                |
| -------- | --------------------------------------------------- |
| clean    | 构建之后清理目标文件。删除目标目录。                |
| compiler | 编译 Java 源文件。                                  |
| surefile | 运行 JUnit 单元测试。创建测试报告。                 |
| jar      | 从当前工程中构建 JAR 文件。                         |
| war      | 从当前工程中构建 WAR 文件。                         |
| javadoc  | 为工程生成 Javadoc。                                |
| antrun   | 从构建过程的任意一个阶段中运行一个 ant 任务的集合。 |

## Maven常用插件

```xml
<build>
		<plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat8-maven-plugin</artifactId>
                <version>3.0-r1756463</version>
                <configuration>
                    <!-- 这里配置端口号和访问路径 -->
                    <path>/wfdev</path>
                    <url>http://localhost:8080/hello</url>
                    <username>tomcat</username>
                    <password>tomcat</password>
                </configuration>
        </plugin>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-war-plugin</artifactId>
            <version>2.4</version>
            <configuration>
                <failOnMissingWebXml>flase</failOnMissingWebXml>
            </configuration>
        </plugin>
</build>
```



