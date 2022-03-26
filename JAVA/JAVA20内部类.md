### 分类

成员内部类

匿名内部类





### 匿名内部类

实现类没有名称

#### ==优缺点==

优点：省略创建类

缺点：复用性差



#### e.g.

```java
public class Demo{
    public static void main(String[] args){
        
        //实现接口的匿名内部类
        Comparator comparator = new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                return 0;
            }
        };
        
        new Car(){
          //Car的子类类  
        };
        
        new AbstraCar(){
            //实现所有抽象方法
        };
    }
}
```



### 成员内部类

编译后文件：外部类名$内部类名.class

==类的五大成员都可以有，但不可以存在静态资源；==

> 使用：
>
> 1.内部类使用外部类资源！
>
> ​	可以直接访问
>
> 2.外部类使用内部类资源
>
> ​	要创建对象
>
> 3.其他类使用该类内部类
>
> ​	Outher.Inner inner = new Outher().new Inner()；



#### 冲突

内部类的属性命名与外部类属性名冲突；

```java
this.内容; //但前对象
外部类名.this.内容; //访问外部类属性
```





### 静态成员内部类

==类的五大成员都可以存在，包括静态资源==

[修饰符] static class 类名{}

#### 注意：

1.不依赖于外部类对象，所以无法直接使用外部类的实例方法和实例属性，需要实例化；

2.外部类使用内部资源：静态资源，类名使用；非静态，实例化使用。

3.其他类使用的话，

​	Outher.Inner inner = new Outher.Inner()；





### 方法内部类/局部内部类

```java
public class Demo{
	public void function(){
        //class文件为 Demo$1Inner3.class
        //如果局部内部类使用了当前局部变量，默认该变量为常量
        class Inner3{
            
        }
        
        
    }
}
```

成员：五大成员都可以，但不包括静态资源；

修饰符：不可能使用访问修饰符

> 使用：
>
> 1.方法内部类使用外部类资源！
>
> ​	可以直接访问
>
> 2.外部类使用内部类资源
>
> ​	无法使用
>
> 3.其他类使用该类内部类
>
> ​	无法访问

冲突：(不懂)

![image-20220321041127350](JAVA20内部类.assets/image-20220321041127350.png)

