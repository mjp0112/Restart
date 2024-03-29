## 数据表操作



### 基本语句



#### 表基本操作

```sql
#创建表
create table 表名称(
    字段名称1 字段类型,
    字段名称2 字段类型,
    ...
);

create table t_user(id int, name varchar(100));



#展示表
show tables;
show tables from 数据库库名;

#查看表结构
desc 表名;

#删表
drop table 表名;   ###不能回滚



```



#### 修改表结构操作

```mysql
#（1）重命名表
ALTER TABLE t_stu RENAME students

#（2）增加一列
ALTER TABLE students ADD newcolumn INT
ALTER TABLE students ADD newcolumn1 INT AFTER id
ALTER TABLE students ADD newcolumn2 INT FIRST

#（3）删除列
ALTER TABLE students DROP newcolumn2;

#（4）修改列类型
ALTER TABLE students MODIFY newcolumn VARCHAR(100);

#（5）修改列名等
ALTER TABLE students CHANGE newcolumn address VARCHAR(100);
```



#### 增删改查

#### 新增语句

```mysql
#插入语句
insert into t_user(id,name) values(1,'lucy');
#不写属性时，默认都加上
insert into t_user values(1,'lucy');

#creatime 类型为timestamp，birthday 为datetime
INSERT INTO
 students(id,NAME,gender,salary,birthday,createtime)
 VALUES(1,'张三','男',999,'2020-11-11',NULL)
 
 #添加一条记录时，可以写成value
 insert into t_user(id,name) value(1,'lucy');
 #values可以添加多条记录
insert into t_user(id,name) values(1,'lucy'),(,'lucy'),(3,'lucy');


```



#### 修改语句

```mysql
update 表名称 set 字段名称=值，字段名称=值 where....

#修改
update t_user set name='lucyaaa' where id =1;    

#修改所有人的年龄+10
update t_user set age=age+10;
    
```



#### 删除语句

delete关键字(DML语句）

```mysql

#删除
delete from t_uer where id=2;
```

truncate关键字（DDL语句）

删除表之后，在创建相同结构的表



#### 查询操作

```mysql
#查
select * from t_user;
#语法
select 字段1，字段2... from 表名称1，表名称2....
where 条件
group by 字段 having 条件表达式(筛选条件)
order by 字段 [asc|desc]
limit m,n


#起别名 as可以省略
select sid as id,sname as name from stu

#查询id大于2并且年纪大于30的人
select sid,sname,age,address from stu s where sid>2 and age>30
```

飘符号（''）：变小写？







## 约束

限制条件：



#### 主键约束

创建表，把一些字段作为主键，这个字段非空、唯一。

```mysql
#单主
CREATE TABLE users (
   id INT PRIMARY KEY,
   NAME VARCHAR(100)
)
#复合主键
CREATE TABLE book (
  id INT,
  bname VARCHAR(100),
  bno INT,
 
  PRIMARY KEY(id,bno)
)


```





#### 唯一约束

关键值不重复 unique



#### 非空约束

not null



#### 缺省约束

添加指定默认值

```mysql
CREATE TABLE users (
   id INT PRIMARY KEY,
   NAME VARCHAR(100) default 'lucy'
)
```





#### 外键约束
