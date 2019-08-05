# 			Basic Knowledges of Thread

## Ⅰ. Thread Ststus

https://mp.weixin.qq.com/s/_1pU-nfok24dCfYz_jyQfA

https://www.xttblog.com/?p=3942

![1564550449270](D:\Typora\MKDFiles\media\1564550449270.png)

![1564550597199](D:\Typora\MKDFiles\media\1564550597199.png)

### 1. Thread status in code

https://www.zhihu.com/question/27654579/answer/252912242

#### - 初始态：NEW

创建一个Thread对象，但还未调用start()启动线程时，线程处于初始态。

#### - 运行态：RUNNABLE

在Java中，运行态包括就绪态 和 运行态。

- 就绪态  

- - 该状态下的线程已经获得执行所需的所有资源，只要CPU分配执行权就能运行。
  - 所有就绪态的线程存放在就绪队列中。

- 运行态  

- - 获得CPU执行权，正在执行的线程。
  - 由于一个CPU同一时刻只能执行一条线程，因此每个CPU每个时刻只有一条运行态的线程。

#### - 阻塞态

- 当一条正在执行的线程请求某一资源失败时，就会进入阻塞态。
- 而在Java中，阻塞态专指请求锁失败时进入的状态。
- 由一个阻塞队列存放所有阻塞态的线程。
- 处于阻塞态的线程会不断请求资源，一旦请求成功，就会进入就绪队列，等待执行。

PS：锁、IO、Socket等都资源。

#### - 等待态

- 当前线程中调用wait、join、park函数时，当前线程就会进入等待态。
- 也有一个等待队列存放所有等待态的线程。
- 线程处于等待态表示它需要等待其他线程的指示才能继续运行。
- 进入等待态的线程会释放CPU执行权，并释放资源（如：锁）

#### - 超时等待态

- 当运行中的线程调用sleep(time)、wait、join、parkNanos、parkUntil时，就会进入该状态；
- 它和等待态一样，并不是因为请求不到资源，而是主动进入，并且进入后需要其他线程唤醒；
- 进入该状态后释放CPU执行权 和 占有的资源。
- **与等待态的区别：**到了超时时间后自动进入阻塞队列，开始竞争锁。

#### - 终止态

线程执行结束后的状态。

---

## `注意：`

- wait()方法会释放CPU执行权 和 占有的锁。
- sleep(long)方法仅释放CPU使用权，锁仍然占用；线程被放入超时等待队列，与yield相比，它会使线程较长时间得不到运行。
- yield()方法仅释放CPU执行权，锁仍然占用，线程会被放入就绪队列，会在短时间内再次执行。
- wait和notify必须配套使用，即必须使用同一把锁调用；
- wait和notify必须放在一个同步块中
- 调用wait和notify的对象必须是他们所处同步块的锁对象。

---



> 新建状态 New：新的一天
> 准备就绪 start：闹钟响了
> sleeping：还没起床，请给我起床的勇气
> Runnable：起床收拾好了，随时可以坐地铁出发
> Waiting：等地铁来
> I/O阻塞：地铁来了，但要排队上地铁
> synchronized 阻塞：上了地铁，发现暂时没座位
> Running：地铁上找到座位
> Dead：到达目的地

---

### 2. Thread status in dump 

https://www.xttblog.com/?p=3942

这 6 种状态看起来很好理解，但是再实际工作中，当程序出现异常后，你会发现堆栈中的状态和上面的状态不一样。

在 dump 文件里，各种线程状态解释如下：

> - 死锁，Deadlock（重点关注）
> - 执行中，Runnable
> - 等待资源，Waiting on condition（重点关注）
> - 等待获取监视器，Waiting on monitor entry（重点关注）
> - 对象等待中，Object.wait() 或 TIMED_WAITING
> - 暂停，Suspended
> - 阻塞，Blocked（重点关注）
> - 停止，Parked

#### - 死锁 Deadlock 状态 

![线程死锁 Deadlock 状态](D:\Typora\MKDFiles\media\8fa5dcfcgy1g0nfmq9bmrj20go07emz7.jpg)

这是一个典型的死锁堆栈，t1 线程 lock 在地址 0x22a297a8，同时 t2 线程在 waiting to lock 这个地址。更有意思的是，堆栈也记录了发生死锁的代码行数，这对我们定位问题起到很大的帮助。

#### - 等待资源 Waiting on condition 

最常见情况是该线程在 sleep，等待 sleep的时间到了时候，将被唤醒。关键字：TIMED_WAITING,sleeping,parking。TIMED_WAITING可能是调用了有超时参数的wait所引起的。parking指线程处于挂起中。

```java
"thread-1" prio=10 tid=0x00007fbe985cd000 nid=0x7bc6 waiting on condition [0x00007fbe65848000]
  java.lang.Thread.State: TIMED_WAITING (sleeping)
       at java.lang.Thread.sleep(Native Method)
       at com.xxx.MonitorManager$2.run(MonitorManager.java:124)
       at java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:895)
       at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:918)
       at java.lang.Thread.run(Thread.java:662)
```



如果堆栈信息明确是应用代码，则证明该线程正在等待资源。一般是大量读取某资源，且该资源采用了资源锁的情况下，线程进入等待状态，等待资源的读取。

![堆栈线程 WAITING parking](D:\Typora\MKDFiles\media\8fa5dcfcgy1g0nfnv63zhj20nv04habw.jpg)

#### - Waiting on monitor entry 

意味着线程在等待进入一个临界区。Monitor 是 Java中用以实现线程之间的互斥与协作的主要手段，它可以看成是对象或者 Class的锁。每一个对象都有，也仅有一个 monitor。

这种状态通常发生在线程在等待数据库连接池返回一个可用的链接。

```java
" DB-Processor-13" daemon prio=5 tid=0x003edf98 nid=0xca waiting for monitor entry [0x000000000825f000]
java.lang.Thread.State: BLOCKED (on object monitor)
       at beans.ConnectionPool.getConnection(ConnectionPool.java:102)
       - waiting to lock <0xe0375410> (a beans.ConnectionPool)
       at xxx.getTodayCount(ServiceCnt.java:111)
       at xxx.ServiceCnt.insertCount(ServiceCnt.java:43)
```



#### - Blocked 

线程阻塞，是指当前线程执行过程中，所需要的资源长时间等待却一直未能获取到，被容器的线程管理器标识为阻塞状态，可以理解为等待资源超时的线程。如果线程处于 Blocked 状态，但是原因不清楚。可以使用 jstack -m pid 得到线程的 mixed 信息。

```java
----------------- t@13 -----------------
0xff31e8b8      ___lwp_cond_wait + 0x4
0xfea8c810      void ObjectMonitor::EnterI(Thread*) + 0x2b8
0xfeac86b8      void ObjectMonitor::enter2(Thread*) + 0x250
```



例如，上面的信息表明，线程在尝试进入同步块时阻塞了。

最后，要强调一下。

当程序出现故障，往往一次 dump 的信息，还不足以确认问题。建议产生三次 dump 信息，如果每次 dump 都指向同一个问题，我们才确定问题的典型性。

堆栈信息只是一种参考，一些正常 RUNNING 的线程，由于复杂网络环境和 IO 的影响，也是有问题的，用 jstack 就无法定位，需要结合对业务代码的理解。