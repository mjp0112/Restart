Apache软件基金会组织下软件工具



### 为啥要用maven

1.框架封装程度越来越高，使用的jar包越来越多；

2.jar包来源：不安全，繁琐

3.jar包之间的依赖关系错综复杂



### 构建

![image-20220404064219998](Maven1.assets/image-20220404064219998.png)





### 配置：

1.conf包下setting文件

配置本地仓库

```xml
<!-- localRepository
| The path to the local repository maven will use to store artifacts.
|
| Default: ${user.home}/.m2/repository
<localRepository>/path/to/local/repo</localRepository>
-->
<localRepository>D:\maven-rep1026</localRepository>
```

本地仓库这个目录，我们手动创建一个空的目录即可。

<span style="color:blue;font-weight:bold;">记住</span>：一定要把localRepository标签<span style="color:blue;font-weight:bold;">从注释中拿出来</span>。

<span style="color:blue;font-weight:bold;">注意</span>：本地仓库本身也需要使用一个<span style="color:blue;font-weight:bold;">非中文、没有空格</span>的目录。



配置阿里云镜像仓库：

```xml
<!-- 阿里云仓库 -->
        <mirror>
            <id>alimaven</id>
            <mirrorOf>central</mirrorOf>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
        </mirror>
    
        <!-- 中央仓库1 -->
        <mirror>
            <id>repo1</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo1.maven.org/maven2/</url>
        </mirror>
    
        <!-- 中央仓库2 -->
        <mirror>
            <id>repo2</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo2.maven.org/maven2/</url>
        </mirror>
```

配置一下JDK版本

```xml
<profile>
      <id>jdk-1.8</id>

      <activation>
        <activeByDefalut>true</activeByDefalut>
		<jdk>1.8</jdk>
      </activation>

      <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
      </properties>
</profile>
```



2.环境配置

1.配置JAVA_HOME(依赖java)

2.配置MAVEN_HOME

```java
MAVEN_HOME: maven bin目录上一层目录;
PATH : %MAVEN_HOME%\bin
```









