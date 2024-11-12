# 基于Maven的包依赖管理与仓库配置实验

## 实验原理

### Maven基本命令

Maven运行命令格式如下:

```Plain Text
mvn [options] [<goal(s)>] [<phase(s)>]
```
使用`mvn -h` 可以查看到Maven所以可选项. Maven三个标准生命周期常用的阶段包括:

   * `clean` -`clean`
   * `default` - `validate`, `compile`, `test`, `package`, `verify`, `install`, `deploy`
   * `site` - `site`, `site-deploy`

例如, 打包项目、生成所有文件站点信息并将其部署至仓库的命令为:

```Plain Text
mvn clean deploy site-deploy
```
常用IAR打包命令为:

```Plain Text
mvn clean package -Dmaven.test.skip=true  # 跳过测试打包
mvn clean install -Dmaven.test.skip=true  # 跳过测试打包,并把打好的包上传到本地仓库
mvn clean deploy -Dmaven.test.skip=true  # 跳过测试打包,并把打好的包上床到远程仓库
```
Maven是一款项目管理工具, 通过POM(Project Object Model)文件来定义项目的结构、依赖和构建过程. 实验涉及两个方面的内容:

1. 包依赖管理: Maven使用POM文件中的`<dependencies>` 元素来管理项目的依赖关系. 通过配置依赖项、Maven能够自动下载并集成项目所需的第三方库.
2. 仓库配置: Maven仓库是用来存储和管理构建过程中所需的库文件的地方. 实验中将学习如何卑职Maven仓库, 以便Maven能够正确地下载项目所需的依赖.

在正式开始试验之前, 需要先安装Maven工具.

### Maven Archetype

Archetype是Maven工程的模板工具包, 定义了要做的一类项目的基本模型或者架构. 通过`mvn archetype:generate` 命令, 开发人员可以很方便地将一类项目的最佳实现 应用 到自己的项目中. 在Maven项目中, 开发者可以通过archetype提供的范例快速入门并了解该项目的结构与特点. 一些常见的Archetype如下:

   * `maven-archetype-quickstart` : 默认的archetype, Maven工程项目的样例原型;
   * `maven-archetype-j2ee-simple` :简单的J2EE应用程序样例;
   * `maven-archetype-mojo` :Maven插件项目的示例样例;
   * `maven-archetype-webapp` :Maven的Webapp工程样例.

`mvn archetype:generate` 命令执行示例如下:

```Plain Text
mvn archetype:generate \
  -DgroupId=cc.synx \
  -DartifactId=my-project \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false
```
值得一提的是, 与Spring Boot项目框架最相近的Archetype是`maven-archetype-webapp` , Spring Initializr([https://start.spring.io/](https://start.spring.io/))执行与Archetype相同的操作, 而且对用户更加友好, 因此推荐大家直接使用Spring Initializr来创建.

### Setting.xml

`setting.xml` 文件是Maven的配置文件之一, 它用于配置Maven的全局设置, 包括仓库、代码、镜像、插件等. 这个文件位于Maven的安装目录下的`conf` 文件夹中, 或者在用户的Maven主目录下的`.m2` 文件夹中. 该文件是可选的, 如果用户没有在Maven中指定该文件的位置, Maven会使用默认的设置, 以下是`setting.xml` 中一些常见的配置选项:

   * `<localRepository/>`: 指定本地仓库的路径。默认情况下，Maven 会在用户主目录下的 `.m2` 文件夹中创建一个 `repository` 文件夹来存储下载的依赖。
   * `<mirrors/>` : 用于配置镜像, 可以提高构建速度并减轻官方仓库的负载. 镜像可以指向其他Maven仓库, 以加速依赖的下载.
   * `<proxies/>` : 用于配置代理, 网络环境需要使用代理来连接Maven中央仓库或其他远程仓库时, 可以在这里配置代理信息.
   * `<profiles/>` : 定义一系列的配置文件激活条件, 比如根据环境、操作系统等等条件来加载不同的配置文件.
   * `<servers/>` : 用于配置远程仓库的身份验证信息, 包括用户名、密码等. 这些信息在访问受保护的仓库时需要用到.

### Maven私有库

#### 软件组件

在软件工程中, "软件组件"通常指的是构成软件系统的独立并且可以重用的模块或部分. 这些组件可以是库、框架、模块、插件等, 用于实现特定的功能或者服务.

   * 库(library): 一个库是一组已经编写好并被打包成可重用形式的代码, 提供一组特定的功能. 开发者可以再起英语程序中使用这些库, 而不必重新实现相同的功能.
   * 框架(framework): 框架是一个更大范围的组件, 通常包含一组库、工具和标准, 用于帮助开发者构建特定类型的应用程序. 框架提供了应用程序的基本结构, 开发者通过扩展或配置框架来实现具体的业务逻辑.
   * 模块(module): 模块是软件系统中的一个独立单元, 具有特定的功能. 模块可以包含代码、数据、配置等等, 并且通常设计为可独立开发和测试的单元.
   * 插件(plugin): 插件是一种可动态加载到应用程序中以扩展器功能的组件. 插件通常是独立开发的, 可以根据需要添加到主应用程序中.

#### Maven仓库管理工具

Maven 仓库管理工具主要用于管理 Maven 仓库中的各种构件(如 JAR 文件、WAR文件等)和元数据。以下是一些常见的 Maven 仓库管理工具:

   * **Nexus Repository Manager**：Nexus 是由 Sonatype 提供的强大的仓库管理和仓库代理工具。它支持 Maven、Docker、npm、NuGet 等多种仓库格式。Nexus 有开源版本（Nexus Repository OSS）和专业版（Nexus Repository Pro）。截至 2024 年 1 月，Nexus 运行环境**仅**支持 **JDK 8**。
   * **JFrog Artifactory**：Artifactory 是由 JFrog 提供的仓库管理工具，支持 Maven、Gradle、Ivy、Docker、npm、NuGet 等多种仓库格式。Artifactory 有开源版（Artifactory OSS）和付费版本（Artifactory Pro、Artifactory Enterprise）。
   * **Apache Archiva**：Archiva 是 Apache 基金会提供的开源 Maven 仓库管理工具。它支持 Maven 仓库的基本功能，适用于小型项目和团队。
   * **JitPack**：JitPack 是一个基于 Git 的仓库管理服务，它可以将 GitHub 上的项目直接转换为 Maven 仓库。你可以通过 JitPack 将 GitHub 项目发布到 Maven 仓库，以便其他项目可以依赖这些构建产物。

#### Sonatype Nexus

Sonatype Nexus 是一个用于管理和组织软件构件(例如 JAR 包、WAR 包等)的开源仓库管理系统。它是一个用于构建、部署和管理软件组件的仓库管理器，主要用于帮助团队有效地共享和管理软件构件。Sonatype Nexus 中的一些常见术语包括:

   * **Repository（仓库）**：仓库是 Nexus 存储软件/组件的地方。Nexus 支持多种类型的仓库，包括 Maven、npm、NuGet、Docker 等。
   * **Proxy Repository（代理仓库）**：代理仓库是一个与远程仓库关联的本地缓存。当从远程仓库下载构件时，代理仓库会缓存这些构件，以便在将来的构建中快速访问。
   * **Group Repository（组合仓库）**：组合仓库允许将多个仓库组合在一起，并将它们看作一个单独的仓库。这使得在构建中引用多个仓库变得更加简单，同时也提供了对不同类型仓库的统一访问。
   * **Lifecycle Management（生命周期管理）**：Nexus 提供了丰富的生命周期管理功能，包括对构件的版本控制、审计、发布、存储等管理。

### 雪花算法

雪花算法(Snowflake Algorithm)是由Twitter开源的64位分布式ID生成算法。雪花算法的名称来源于其生成的唯一标识的形状，看起来像雪花的晶体结构。雪花算法的核心思想是利用一个64位的整数，其中包含了时间戳、数据中心的标识、机器的标识和一些序列号，以保证在分布式系统中生成的每个ID都是唯一的。

   * **符号位**：1位，通常为0。
   * **时间戳**：41位，毫秒级时间戳，通过时间戳来保证 ID 的递增性。
   * **机器ID**：10位，存储机器码，5位Data Center ID + 5位Worker ID，最多可标识集群内1024台机器。
   * **序列号**：12位，单台机器每毫秒可生成的 ID 数。当同一毫秒内生成的 ID 数量超过4096，会等待下一毫秒再生成。

雪花算法的优点是简单且高效，能够在分布式环境中生成唯一的ID。然而，雪花算法依赖于系统的时钟，如果系统时钟不稳定或发生回拨，可能会导致 ID 生成重复或不连续。在实际应用中，需要根据系统的特点和需求来选择合适的ID生成算法。

## 实验要求
* 要求学生掌握在 Maven 项目中管理依赖项的方式。
* 要求学生理解并熟练使用 Maven 命令执行 Spring Boot 项目构建打包。
* 要求学生学会安装和配置 Maven 仓库管理工具 Sonatype Nexus 并成功应用。

## 实验环境
* JDK17
* Maven 3.9
* Docker
* Ubuntu 22.04 LTS(用咱们之前git虚拟机就可以吧)
* Sonatype Nexus 3.x

## 实验步骤
### Maven安装
#### JDK17下载安装
Maven编译运行离不开JDK, 在安装Apache Maven之前, 需要先在系统中安装JDK, 并设置java环境变量. Maven 3.9+ 需要 JDK8及以上版本支持, 本节介绍在Ubuntu 22.04 LTS上安装JDK17.

1. 在Oracle官网或使用wget在当前目录获取`jdk-17_linux-x64_bin.tar.gz` 压缩包,然后解压后移至`/usr/local` 文件夹下.

输入一下指令

```Plain Text
# 下载JDK17
wget https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.tar.gz
# 查看
ls
# 解压
sudo tar -xzvf jdk-17_linux-x64_bin.tar.gz
# 再次查看
ls
# 转移到/usr/local/jdk-17
sudo mv jdk-17.0.12 /usr/local/jdk-17

# 查看是不是成功转移
cd /usr/local
ls
```