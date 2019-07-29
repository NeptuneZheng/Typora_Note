# 							Spring4 Note

## Ⅰ. Advantages of Spring 4

- 轻量级框架

  > 指业务代码无需继承或者实现框架的类或者接口，使得框架对业务代码没有侵入性

  ---

  

- IOC容器   ----- 控制反转  / 依赖注入

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

  

- AOP 面向切面编程

  ---

  ***静态代理***
  
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
  
  ***动态代理***    (指动态生成代理类)
  
  https://rejoy.iteye.com/blog/1627405
  
  https://blog.csdn.net/xybz1993/article/details/80584157
  
  > PS: 此处建议使用Java而不是Groovy（**至少真实角色类要是Java的**），因为Groovy是***运行时调度和多方法***会在invoke方法处多次调用，导致代理方法多次执行。而Java在编译时，根据定义的类型， 选择方法,在运行时就不会反复调用invoke方法。[参考链接]: https://www.cnblogs.com/caizengming/p/9546132.html
  
  ![](D:\Typora\MKDFiles\media\1564385281471.png)
  
  
  
  > - 基于接口动态代理 --- JDK动态代理 ---- 只能对有接口类的实例进行代理，因为Java是单继承，而Proxy.newProxyInstance这步生成的proxy继承了Proxy类
  >
  >   public final class $Proxy0 extends Proxy implements Login
  >
  >   https://www.cnblogs.com/wh-king/archive/2013/01/02/2841880.html
  
  interface:   **--- Java或者Groovy**
  
  ```java
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
  
  ```java
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
  
  ```java
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
  
  
  
  > - 基于类的动态代理 --- cglib, javassist



* Spring事物

  

