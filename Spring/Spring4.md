# 							                          Spring4 Note

## Ⅰ. Advantages of Spring 4

- 轻量级框架

  > 指业务代码无需继承或者实现框架的类或者接口，使得框架对业务代码没有侵入性

  ---

  

  ### IOC容器   ----- 控制反转  / 依赖注入

  >- 由原来的的程序本身主动地创建对象，变为现在程序接收对象
  >
  >- 实现了Service，Dao，Entity之间的解耦，没有直接依赖关系。如果Dao层发生改变，Service层无需改变
  >
  >---
  >
  >[**控制的内容**] ： 对象创建的权利
  >
  >[**反转**]：对象创建权利的转换，由程序转交给Spring
  >
  >---
  >
  >[**依赖**]：bean对象的创建依赖于容器
  >
  >[**注入**]：bean对象以来的资源由容器来设置和装配

  

   - IOC 容器本质是BeanFactory(**interface**，ClassPathXmlApplication 是其实现方式之一)

   - IOC创建Bean有三种方式

     ​			-- Set方法，当初始化时会同时将对应的属性赋值（需要有对应的set方法）

     ​			-- 通过有参构造方法

     ​			-- 通过工厂方法，又细分为静态工厂和动态工厂

  ---

   

  ### AOP 面向切面编程 

  ### 			--- 在Spring应用中提供声明式服务，允许用户自定义切面

  ---

  #### A. 涉及的名词概念：

  [关注点]: 增加的某个业务	"如：日志，安全，缓存，事物，异常处理等"
  [切面Aspect]: 一个关注点的模块化	"将摸一个关注点（如日志）封装为一个模块（类）"
  [连接点Joinpoint]: 实体业务中可织入AOP业务的方法
  [通知Advice]: 在切面的某个特定的连接点上执行的动作	"包括：around，before，after"
  [切入点Pointcut]: 切面织入实体类的位置上

  

  ---

  #### B. 静态代理

  - 静态代理的角色
    - 抽象角色 --- 公共接口或抽象类
    - 真实角色
    - 代理角色
  - 静态代理的优点：
    - 使真实角色变得纯粹，不再去关注一些公共业务，公共业务由代理完成，实现了业务的分工。
    - 公共业务扩展时更加集中和方便
  - 静态代理的缺点：
    - 类的数量变多，工作量变大，开发效率降低

  ---

  #### C. 动态代理    (指动态生成代理类)

  - **Spring AOP中的JDK和CGLib动态代理哪个效率更高**

    https://zhuanlan.zhihu.com/p/67041662

  > ## 一、基本概念
>
  > 首先，我们知道Spring AOP的底层实现有两种方式：一种是JDK动态代理，另一种是CGLib的方式。
>
  > 自Java 1.3以后，Java提供了动态代理技术，允许开发者在运行期创建接口的代理实例，后来这项技术被用到了Spring的很多地方。
>
  > JDK动态代理主要涉及java.lang.reflect包下边的两个类：Proxy和InvocationHandler。其中，InvocationHandler是一个接口，可以通过实现该接口定义横切逻辑，并通过反射机制调用目标类的代码，动态地将横切逻辑和业务逻辑贬值在一起。
>
  > JDK动态代理的话，他有一个限制，就是它只能为接口创建代理实例，而对于没有通过接口定义业务方法的类，如何创建动态代理实例哪？答案就是CGLib。
  >
  > CGLib采用底层的字节码技术，全称是：Code Generation Library，CGLib可以为一个类创建一个子类，在子类中采用方法拦截的技术拦截所有父类方法的调用并顺势织入横切逻辑。
>
  > ## 二、JDK 和 CGLib动态代理区别
>
  > - 通过实现InvocationHandlet接口创建自己的调用处理器；
  > - 通过为Proxy类指定ClassLoader对象和一组interface来创建动态代理；
  > - 通过反射机制获取动态代理类的构造函数，其唯一参数类型就是调用处理器接口类型；
  > - 通过构造函数创建动态代理类实例，构造时调用处理器对象作为参数参入；
  >
  > JDK动态代理是面向接口的代理模式，如果被代理目标没有接口那么Spring也无能为力，Spring通过Java的反射机制生产被代理接口的新的匿名实现类，重写了其中AOP的增强方法。
  >
  > **2、CGLib动态代理：**
  >
  > CGLib是一个强大、高性能的Code生产类库，可以实现运行期动态扩展java类，Spring在运行期间通过 CGlib继承要被动态代理的类，重写父类的方法，实现AOP面向切面编程呢。
>
  > **3、两者对比：**
>
  > - JDK动态代理是面向接口的。
  > - CGLib动态代理是通过字节码底层继承要代理类来实现（如果被代理类被final关键字所修饰，那么抱歉会失败）。
  >
  > **4、使用注意：**
  >
  > - 如果要被代理的对象是个实现类，那么Spring会使用JDK动态代理来完成操作（Spirng默认采用JDK动态代理实现机制）；
  > - 如果要被代理的对象不是个实现类那么，Spring会强制使用CGLib来实现动态代理。
  >
  > ## 三、JDK 和 CGLib动态代理性能对比-教科书上的描述
  >
  > 我们不管是看书还是看文章亦或是我那个上搜索参考答案，可能很多时候，都可以找到如下的回答：
  >
  > 关于两者之间的性能的话，JDK动态代理所创建的代理对象，在以前的JDK版本中，性能并不是很高，虽然在高版本中JDK动态代理对象的性能得到了很大的提升，但是他也并不是适用于所有的场景。主要体现在如下的两个指标中：
  >
  > 1、CGLib所创建的动态代理对象在实际运行时候的性能要比JDK动态代理高不少，有研究表明，大概要高10倍；
  >
  > 2、但是CGLib在创建对象的时候所花费的时间却比JDK动态代理要多很多，有研究表明，大概有8倍的差距；
  >
  > 3、因此，对于singleton的代理对象或者具有实例池的代理，因为无需频繁的创建代理对象，所以比较适合采用CGLib动态代理，反正，则比较适用JDK动态代理。
  >
  > 4、经过多次试验，可以看出平均情况下的话，JDK动态代理的运行速度已经逐渐提高了，在低版本的时候，运行的性能可能不如CGLib，但是在1.8版本中运行多次，基本都可以得到一致的测试结果，那就是JDK动态代理已经比CGLib动态代理快了！
  
  
  
  https://rejoy.iteye.com/blog/1627405
  
  https://blog.csdn.net/xybz1993/article/details/80584157
  
  > PS: 此处建议使用Java而不是Groovy（**至少真实角色类要是Java的**），因为Groovy是***运行时调度和多方法***会在invoke方法处多次调用，导致代理方法多次执行。而Java在编译时，根据定义的类型， 选择方法,在运行时就不会反复调用invoke方法。[参考链接]: https://www.cnblogs.com/caizengming/p/9546132.html
  
  ![](D:\Typora\MKDFiles\media\1564385281471.png)
  
  
  
  ##### 1. 基于接口动态代理 --- JDK动态代理 ---- 只能对有接口类的实例进行代理，因为Java是单继承，而Proxy.newProxyInstance这步生成的proxy继承了Proxy类

  > public final class $Proxy0 extends Proxy implements Login
>
  > https://www.cnblogs.com/wh-king/archive/2013/01/02/2841880.html

  interface:   **--- Java或者Groovy**
  
```java/groovy
  package aop.interfaces;
  
  public interface Login {
      boolean loginVerify(String name,String password);
      void saySomething();
      Object getAReturn(int i);
  }
  
  ```
  
  real class:  **--- 必须是Java**
  
  ```java
  package aop.impl;
  
  import aop.interfaces.Login;
  import com.sun.org.apache.xpath.internal.operations.String;
  
  import java.util.ArrayList;
  import java.util.HashMap;
  import java.util.HashSet;
  
  public class WebLogin implements Login {
      @Override
      public boolean loginVerify(java.lang.String name, java.lang.String password) {
          if(name.equals(password)){
              return true;
          }
          return false;    }
  
      @Override
      public void saySomething() {
          System.out.println("welcome to WebLogin");
      }
  
      @Override
    public Object getAReturn(int i) {
          switch (i){
            case 1:return new ArrayList<>();
              case 2:return new HashMap<>();
            case 3:return new HashSet<>();
              default:return new StringBuilder("HHHH");
  
          }
      }
  }
  ```
  
  
  
  proxy class:   **--- Java或者Groovy**
  
  [InvocationHandler]: 是代理实例的调用处理程序实现的接口	"代理类需要实现"
  [Proxy]: 供用于创建动态代理类和实例的静态方法，它还是由这些方法创建的所有动态代理类的超类	"代理类"
  
  ```java/groovy
  package aop.proxy;
  
  import org.apache.logging.log4j.LogManager;
  import org.apache.logging.log4j.Logger;
  
  import java.lang.reflect.InvocationHandler;
  import java.lang.reflect.Method;
  import java.lang.reflect.Proxy;
  import java.util.Arrays;

  public class MyInvocationHandler implements InvocationHandler {
    private Logger logger = LogManager.getLogger();
      private Object login;

      public MyInvocationHandler(Object login) {
        this.login = login;
      }
  
      @Override
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
          log(method.getName());
          Object object = method.invoke(login,args);
          System.out.println("after invoke function ..." + Arrays.toString(args));
          return object;
      }
  
      void log(String methodName){
          logger.info(this.getClass().getName() + " call my proxy with Method: " + methodName);
      }
  
      public Object getProxy(){
          return Proxy.newProxyInstance(login.getClass().getClassLoader(),login.getClass().getInterfaces(),this);
      }
  }
  ```
  
  
  
  test:    --- **Java或者Groovy**
  
  ```java/groovyjava
  import aop.impl.WebLogin;
  import aop.interfaces.Login;
  import aop.proxy.MyInvocationHandler;
  
  public class Main {
      public static void main(String[] args) {
          Login login = new WebLogin();
  
          MyInvocationHandler handler = new MyInvocationHandler(login);
  
          Login proxy = (Login) handler.getProxy();

          proxy.saySomething();
        System.out.println("---------------------------");
          System.out.println(proxy.loginVerify("aaa","sou"));
        System.out.println("---------------------------");
          System.out.println(proxy.loginVerify("bbb","bbb"));
          System.out.println("---------------------------");
          System.out.println(proxy.getAReturn(0));
  
      }
  }
  ```
  
  ![](D:\Typora\MKDFiles\media\1564380919525.png)
  

  
  如果把真实角色类改成Groovy代码，则会出现如下多次调用问题：
  
  ```groovy
  package aop.impl;
  
  import aop.interfaces.Login;
  import com.sun.org.apache.xpath.internal.operations.String;
  
  import java.util.ArrayList;
  import java.util.HashMap;
  import java.util.HashSet;
  
  public class WebLogin implements Login {
      @Override
      public boolean loginVerify(java.lang.String name, java.lang.String password) {
          if(name.equals(password)){
              return true;
          }
          return false;    }
  
      @Override
      public void saySomething() {
          System.out.println("welcome to WebLogin");
      }
  
      @Override
      public Object getAReturn(int i) {
          switch (i){
              case 1:return new ArrayList<>();
              case 2:return new HashMap<>();
              case 3:return new HashSet<>();
              default:return new StringBuilder("HHHH");
  
          }
      }
  }
  
  ```
  
  ![](D:\Typora\MKDFiles\media\1564384231542.png)
  
  机制分析:
  
  Proxy.newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)做了以下几件事.
  (1)根据参数loader和interfaces调用方法 getProxyClass(loader, interfaces)创建代理类$Proxy.$Proxy0类 实现了interfaces的接口,并继承了Proxy类.
  (2)实例化$Proxy0并在构造方法中把BusinessHandler传过去,接着$Proxy0调用父类Proxy的构造器,为h赋值,如下:
  class Proxy{
     InvocationHandler h=null;
     protected Proxy(InvocationHandler h) {
      this.h = h;
     }
     ...
  }
  
  ##### 2. 基于类的动态代理 --- cglib, javassist

 





### Spring事物



