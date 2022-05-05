### 静态代理模式

真实对象和代理对象同时实现同一个接口，代理对象要代理真实对象，即将真实对象作为传参。

```java
public class staticProxy {
    public static void main(String[] args) {
        You you = new You();
        Company c = new Company(you);
        c.HappyMarry();
    }

}

//实现的接口
interface Marry{
    void HappyMarry();
}

//真实对象
class You implements Marry{
    //首先，接口中所有方法默认都是public，
    // 至于为什么要是public，原因在于如果不是public，
    // 那么只能在同个包下被实现，可访问权限就降低很多了，
    // 那么在实现类中，实现的类相当于子类，子类的访问权限是不能比父类小的。
    public void HappyMarry(){
        System.out.println("我要结婚了");
    }
}
//代理对象
class Company implements Marry{
    private Marry target;

    public Company(Marry target){
        this.target = target;
    }

    public void HappyMarry(){
        System.out.println("结婚");
        target.HappyMarry();
        System.out.println("收钱");
    }

}

```
