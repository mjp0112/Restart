## 枚举类

一个类对外提供固定个数的对象



### JDK1.5之前

构造器私有化

```java
publci class Gender{
    private static fianl Gender BOY = new Gener();
    private static final Gender GIRL = new Gender();
    private Gender(){}
    public static Gender getBoy(){
        return BOY;
    }
     public static Gender getGirl(){
        return GIRL;
    }
    
    
}
```



### JDK1.5之后

引用了关键字enum,通过该关键字创建的类就是枚举类

```java
public enum Gender2{
    //默认有私有构造器
    //创建了两个对象
    BOY;
    GIRL;
    
    Boy("男孩");
}
```

#### 特点：

1.默认有私有构造器

2.一个名称对应一个对象，且对象的声明在第一行；

3.默认使用无参构造器

4.也可以在对象名后主动使用有参构造



#### 枚举类的实现父类和实现接口



##### 枚举类存在一个默认的父类Enum

两个获取名称方法：

name(); 不可以重写，final修饰

toString(); 可以重写，不重写返回name()



ordinal()； 返回该实例位置/角标

static values(); 返回该枚举类所有常量对象 

static valueOf(String name); 返回改名字的常量对象； 





#### 实现接口

implements 接口名

##### 特殊点：

枚举类提供的对象个数确定，由于对象确定，可以直接在对象上对方法进行扩展

```java
常量对象("name"){
    实现方法一;
    ....
}
```

当当前枚举类中的所有常量对象都对抽象方法进行了实现，可以删除公有的方法；