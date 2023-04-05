### Comparable

java.lang.Comparable

功能：实现Comparable接口，实现compareTo，this和obj比大小；==相当于在原有的类上直接添加上比较大小的功能，需要有源代码==

**返回值**

- 整数，说明this大

- 负数，说明this小

- 0，说明相等




Collections.sort(List),Arrays.sort() 自动调用compareTo方法。

```java
public class Person implements Comparable<Person>{
   
    .....

    @Override
    public int compareTo(Person o) {
        return getAge() - o.getAge();
    }
}

```





### Comparator

java.util。Comparator

应用场景

1.无法获得对象源码

2.类中本身已经实现了Comparable接口

==实现int compare(Object obj1,Object obj2)方法，通过Collections.sort(List,Comparator)调用==

```java
class personComparator implements Comparator<Person>{
    @Override
    public int compare(Person o1, Person o2) {
        return o1.getAge()-o2.getAge();
    }
}
```

