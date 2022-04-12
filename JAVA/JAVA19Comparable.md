### Comparable

java.lang.Comparable

功能：实现Comparable接口，实现compareTo，this和obj比大小；

==返回值==

整数，说明this大

负数，说明this小

0，说明相等



Arrays.sort() 自动调用compareTo方法。





### Comparator

java.util。Comparator

应用场景

1.无法获得对象源码

2.类中本身已经实现了Comparable接口

==实现int compare(Object obj1,Object obj2)方法==