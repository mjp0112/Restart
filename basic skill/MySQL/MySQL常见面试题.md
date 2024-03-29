### 1.索引是什么

索引是对数据库表中一列或多列的值进行排序的一种结构，使用索引可快速访问数据库表中的特定信息。如果想按特定职员的姓来查找他或她，则与在表中搜索所有的行相比，索引有助于更快地获取信息。简而言之，数据库索引是排好序的[数据结构](https://baike.baidu.com/item/数据结构/1450?fromModule=lemma_inlink)。



### 2.索引类型

#### 按逻辑区分

**1.普通索引**

普通索引是 MySQL 中最基本的索引类型，它没有任何限制，唯一任务就是加快系统对数据的访问速度。

普通索引允许在定义索引的列中插入重复值和空值。

> CREATE INDEX index_id ON tb_student(id);



**2.唯一索引**

与普通索引类似，不同的是创建唯一性索引的==目的不是为了提高访问速度，而是为了避免数据出现重复。==

唯一索引列的值必须唯一，允许有空值。如果是组合索引，则列值的组合必须唯一。

> CREATE UNIQUE INDEX index_id ON tb_student(id);



**3.主键索引**

顾名思义，主键索引就是专门为主键字段创建的索引，也属于索引的一种。

主键索引是一种特殊的唯一索引，不允许值重复或者值为空。

创建主键索引通常使用 PRIMARY KEY 关键字。不能使用 CREATE INDEX 语句创建主键索引。



**4.空间索引**

空间索引是对空间数据类型的字段建立的索引，使用 SPATIAL 关键字进行扩展。

创建空间索引的列必须将其声明为 NOT NULL，空间索引只能在存储引擎为 MyISAM 的表中创建。

空间索引主要用于地理空间数据类型 GEOMETRY。对于初学者来说，这类索引很少会用到



**5.全文索引**

全文索引主要用来查找文本中的关键字，只能在 CHAR、VARCHAR 或 TEXT 类型的列上创建。在 MySQL 中只有 MyISAM 存储引擎支持全文索引。

全文索引允许在索引列中插入重复值和空值。

不过对于大容量的数据表，生成全文索引非常消耗时间和硬盘空间。

创建全文索引使用 FULLTEXT 关键字。



#### 实际使用区分

1、单列索引

2、多列索引