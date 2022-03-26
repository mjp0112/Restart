### 可变参数

1.可变参数就当做数组去使用；

2.可变参数必须是参数列表的最后一个；

```java
public class Demo{
    public static void main(String[] args){
        int[] arr = {1,32,321,12};
        Algorithm a = new Algorithm();
        System.out.println(a.sum(arr ));
    }
}

class Algorithm{
    public int sum(String a,int...abc){
        int sum = 0;
        for(int i=0;i<abc.lengthl;i++){
            sum += i;
        }
        return sum;
    }
}

```





### 方法重载

1 同一个类中，方法名可以相同

2 形参列表必须不同(==类型个数、类型的顺序、类型==)

3 与返回值无关

4 与访问修饰符无关 

==注：==重载方法调用的时候，先找完全符合的，找不到完全符合的，找兼容的！如果有多个兼容的就报错！



#### 参数传递机制

形参：在方法定义位置，指定的形式参数

实参：实际传入的参数

> ==总结==
>
> 引用数据类型，传递参数是，传递的是地址值，修改数值，会修改原值；
>
> 基本数据类型，传递的是实际数值；
