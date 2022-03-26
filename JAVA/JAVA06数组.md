## JAVA06 数组

数组：用于存储数据的长度固定的容器，保证多个数据的数据类型要一致（有序）。



### 数组的使用

#### 数组的定义

##### 1.动态初始化

```java
语法:
	a.数组的声明
        数据类型[] 数组名;
		int[]  arrs;
	b.数组空间的开辟
        数组名 = new 数据类型[数组长度];
		arrs = new int[5];

		System.out.println(arrs); //打印的数组地址
```



##### 2.静态初始化

```java
数据类型[] 数据名 = new 数据类型[]{元素1，元素2....};

数据类型[] 数据名 = {元素1，元素2....}; //不允许拆开声明和初始化
```





#### 数组的操作

##### 1.存值

```java
数组名[索引]  =  元素值;
//元素值要类型匹配
```



##### 2.取值

```java
数组名[索引]
```



#### 数组的特性

##### 1.数组存在默认值

​	整型：0；

​	浮点型：0.0

​	String：null

​	boolean：false

​	char：空白符



#### 异常总结

越界 ArraysIndexOUtOfBoundException

空指针 NullPointerException



#### 数组遍历

数组长度：==数组名.length==



#### 数组基操

扩容

```java
int arr = new int[5];
int newA = new int[arr.length+1];
for(int i=0;i<arr.length;i++){
    newA[i] = arr[i];
}
arr = newA;

```



反转

```java
//方法一。新建一个数组，然后倒序赋值

//方法而。首尾交换

```



数组的查找

1.顺序查找

2.二分查找









## 对象数组

对象数组的技术要求和普通数组一样

==数组默认存储的对象（对象的地址）==
