## 抽象类

```java
public abstract class Animal{
    public abstract void eat();
}
```

#### 特点

1.抽象方法所在的类，必须是抽象类；

2.子类继承抽象类时，必须实现抽象类中所有的抽象方法；

3.抽象类中可以有0-n个抽象方法或普通方法；

4.子类中有没事闲的抽象类，也只能变为抽象类；

5.抽象类不能实例化，但是有构造方法；



#### 应用

一般作为模板工具类

例子：

```java
abstract class Time{
    public abstract void code();
    
    public long getTime(){
        long start= System.currentTimeMillis();
        code;
        long end = System.currentTimeMillis();
        return end - start;
    }
   
}
```

 

## Debug

### 快捷键

F8 单步运行，逐行查看

![image-20220315162314423](JAVA16 抽象类.assets/image-20220315162314423.png)

F9 找下一个断点

![image-20220315162403132](JAVA16 抽象类.assets/image-20220315162403132.png)

F7 强制进入方法

![image-20220315162428734](JAVA16 抽象类.assets/image-20220315162428734.png)

Shift + F8 退出当前方法

![image-20220315162528724](JAVA16 抽象类.assets/image-20220315162528724.png)





