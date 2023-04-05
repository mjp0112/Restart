# 多线程实践



### 1.获取钥匙

```java

public class Client {
    private int number = 0;

    private void read() {
        System.out.println("number = "+ number);
    }

    private void write(int change) {
        number += change;
    }

    @Test
    public void test() throws InterruptedException {
        // 开启一个线程加 10000 次
        new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                write(1);
            }
            System.out.println("增加 10000 次已完成");
        }).start();

        // 开启一个线程减 10000 次
        new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                write(-1);
            }
            System.out.println("减少 10000 次已完成");
        }).start();

        // 睡眠一秒保证线程执行完成
        Thread.sleep(1000);
        // 读取结果
        read();
    }
}

//上述的运行结果可能并不是我们想象中的number=0，而是每次number的数值可能都会随机变化
增加 10000 次已完成
减少 10000 次已完成
number = -981
  
增加 10000 次已完成
减少 10000 次已完成
number = 92
```

在多线程下需要保证每一条操作必须为原子操作，不然有可能导致结果不正确。因为操作系统需要提高电脑运行效率。**线程的调度完全是由操作系统决定的，程序不能自己决定什么时候执行，以及执行多长时间。**所以需要使用钥匙作为通行证来保证一次只有一个线程对资源进行操作。

```java

public class Client {
    private int number = 0;
    private final Object lock = new Object();

    private void read() {
        synchronized (lock) {
            System.out.println("number = " + number);
    	 }
	}

    private void write(int change) {
        synchronized (lock) {
            number += change;
            System.out.println("写入 " + number);
        }
    }

    @Test
    public void test() throws InterruptedException {
        // 开启一个线程写入 100 次 number
        new Thread(() -> {
            for (int i = 0; i < 100; i++) {
                write(1);
            }
        }).start();

        // 开启一个线程读取 100 次 number
        new Thread(() -> {
            for (int i = 0; i < 100; i++) {
                read();
            }
        }).start();

        // 睡眠一秒保证线程执行完成
        Thread.sleep(1000);
    }
}
```



### 2.实现写完再读取的功能

很简单的，我们会首先想到增加一个标志位writeCompleted，当写入完成后对其惊醒更改。相应的write方法为。

```java
boolean writeCompleted = false;

private void read() {
        synchronized (lock) {
            // 如果还没有写入完成，循环等待直到写入完成
            while (!writeCompleted) {
                System.out.println("等待写入完成...");
            }
            System.out.println("number = " + number);
        }
}
```

不过直接如此更改，会出现如下错误

```java
写入 1
写入 2
写入 3
写入 4
写入 5
写入 6
等待写入完成...
等待写入完成...
等待写入完成...
等待写入完成...
... 省略 20 多万次 "等待写入完成..."
```

因为read方法会抢占了钥匙后，导致write方法阻塞。

至此，引入==wait和notify==方法，唯一需要注意的一点是，等待与唤醒操作必须在锁的范围内执行，也就是说，调用 wait() 或 notify() 时，都必须用 synchronized 锁住被 wait/notify 的对象。如果不是的话会报运行时异常。至于为什么要如此设计，是因为原子性原因，有可能导致一个方法还没运行到wait方法，另外一个方法通过抢占的形式运行到notify，那么其中一个线程会一直等待。

```java

public class Client {
    private int number = 0;
    private final Object lock = new Object();
    // 标志是否写入完成
    private boolean writeComplete = false;

    private void read() {
        synchronized (lock) {
            // 如果还没有写入完成，循环等待直到写入完成
            while (!writeComplete) {
                // 等待，并且不要阻塞写入
                try {
                    lock.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println("number = " + number);
        }
    }

    private void write(int change) {
        synchronized (lock) {
            number += change;
            System.out.println("写入 " + number);
        }
    }

    @Test
    public void test() throws InterruptedException {
        // 开启一个线程写入 100 次 number
        new Thread(() -> {
            writeComplete = false;
            for (int i = 0; i < 100; i++) {
                write(1);
            }
            writeComplete = true;
            // 写入完成，唤醒读取线程，wait/notify 操作必须在 synchronized 中执行。
            synchronized (lock) {
                lock.notify();
            }
        }).start();

        // 开启一个线程读取 100 次 number
        new Thread(() -> {
            for (int i = 0; i < 100; i++) {
                read();
            }
        }).start();

        // 睡眠一秒保证线程执行完成
        Thread.sleep(1000);
    }
}
```

