

### 单行函数



1、通过select直接使用，不需要依赖表

 

\# mysql单行函数

SELECT POW(2,3)

SELECT SQRT(4)

 

SELECT LENGTH('atguigu')

 

SELECT CURDATE()

SELECT NOW()

 

SELECT ename ,CASE 

WHEN age>=50 THEN '事业顶峰'

WHEN age>=40 THEN '事业上升期'

WHEN age>=30 THEN '事业奋斗期'

ELSE '事业积累期'

END

FROM emp;

 

SELECT DATABASE()

SELECT VERSION()

SELECT USER()

SELECT PASSWORD("123")

SELECT MD5('123456')





