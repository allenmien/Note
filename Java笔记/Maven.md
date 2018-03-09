# Maven

## Maven插件

每个任务对应了一个插件目标（goal），每个插件会有一个或者多个目标，例如maven-compiler-plugin  compile目标用来编译位于src/main/java/目录下的主源码，testCompile目标用来编译位于src/test/java/目录下的测试源码。

Maven官方有两个插件列表：

> 第一个列表的GroupId为org.apache.maven.plugins，这里的插件最为成熟，具体地址为：http://maven.apache.org/plugins/index.html。
>
> 第二个列表的GroupId为org.codehaus.mojo，这里的插件没有那么核心，但也有不少十分有用，其地址为：http://mojo.codehaus.org/plugins.html

## Maven仓库

比如，工程中需要依赖spring-core这个jar包，在pom.xml中声明之后，maven会首先在本地仓库中找，如果找到了很好办，自动引入工程的依赖lib库即可。找不到，去远程仓库去下载。

maven在安装的时候，自带的默认中央仓库地址为http://repo1.maven.org/maven2/，此仓库由Maven社区管理，包含了绝大多数流行的开源Java构件。

除此之外，还有很多各具特色的公共仓库，如果需要都可以在网上找到，比如Apache Snapshots仓库，包含了来自于Apache软件基金会的快照版本。

一般来讲，公司都会通过自己的私有服务器在局域网内架设一个仓库代理。**私服可以看作一种特殊的远程仓库**，代理广域网上的远程仓库，供局域网内的Maven用户使用。当Maven需要下载构件的时候，先从私服请求，如果私服上不存在该构件，则从外部的远程仓库下载，缓存在私服上之后，再为Maven的下载请求提供服务。

![img](http://img.blog.csdn.net/20160522144525838?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

Maven私服有很多好处，可以把公司的私有jar包，以及无法从外部仓库下载到的构件上传到私服上，供公司内部使用；

当前主流的maven私服有Apache的Archiva、JFrog的Artifactory以及Sonatype的**Nexus**。

上面提到的中央仓库、中央仓库的镜像仓库、其他公共仓库、私服都属于远程仓库的范畴。

## setting.xml配置文件

maven的配置文件settings.xml存在于两个地方：

> 1.安装的地方：${M2_HOME}/conf/settings.xml
>
> maven安装环境变量的位置：(M2_HOME: E:\maven\apache-maven-3.5.0-bin\apache-maven-3.5.0)
>
> 2.用户的目录：${user.home}/.m2/settings.xml（C:\Users\user\.m2\settings.xml）

前者又被叫做全局配置，对操作系统的所有使用者生效；后者被称为用户配置，只对当前操作系统的使用者生效。如果两者都存在，它们的内容将被合并，并且用户范围的settings.xml会覆盖全局的settings.xml。

## idea新建一个Maven文件

- 创建新项目
- Maven
- GroupId..
- 完成



