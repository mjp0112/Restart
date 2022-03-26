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

```mysql
create index index_name on users(name);
#复合索引
create index index_name on users(name,phone);
```

#### 索引使用原则：

1.不过度索引

2.索引条件列(where后面频繁的条件适合索引)

3.索引散列值，如给性别加索引，意义不大