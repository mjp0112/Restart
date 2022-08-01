## SpringBoot基础

==为什么使用SpringBoot==

虽然Spring的组件代码是轻量级的，但它的配置却是重量级的。一开始，Spring用XML配置，而且是很多XML配置。Spring 2.5引入了基于注解的组件扫描，这消除了大量针对应用程序自身组件的显式XML配置。Spring 3.0引入了基于Java的配置，这是一种类型安全的可重构配置方式，可以代替XML。

所有这些配置都代表了开发时的损耗。因为在思考Spring特性配置和解决业务问题之间需要进行思维切换，所以编写配置挤占了编写应用程序逻辑的时间。和所有框架一样，Spring实用，但与此同时它要求的回报也不少。

除此之外，项目的依赖管理也是一件耗时耗力的事情。在环境搭建时，需要分析要导入哪些库的坐标，而且还需要分析导入与之有依赖关系的其他库的坐标，一旦选错了依赖的版本，随之而来的不兼容问题就会严重阻碍项目的开发进度。



### 1.快速创建

在IDEA的New Project中有自带的Spring initialize可以进行快速创建，注意勾选Spring Web选项才会开启默认端口号为8080的web服务。



Group: 是公司或者组织的名称，是一种命名空间的概念，比如https://pdai.tech网站，那么group可以是tech.pdai

Artifat: 当前项目的唯一标识

Version: 项目的版本号，一般xx-SNAPSHOT表示非稳定版



在开启服务的入口类中加入以下为最基础的一个服务请求，当然要加上@RestController 注解。

```java
    @RequestMapping("/")
    String index() {
        return "hello spring boot";
    }
```



#### Pom文件

==spring-boot-starter-parent==

spring-boot-starter-parent指定了当前项目为一个Spring Boot项目，它提供了诸多的默认Maven依赖，具体可查看目录[D:\m2\repository\org\springframework\boot\spring-boot-dependencies\1.5.9.RELEASE](file:///D:/m2/repository/org/springframework/boot/spring-boot-dependencies/1.5.9.RELEASE)下的spring-boot-dependencies-1.5.9.RELEASE.pom文件，这里仅截取一小部分：

```
<properties>
...	
    <spring-security.version>4.2.3.RELEASE</spring-security.version>
    <spring-security-jwt.version>1.0.8.RELEASE</spring-security-jwt.version>
    <spring-security-oauth.version>2.0.14.RELEASE</spring-security-oauth.version>
    <spring-session.version>1.3.1.RELEASE</spring-session.version>
    <spring-social.version>1.1.4.RELEASE</spring-social.version>
    <spring-social-facebook.version>2.0.3.RELEASE</spring-social-facebook.version>
    <spring-social-linkedin.version>1.0.2.RELEASE</spring-social-linkedin.version>
    <spring-social-twitter.version>1.1.2.RELEASE</spring-social-twitter.version>
    <spring-ws.version>2.4.2.RELEASE</spring-ws.version>
    <sqlite-jdbc.version>3.15.1</sqlite-jdbc.version>
    <statsd-client.version>3.1.0</statsd-client.version>
    <sun-mail.version>${javax-mail.version}</sun-mail.version>
    <thymeleaf.version>2.1.6.RELEASE</thymeleaf.version>
    <thymeleaf-extras-springsecurity4.version>2.1.3.RELEASE</thymeleaf-extras-springsecurity4.version>
    <thymeleaf-extras-conditionalcomments.version>2.1.2.RELEASE</thymeleaf-extras-conditionalcomments.version>
    <thymeleaf-layout-dialect.version>1.4.0</thymeleaf-layout-dialect.version>
    <thymeleaf-extras-data-attribute.version>1.3</thymeleaf-extras-data-attribute.version>
    <thymeleaf-extras-java8time.version>2.1.0.RELEASE</thymeleaf-extras-java8time.version>
    <tomcat.version>8.5.23</tomcat.version>
...    
</properties>
```

需要说明的是，并非所有在`<properties>`标签中配置了版本号的依赖都有被启用，其启用与否取决于您是否配置了相应的starter。比如tomcat这个依赖就是spring-boot-starter-web的传递性依赖（下面将会描述到）。

当然，我们可以手动改变这些依赖的版本。比如我想把thymeleaf的版本改为3.0.0.RELEASE，我们可以在pom.xml中进行如下配置：

```
<properties>
   <thymeleaf.version>3.0.0.RELEASE</thymeleaf.version>
</properties>
```



==替换模块中的组件==

我们可以在模块中替换掉我们的组件，如spring-boot-starter-web中，我们想启用jetty替换掉tomcat可以如下：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```



### 2.基础配置

##### 1.启动时默认图案可以更改,默认图案如下

```xml
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.5.9.RELEASE)
```

在src/main/resource目录下新建banner.txt，在该文本文档中添加即可更改。ASCII图案可通过网站http://www.network-science.de/ascii/一键生成。



banner也可以通过以下方式进行关闭。

```java
public static void main(String[] args) {
    SpringApplication app = new SpringApplication(DemoApplication.class);
    app.setBannerMode(Mode.OFF);
    app.run(args);
}
```

==为什么Mode类为接口可以直接使用==

==enum枚举类==



##### 2.获取application.properties中属性

Spring Boot允许我们在application.properties下自定义一些属性，比如：

```pro
mrbird.blog.name=mrbird's blog
mrbird.blog.title=Spring Boot
```

定义一个BlogBean Bean，通过`@Value("${属性名}")`来加载配置文件中的属性值：

```java
public class BlogBean {

    @Value("${mrbird.blog.name}")
    private String name;

    @Value("${mrbird.blog.title}")
    private String title;

    //..get、set方法	
}
```

编写IndexController，注入该Bean类：

```java
@RestController
public class IndexController {
    @Autowired
    private BlogBean blogProperties;
    
    @RequestMapping("/")
    String index() {
        return blogProperties.getName()+"——"+blogProperties.getTitle();
    }
}
```



==另外一种获取配置方式==

在Bean类前的注解中加入属性的前缀，即@ConfigurationProperties(prefix = "mrbird.blog");

并且该方法需要在入口类中加入注解来开启配置，@EnableConfigurationProperties({ConfigBean.class})。



##### 3.自定义配置文件

注解`@PropertySource("classpath:test.properties")`指明了使用哪个配置文件。要使用该配置Bean，同样也需要在入口类里使用注解`@EnableConfigurationProperties({TestConfigBean.class})`来启用该配置。

```java
@Configuration
@ConfigurationProperties(prefix="test")
@PropertySource("classpath:test.properties")
@Component
public class TestConfigBean {
    private String name;
    private int age;
    // get,set略
}
```

==#通过命令行来修改配置==

在运行Spring Boot jar文件时，可以使用命令`java -jar xxx.jar --server.port=8081`来改变端口的值。这条命令等价于我们手动到application.properties中修改（如果没有这条属性的话就添加）server.port属性的值为8081。

如果不想项目的配置被命令行修改，可以在入口文件的main方法中进行如下设置：setAddCommandLineProperties(false);



##### 4.开发环境

Profile用来针对不同的环境下使用不同的配置文件，多环境配置文件必须以`application-{profile}.properties`的格式命，其中`{profile}`为环境标识。比如定义两个配置文件：

- application-dev.properties：开发环境

  ```
  server.port=8080
  ```

- application-prod.properties：生产环境

  ```
  server.port=8081
  ```

至于哪个具体的配置文件会被加载，需要在application.properties文件中通过`spring.profiles.active`属性来设置，其值对应`{profile}`值。

如：`spring.profiles.active=dev`就会加载application-dev.properties配置文件内容。可以在运行jar文件的时候使用命令`java -jar xxx.jar --spring.profiles.active={profile}`切换不同的环境配置。





