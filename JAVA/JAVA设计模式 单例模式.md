## 单例模式

有些类只提供一个对象

> a.构造器私有化，（不让外界new）
>
> b.在本类实例化一个对象，让外界可以获取到



### 1.饿汉式

```java
//方式一
public class Singe{
    private static final Single SINGLE=new Single();
    private Single(){}
    
    //必须为静态方法
    public static Single getSingle(){
        return SINGLE;
    }
    
}


//方式二
public class Singe1{
    public static final Single SINGLE=new Single();
    private Single(){}

}
```





### 2.懒汉式

```java
//存在线程安全问题
public class Singe1{
    private static Single SINGLE
    
    private Single(){}
    //必须为静态方法
    public static Single getSingle(){
        if(SINGLE==null)
            SINGLE=new Single();
        return SINGLE;
    }
   
}
```

