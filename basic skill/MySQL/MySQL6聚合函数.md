### 查询操作1

#### 语句顺序：

select 字段1，字段2... from 表名称1，表名称2....
where 条件
group by 字段 having 条件表达式(筛选条件)
order by 字段 [asc|desc]
limit m,n





#### 去重操作 distinct：

```mysql
select distinct ename from emp 
```

==mysql可以存储空格字符==





#### 模糊查询 like:

通配字符%

```mysql
#查询名字中含有a的名字，
select * from emp where ename like '%a%'
```

==%写在左边，索引会失效？==



```mysql
#查询第一个字母是m，后面是三个字母
select * from emp where ename like 'm___'
```





#### 排序操作

order by

```mysql
#降序，如果不填写desc，默认为asc
select * from emp order by eid desc
```





#### 查询范围

##### 查询年龄大于等于20小于等于50；

where 写法

```mysql
select * from emp where age>=20 and age<=50
```

between

```mysql
select * from emp where age between 20 and 50
```



##### 查询年龄是20 40 60；

```mysql
select * from emp where age in(20,40,60)
```







### 查询操作2

#### 分页操作 limit

limit分页操作只能在mysql中，不是标准sql；

> limit 后面有两个参数
>
> 第一个参数 查询数据开始位置
>
> ​	公式：(当前页-1)*每页显示记录数
>
> 第二个参数 每页显示多少条记录

```mysql
select * from emp limit 0,3
```



### mysql聚合函数

#### 1.count()统计功能

```mysql
#查询表中有多少记录
select count(*) from emp

#查询年龄大于40的人员数量
select count(*) num from emp where age>40
```



#### 2.sum()求和

```mysql
#求所有人年龄总和
select sum(age) from emp 
```





#### 3.avg()平均数

默认小数点后四位

```mysql
#平均年龄
select avg(age) from emp 

#用cast转换位数
select CAST(avg(age) as DECIMAL(10,1)) from emp 
```





#### 4.max()最大值

```mysql
#求最大值
select max(age) from emp 
```





#### 5.min()最小值

```mysql
#求最小值
select min(age) from emp 
```





### 分组查询

#### 1.group by

求每个部门中的人数

==在标准sql中，group by后出现的字段，一定要在select后出现==

```mysql
#统计各部门中有多少个员工
select edid deptid,count(*) num from emp group by edid
```



#### 2.exists

查询部门信息，该部门必须有员工

```mysql
select * from dept
where exits(select * from emp where dept.did=dept.edid)
```





#### 3.复制表

```mysql
#赋值表结构
create table newemp like emp;
#复制拷贝数据
create table newemp as (select * from emp);

#复制拷贝
create table newemp like emp;
insert into newemp select * from emp;

```

