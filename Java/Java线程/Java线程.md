> Java多线程可以实现并行处理，避免了某项任务长时间占用CPU时间，以便更好的利用CPU的资源。

我们平时用main方法执行的代码，都是以主线程去执行。

## 1. Java Thread类的6种状态

在《Java并发编程》、《Java核心技术卷一》这两本书说到：**在Java中，线程是分6中状态的**。

Thread类的枚举也印证了这点：

![image-20200611004413289](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200611004413289.png)

| 状态名称      | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| NEW           | **初始状态**，线程刚被构建，但是还没有调用start()方法        |
| RUNNABLE      | **运行状态**，Java系统系统中将操作系统中的就绪和运行两种状态笼统地称为“运行中” |
| BLOCKED       | **阻塞状态**，表示线程阻塞于锁                               |
| WAITTING      | **等待状态**，表示线程进入等待状态，进入该状态表示当前线程做出一些特定动作（通知或者中断） |
| TIME_WAITTING | **超时等待状态**，该状态不同于等待状态，它可以在指定的时间后自行返回 |
| TERMINATED    | **中止状态**，表示当前线程已经执行完毕                       |

Blocked也分三种：

(一). 等待阻塞：运行(running)的线程执行o.wait()方法，JVM会把该线程放入等待队列(waitting queue)中，通过调用motify()方法回到就绪状态。
(二). 同步阻塞：运行(running)的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入锁池(lock pool)中。
(三). 其他阻塞：运行(running)的线程执行Thread.sleep(long ms)或t.join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新自动转入可运行(runnable)状态。

![image-20200611005459456](F:\笔记\yudianxxJavaNote\Java\Java线程\picture\image-20200611005459456.png)

## 2.线程的状态(传统模型)

**在操作系统中**，线程的生命周期可以分为5种状态：

①**new** 关键字创建了Thread类（或其子类）的对象，或者Runnable。

②**Runnable** 调用了start()方法，这时的线程就等待时间片轮转到自己这，以便获得CPU；第二种情况是线程在处于RUNNING状态时并没有运行完自己的run方法，时间片用完之后回到RUNNABLE状态；还有种情况就是处于BLOCKED状态的线程结束了当前的BLOCKED状态之后重新回到RUNNABLE状态。

③**Running**：这时的线程指的是获得CPU的RUNNABLE线程，RUNNING状态是所有线程都希望获得的状态。

④**Dead**：处于RUNNING状态的线程，在执行完run方法之后，就变成了DEAD状态了。

⑤**Blocked**：这种状态指的是处于RUNNING状态的线程，出于某种原因，比如调用了sleep方法、等待返回（调用wait方法）。



![image-20200611004316963](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200611004316963.png)

## 



事实是Thread和Runnable没有本质的区别。

点击Thread类，看一下源码，你会发现其实Thread也是实现了Runnable接口

```java
public
class Thread implements Runnable {
    /* Make sure registerNatives is the first thing <clinit> does. */
    private static native void registerNatives();
    static {
        registerNatives();
    }
```



参考：

https://www.cnblogs.com/wxd0108/p/5479442.html