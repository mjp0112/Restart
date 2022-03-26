

添加新表工资档位











#### 复制表

```mysql
#赋值表结构
create table newemp like emp;
#复制拷贝数据
create table newemp as (select * from emp);

#复制拷贝
create table newemp like emp;
insert into newemp select * from emp;

```

