## 事务

数据库操作的最基本单元，逻辑上一组操作，要么都成功，有一个失败，都失败；



### 事务的四个特性

##### 原子性（Atomicity）

原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。

##### 一致性（Consistency）

事务前后数据的完整性必须保持一致。

##### 隔离性（Isolation）

事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务之间要相互隔离。

##### 持久性（Durability）

持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来即使数据库发生故障也不应该对其有任何影响







### 步骤

1.开启事务

```mysql
start transaction;

#或者
begin
```



2.执行具体事务操作

```mysql
update emp set ename='lucyatguigu' where eid=1
```



3.提交/回滚事务

rollback/commit

```mysql
可以在事务操作中设置保存点
savepoint [名称]

然后回退到暂存点
rollback to [暂存点名字]
```





### 读问题

#### 1.脏读

一个未提交事务，读取到另外一个数据库操作的数据



#### 2.不可重复读

一个未提交事务，读取到另外一个提交事务修改操作



#### 3.幻读

一个未提交事务，读取到另外一个提交事务添加操作



解决方法：设置mysql隔离级别？

set session transaction isolation level  

read uncommitted/ read committed/ repeatable read（==默认==）





**1、创建用户，授予权限**

**GRANT SELECT,INSERT,UPDATE,DELETE ON \*.\* TO 'zhang3'@'%' IDENTIFIED BY '123';**