# 						VM Analysis Skills

## Ⅰ. How to use VisualVM

#### 1. For GC Monitor and Thread Analysis 

- https://blog.51cto.com/tianxingzhe/1651384
- https://www.ibm.com/developerworks/cn/java/j-lo-visualvm/
- https://www.jianshu.com/p/bb4ba6612392
- [Java多线程的监控分析工具(VisualVM)][http://developer.51cto.com/art/201203/326397.htm]
- [Visual GC 插件使用][https://www.jianshu.com/p/9e4ccd705709]



1. ​    **install Visual GC**

   - https://visualvm.github.io/pluginscenters.html

      find and download matched plugin

   ![1564019984296](D:\Typora\MKDFiles\media\1564019984296.png)

   

2.  **install Process Explorer for Windows**

   - https://blog.csdn.net/agony_sun/article/details/78793932

     ![1564020015450](D:\Typora\MKDFiles\media\1564020015450.png)

     

3.  find the App PID with **Task Manager** or   **Process Explorer**

![1564020289111](D:\Typora\MKDFiles\media\1564020289111.png)

​					![1564020623417](D:\Typora\MKDFiles\media\1564020623417.png)



4. find the most CPU usage thread's TID with **Process Explorer**

   ![1564020762186](D:\Typora\MKDFiles\media\1564020762186.png)

   5. convert TID to HEX

      ![1564036845924](D:\Typora\MKDFiles\media\1564036845924.png)

      6. dump thread in JVisualVM  and save

         ![1564037220499](D:\Typora\MKDFiles\media\1564037220499.png)

      7. location the thread of 17A0C with dump info

         ![1564037961418](D:\Typora\MKDFiles\media\1564037961418.png)

      

      #### **`补充：`**

      [Java线程Dump分析工具--jstack][https://dzone.com/articles/how-to-read-a-thread-dump]
      
      [How to Read a Thread Dump][https://www.cnblogs.com/nexiyi/p/java_thread_jstack.html]
      
      在Dump文件中关键字解析：
      
      > "pool-1-thread-5" #23 prio=5 os_prio=0 tid=0x000000001eb42800 nid=0x17634 runnable [0x000000001f95e000]
      >    java.lang.Thread.State: RUNNABLE
      > 	at sun.reflect.GeneratedMethodAccessor7.invoke(Unknown Source)
      > 	at org.codehaus.groovy.runtime.metaclass.ClosureMetaClass.invokeMethod(ClosureMetaClass.java:294)
      > 	at groovy.lang.MetaClassImpl.invokeMethod(MetaClassImpl.java:1022)
      > 	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
      > 	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
      >
      >    Locked ownable synchronizers:
      >
      > <0x0000000770123ba0> (a java.util.concurrent.ThreadPoolExecutor$Worker)
      
      | SECTION                       | EXAMPLE                  | DESCRIPTION                                                  |
      | :---------------------------- | :----------------------- | :----------------------------------------------------------- |
      | Name                          | `"pool-1-thread-5"`      | 线程名                                                       |
      | ID                            | `#23`                    | 线程创建是的sequence ID                                      |
      | Daemon status                 | `daemon`                 | A tag denoting if the thread is a daemon thread. If the thread is a daemon, this tag will be present; if the thread is a non-daemon thread, no tag will be present. For example, `Thread-0` is not a daemon thread and therefore has no associated `daemon` tag in its summary: `Thread-0" #12 prio=5...`. |
      | Priority                      | `prio=5`                 | 线程优先级[1-10]，代表获得CPU使用权的概率，默认为5，数值越大，优先级越高 |
      | OS Thread Priority            | `os_prio=0`              | 操作系统线程优先级。这个优先级可以不同于Java线程优先级，并且对应于Java线程被调度的OS线程。 |
      | Address                       | `tid=0x000000001eb42800` | Java线程的地址。此地址表示[Java本地接口（JNI）的指针地址]。  |
      | OS Thread ID                  | `nid=0x17634`            | 映射Java“线程”的OS线程的唯一ID。                             |
      | Status                        | `runnable`               | 当前线程状态（具体分类可参考[Basic Knowledges of Thread][D:\Typora\MKDFiles\Java\Thread\ThreadBasic.md]） |
      | Last Known Java Stack Pointer | `[0x000000001f95e000]`   | 线程栈起始地址                                               |
      
      
      
      #### 2. Locate high memory usage case
      
      

