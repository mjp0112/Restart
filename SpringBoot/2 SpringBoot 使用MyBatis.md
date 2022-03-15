## SpringBoot 使用MyBatis



druid监控功能

http://localhost:8080/druid



druid常见问题

https://github.com/alibaba/druid/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98



### 1.pom文件中引入

引入mybatis

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.1</version>
</dependency>

<!--
由于我们项目使用的mysql，所以这里对jdbc进行引入
        -->
<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.22</version>
</dependency>
```



引入Druid

Druid是一个关系型数据库连接池，是阿里巴巴的一个开源项目，地址：https://github.com/alibaba/druid。Druid不但提供连接池的功能，还提供监控功能，可以实时查看数据库连接池和SQL查询的工作情况。

```xml
<!--
	要注意Druid的版本，老版本容易报错
-->

<dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.2.8</version>
</dependency>
```

同时在application.yml下进行配置：

```yml
server:
  port: 8080
  servlet:
    context-path: /web

spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/scott?serverTimezone=UTC&useUnicode=true&characterEncoding=utf8&useSSL=false&allowPublicKeyRetrieval=true
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver
    type: com.alibaba.druid.pool.DruidDataSource
    druid:
      #初始化大小
      initialSize: 5
      #最小值
      minIdle: 5
      #最大值
      maxActive: 20
      #最大等待时间，配置获取连接等待超时，时间单位都是毫秒ms
      maxWait: 60000
      #配置间隔多久才进行一次检测，检测需要关闭的空闲连接
      timeBetweenEvictionRunsMillis: 60000
      #配置一个连接在池中最小生存的时间
      minEvictableIdleTimeMillis: 300000
      validationQuery: SELECT 1 FROM DUAL
      testWhileIdle: true
      testOnBorrow: false
      testOnReturn: false
      poolPreparedStatements: true
      # 配置监控统计拦截的filters，去掉后监控界面sql无法统计，
      #'wall'用于防火墙，SpringBoot中没有log4j，我改成了log4j2
      filters: stat,wall,log4j2
      #最大PSCache连接
      maxPoolPreparedStatementPerConnectionSize: 20
      useGlobalDataSourceStat: true
      # 通过connectProperties属性来打开mergeSql功能；慢SQL记录
      connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
      # 配置StatFilter
      web-stat-filter:
        #默认为false，设置为true启动
        enabled: true
        url-pattern: "/*"
        exclusions: "*.js,*.gif,*.jpg,*.bmp,*.png,*.css,*.ico,/druid/*"
      #配置StatViewServlet
      stat-view-servlet:
        url-pattern: "/druid/*"
        #允许那些ip
        allow: 127.0.0.1
        login-username: admin
        login-password: 123456
        #禁止那些ip
        deny: 192.168.1.102
        #是否可以重置
        reset-enable: true
        #启用
        enabled: true
```

### 2.实现步骤

创建库表

```sql
//这是Oracle的语句，sql不用引号

CREATE TABLE "SCOTT"."STUDENT" (
    "SNO" VARCHAR2(3 BYTE) NOT NULL ,
    "SNAME" VARCHAR2(9 BYTE) NOT NULL ,
    "SSEX" CHAR(2 BYTE) NOT NULL 
);

INSERT INTO "SCOTT"."STUDENT" VALUES ('001', 'KangKang', 'M ');
INSERT INTO "SCOTT"."STUDENT" VALUES ('002', 'Mike', 'M ');
INSERT INTO "SCOTT"."STUDENT" VALUES ('003', 'Jane', 'F ');

```



创建实体类Bean：

```java
public class Student implements Serializable{
    private static final long serialVersionUID = -339516038496531943L;
    private String sno;
    private String name;
    private String sex;
    // get,set略
}
```



创建包含基本CRUD的Mapper类：

简单的语句只需要使用@Insert、@Update、@Delete、@Select这4个注解即可，动态SQL语句需要使用@InsertProvider、@UpdateProvider、@DeleteProvider、@SelectProvider等注解。具体可参考MyBatis官方文档：http://www.mybatis.org/mybatis-3/zh/java-api.html。

若是需要使用xml的方式需要在application.yml中进行额外配置：

```xml
mybatis:
  # type-aliases扫描路径
  # type-aliases-package:
  # mapper xml实现扫描路径
  mapper-locations: classpath:mapper/*.xml
  property:
    order: BEFORE
```



StudentMapper:

```java
@Component
@Mapper
public interface StudentMapper {
    @Insert("insert into student(sno,sname,ssex) values(#{sno},#{name},#{sex})")
    int add(Student student);
    
    @Update("update student set sname=#{name},ssex=#{sex} where sno=#{sno}")
    int update(Student student);
    
    @Delete("delete from student where sno=#{sno}")
    int deleteBysno(String sno);
    
    @Select("select * from student where sno=#{sno}")
    @Results(id = "student",value= {
        @Result(property = "sno", column = "sno", javaType = String.class),
        @Result(property = "name", column = "sname", javaType = String.class),
        @Result(property = "sex", column = "ssex", javaType = String.class)
    })
    Student queryStudentBySno(String sno);
```





Service:

```java
public interface StudentService {
    int add(Student student);
    int update(Student student);
    int deleteBysno(String sno);
    Student queryStudentBySno(String sno);
}
```



Service实现类：

```java
@Service("studentService")
public class StudentServiceImp implements StudentService{
    @Autowired
    private StudentMapper studentMapper;
    
    @Override
    public int add(Student student) {
        return this.studentMapper.add(student);
    }
    
    @Override
    public int update(Student student) {
        return this.studentMapper.update(student);
    }
    
    @Override
    public int deleteBysno(String sno) {
        return this.studentMapper.deleteBysno(sno);
    }
    
    @Override
    public Student queryStudentBySno(String sno) {
        return this.studentMapper.queryStudentBySno(sno);
    }
}
```



Controller:

```java
@RestController
public class TestController {

    @Autowired
    private StudentService studentService;
    
    @RequestMapping( value = "/querystudent", method = RequestMethod.GET)
    public Student queryStudentBySno(String sno) {
        return this.studentService.queryStudentBySno(sno);
    }
}
```



==使用JdbcTemplates==

https://mrbird.cc/Spring-Boot%20JdbcTemplate.html
