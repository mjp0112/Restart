### 1.Lambda表达式

使用了函数式编程思想，只关注结果，不关心过程；简化代码编写。

```java
/*
1.复制小括号
2.写死右箭头
3.落地大括号
*/

Runnable r = ()->{ System.out.println(Thread.currentThread().getName());};

```

#### 1.1 使用方式

1.接口中只能有一个方法；

2.把只有一个普通方法的接口，称为函数式接口，注解：@FunctionalInterface ；==可以有default方法和static方法==



#### 1.2 java中接口

1.可以有default实现方法

2.可以有static方法





.forEach((e)->{

});



### 类