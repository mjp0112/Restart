## 自动增长

关键字：auto_increment,要求字段类型为int。

一般跟主键一起使用

```mysql
CREATE TABLE users (
   id INT PRIMARY KEY auto_increment,
   NAME VARCHAR(100)
)

insert into users values(null,'lucy');
```





## 索引

索引是一种单独的、物理的对数据库表中一列或多列的值进行排序的一种存储结构。

![image-20220405012808470](MySQL3自动增长、索引.assets/image-20220405012808470.png)

```mysql
create index index_name on users(name);
#复合索引
create index index_name on users(name,phone);
```

#### 索引使用原则：

1.不过度索引

2.索引条件列(where后面频繁的条件适合索引)

3.索引散列值，如给性别加索引，意义不大







### 索引分类

#### 主键索引

设定主键后自动建立，innodb为聚簇索引；（不允许为空）



#### 单值索引

一个表可以有多个单列索引



#### 唯一索引

索引列的值唯一，但允许有空值（null可以多个）



#### 复合索引

一个索引包含多个列

最左前缀原则：查询时，需要从左到右的依次才能利用利用索引；但mysql引擎会自动调整查询字段顺序以利用索引；



#### 全文索引(MyISAM才能用)





### 操作语句

```mysql
#查看索引
show index from t_user;

#建立索引
#普通索引
create index name_index on t_user(name)
#创建表是创建索引,无法自定义名字
create table t_user(id varchar(20) primary key, name varchar(20),key(name));

#唯一索引
create table t_user(id varchar(20) primary key, name varchar(20),unique(name));
#手动创建
create unique index t_unique on t_user2(name);

#复合索引
create table t_user(id varchar(20) primary key, name varchar(20),age int,key(name,age));
#手动创建
create index nameageindex on t_user(name,age);


```





### 索引数据结构

### [索引结构](MySQL索引结构-B+Tree.md)
