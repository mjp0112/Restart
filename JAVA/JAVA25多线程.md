## 多线程



#### 线程简介

串行：依次执行

并发：多个人一起操作同一个资源

<img src="JAVA25多线程.assets/image-20220413143830485.png" alt="image-20220413143830485" style="zoom:50%;" />

并行：多个操作同时执行（理想状态下的并发）

<img src="JAVA25多线程.assets/image-20220413143900773.png" alt="image-20220413143900773" style="zoom:80%;" />



多进程：执行程序的一次执行过程，==是系统资源分配的单位==。

多线程：一个进程含有若干个线程，==线程是CPU调度和执行的单位==。

##### 线程和进程总结

1.进程虽然是相互独立的，但可以用socket或http协议进行通信；

2.进程拥有共享的系统资源

3.进程较重，因为创建进程需要操作系统分配资源；

4.线程存在于进程中，是进程的一个子集



#### 多线程优势与劣势

优势：

1.提高吞吐率，并发执行；

2.提高响应率

3.提高CPU资源利用率

劣势：

1.安全问题，共享数据没有正确并发访问控制，会产生问题；

2.线程活性问题，资源稀缺导致线程一直处于非Runnable；

​	a.死锁；b.锁死；c.活锁；d.饥饿；

3.上下文切换，线程之间切换需要资源开销

4.可靠性，可能有一个线程导致cpu终止。



#### 六大状态

![image-20220414025648917](JAVA25多线程.assets/image-20220414025648917.png)



##### New

处于NEW状态的线程此时尚未启动。这⾥的尚未启动指的是还没调⽤Thread实例 

的start()⽅法。

==问：==反复调⽤同⼀个线程的start()⽅法是否可⾏？ 

==答：==不可行，因为start开始会判断threadStatus!=0,如果不为0，抛出异常



##### Runnable

Java线程的**RUNNABLE**状态其实是包括了传统操作系统线程的**ready**和**running**两个状态的。



##### BLOCKED

阻塞状态。处于BLOCKED状态的线程正等待锁的释放以进⼊同步区。



##### Waiting

等待状态。处于等待状态的线程变成RUNNABLE状态需要其他线程唤醒。



##### Timed_waiting

超时等待状态。线程等待⼀个具体的时间，时间到后会被⾃动唤醒。 

以下⽅法会使线程进⼊超时等待状态：

- Thread.sleep(long millis)：使当前线程睡眠指定时间；
- Object.wait(long timeout)：线程休眠指定时间，等待期间可以通过notify()/notifyAll()唤醒；
- Thread.join(long millis)：等待当前线程最多执⾏millis毫秒，如果millis为0，则 会⼀直执⾏；



##### TERMINATED

终⽌状态。此时线程已执⾏完毕。



### 实现线程的三种方法

1.继承Thread类

2.实现Runnable接口

3.实现Callable接口



#### 一.继承Thread 类

继承Thread类，重写run方法

```java
public class TestThread extends Thread {
    public void run(){
        for (int i = 0; i < 100; i++) {
            System.out.println("我在看"+i+"代码");
        }
    }

    public static void main(String[] args) {
		//因为jvm加载的时候TestThread这个类就在该包下，所以可以直接在static时实例化。
        TestThread testThread = new TestThread();
		
        //用start方法来启动线程，真正实现了多线程运行，
        //这时无需等待run方法体代码执行完毕而直接继续执行下面的代码
        testThread.start();
        /*
        	start方法代码的调用顺序不一定是线程启动顺序;
        	
        */
        

        for (int i = 0; i < 100; i++) {
            System.out.println("我在学习"+i+"多线程");
        }
    }

}
```



#### 二.实现接口类Runnable

```java
public class TestRunnable implements  Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("我在听课");
        }
    }

    public static void main(String[] args) {
        TestRunnable testRunnable = new TestRunnable();

        Thread thread = new Thread(testRunnable);

        thread.start();

        for (int i = 0; i < 100; i++) {
            System.out.println("我在打瞌");
        }
    }
}
```



### 线程常用方法

Thread.currentThread():静态方法，可以获得当前调用代码的线程

Thread.sleep(int):静态方法，让当前线程阻塞

Thread.yield(): 让当前线程放弃cpu资源，Runing回到Runnable



setName("")/getName(""):获取线程名称

==案例==

```java
public class Test{
    psvm{
         /*
        	输出:
        	main
        	Thread-0
        */
        SubThread t1= new SubThread();
        t1.setName("t1");
         /*
        	输出:
        	t1
        	t1
        */
        t1.start();
        Thread.sleep(1000);
        
        /*
        	输出:
        	Thread-1
        	t1
        */
        Thread t2 = new Thread(t1);
        t2.start();
        
    }
    
    
}


public class SubThread extends Thread{
    public SubThread(){
        System.out.println("构造方法中的currentThread:"+Thread.currentThread().getName());
        System.out.println("构造方法中，this.getName:"+this.getName());
    }

    public void run(){

        System.out.println("run方法中currentThread:"+Thread.currentThread().getName());
        System.out.println("run方法中getName:"+this.getName());
    }
    
    
}
```



isAlive()：判断当前线程师父处于活动状态

活动状态就是 线程已启动并且尚未终止；



getId():获取线程id，==会重新分配==



setPriority(num): 设置线程的优先级1—10

给予一个提示信息，但不能保证优先成稿的线程进行；



interrupt(): 中断线程，打上一个停止标志true，并不是停止；

---->isInterrupted()方法，返回布尔值，可以根据布尔值return退出；



setDaemon(boolean):守护线程，==start()前运行==，主线程结束，该线程结束；





#### wait、notify、join

wait 和 notify 均为 Object 的方法：

- Object.wait() —— 暂停一个线程
- Object.notify() —— 唤醒一个线程



wait()方法 表示持有对象锁的线程准备释放对象锁权限，释放CPU资源并进入等待；

notify唤醒一个线程B，B这个线程要等到当前对象释放synchronized的对象锁才能执行，也就是A执行 flag.wait()才能执行

==注==

1.wait()进入的是waitting状态，需要靠notify唤醒；

2.但是notify完不一定就立刻执行，没有获取到锁资源进入到阻塞状态，需要另外一个线程释放或者是运行结束自动释放锁资源

3.如果A调用了wait方法，即使另外一个线程B执行完毕，如果B没有使用notify，A也不会获得锁资源

4.notifyAll是唤醒所有线程，注意配合使用while循环？如果是if套间wait方法，可能造成唤醒所有方法后，if条件不满足，但所有线程继续执行，产生错误。



##### ==wait和sleep的区别==

- wait可以指定时间，也可以不指定；⽽sleep必须指定时间。
- wait释放cpu资源，同时释放锁；sleep释放cpu资源，但是不释放锁，所以易死锁。
- wait必须放在同步块或同步⽅法中，⽽sleep可以再任意位置



##### 关于join

join的实现代码

```java
public final synchronized void join(long millis)
    throws InterruptedException {
        long base = System.currentTimeMillis();
        long now = 0;

        if (millis < 0) {
            throw new IllegalArgumentException("timeout value is negative");
        }
       	
		//使用join()方法时，会执行这一段；
        if (millis == 0) {
            while (isAlive()) {
                wait(0);
            }
        } else {
            while (isAlive()) {
                long delay = millis - now;
                if (delay <= 0) {
                    break;
                }
                wait(delay);
                now = System.currentTimeMillis() - base;
            }
        }
    }

```

1.方法上加上final说明该方法不能被重写

2.join()默认传递的是join(0) 即millis等于0，会运行 this.isAlive()判断myThread.start()开启的线程是否存活，而wait(0)是一个定时等待方法，且哪个线程使用让哪个线程进入等待，在这个例子中是main()线程使用，所以main()线程等待(==相当于main线程释放掉了对于子线程的控制权，即进入等待，但当子线程不存活，当子线程执行完毕的时候，jvm会自动唤醒阻塞在子线程对象上的线程==)



### 线程池

池化思想：字符串池、线程池

线程池有点：

1. 提高线程的利用率
2. 提高程序的响应速度
3. 便于统一管理线程对象
4. 可以控制最大并发数









### 线程安全问题

非线程安全主要是多个线程对一个对象的实例变量进行操作室，会出现值被更改，值不同步的情况；

线程安全问题主要是==原子性==、==可见性==、==有序性==



#### 原子性

不可分割

1.访问某个共享变量的操作在其他线程来看，要么已经执行完毕，要么尚未发生；

2.访问同一组共享变量的原子操作是不能够交错的；



##### 实现方式

锁：具有排他性

利用处理器的cas(Compare and swap)



#### 可见性

在多线程环境中，一个线程对某个共享变量更新后，后续线程可能无法立即读到这个结果；

![image-20220424201839818](JAVA25多线程.assets/image-20220424201839818.png)

![image-20220424201853675](JAVA25多线程.assets/image-20220424201853675.png)



#### 有序性

一个处理器上运行的一个线程所执行的内存访问操作在另外一个处理器运行的其他线程看来是乱序的。



编译器可能会改变两个操作的先后顺序；

处理器也有可能不按照目标代码的顺序执行，重排序；



### 重排序

#### 指令重排序

![image-20220421104637357](JAVA25多线程.assets/image-20220421104637357.png)



#### 存储系统重排序

存进了缓存，没存进内存？



### Java内存模型

![image-20220424202255954](JAVA25多线程.assets/image-20220424202255954.png)





