## 流程控制

### 1.顺序结构

程序按照顺序从上到下执行

1. 输出

   System.out.println()

   System.out.print()

2. 输入

​	用途：从命令窗口获取输入

```java
//输入工具
java.util.Scanner sc =new Scanner(System.in);
//获取
sc.nextInt();
sc.nextDobule();
sc.next();      //String.只能去空格前的内容
sc.nextLine();  // 一整行,但是读取到上面的空格会直接空行
```



### 2.分支和选择结构：

##### 单分支

语法：if(条件){代码块}

==当大括号中只有一行代码，大括号可以省略；但如果是变量的声明，不可以省略！因为变量的作用域没用==



##### 双分支

语法：if(条件1){代码块1} else{代码块2}

==当大括号中只有一行代码，大括号可以省略；但如果是变量的声明，不可以省略！因为变量的作用域没用==

##### 多分支

语法：if(条件1){代码块1} else if(条件2){代码块2} ... 





##### 选择结构

```java
switch(变量名){
    case 值1:
        代码块1;
        【break;】
    case 值2:
        代码块2;
        【break;】
    default:
        代码块;
        【break;】
        
}
```

==如果没有break，会直接穿透==

==swtich 支持所有整型和char，String（jdk1.7），枚举==





![img](Java05流程控制.assets/1001990-20161025144757265-1724289762.png)

### 例题

```java
1.  在JAVA中，以下程序的输出结果为（）。 
        boolean b1=true,b2=false;    
    
        if((b1=2>3) && (b2=5>0)){ //双目运算法与，前面为flase，直接不执行后面语句
            b1=true;
        }
        System.out.print("b1="+b1+";b2="+b2);





2. 在java中，运行下面的代码输出的结果是（）
		int x = 1;int y = 2;x += y+y; // x=5   y=2       2
		if(x < 5 && y <=4){
			System.out.println("1");
		}else if(x >= 5 || y>4){
			System.out.println("2");
		}else{
			System.out.println("error");
		}
3. 简述switch中可以使用哪些数据类型？
	byte short int char String(JDK1.7新增) 枚举


```



### 3.循环

#### while循环

语法:

```java
while（循环条件）{
    代码块
}
```

练习：

```java
//4. 求出1-100之间偶数和
int i=1,sum=0;
while(i<=100){
    if(i%2 == 0){
        sum +=i;
    }
    i++;
}
System.out.println(sum);


/* 5. 不断输入正数，直到输入-1为止(输入-1之外的数字时，提示输入有误，再次输入)
				不知道循环次数！循环次数是由用户决定的！
				*/
boolean flag = true;
Scanner sc =new Scanner(System.in);
while(flag){
    if(sc.nextInt()==-1){
        flag = false;
    }
}
```



#### for循环

```java
for(循环因子的声明;循环条件;循环因子值的改变){
    代码块
}
```

==确定循环次数用for，不确定用while==



#### do-while循环

```java
do{
    
}while(循环条件);

/*执行代码块，然后判断循环条件，如果条件成立，继续执行循环*/
```

==特点：==至少循环一次





### 4.跳转结构

#### 1.break

结束当前循环



#### 2.continue

跳出本次循环
