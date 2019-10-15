

# Basic Knowledge of Class Loading 

## Ⅰ. 类加载步骤

### 1. 加载

> 查找并加载类的二进制数据
>
> 类加载器并不需要等到某个类被`首次主动使用`（跟初始化相区别）时再加载它

![1568895199140](D:\Typora\MKDFiles\media\1568895199140.png)

### 3. 连接

#### - 验证

> 确保加载类的正确性

#### - 准备

> 为类的***静态变量***分配内存，并将其初始化为***默认值***

#### - 解析

> 把类中的符号引用转换为直接引用（指针）

### 4. 初始化

> 为类的静态变量赋予真正的初始值(只有`首次主动`使用才会初始化)
>
> 静态变量的声明语句，以及静态代码块都被看做类的初始化语句，Java虚拟机会按照初始化语句在类文件中的先后顺序来依次执行它们。



![1568895994346](D:\Typora\MKDFiles\media\1568895994346.png)

- 访问某个类或者接口的静态变量，或者对该静态变量的赋值，或者对该静态方法的使用（**这个静态变量/方法定义在哪，就表示对哪个类对的主动使用**）

 ```java
public class test {
    public static void main(String[] args) {
        System.out.println(Child.a);
    }
}


class Parent{
   public static int a = 10;
   static {
       System.out.println("Parent invoked");
   }
}

class Child extends Parent{
    static {
        System.out.println("Child invoked");
    }
}
 ```

> 没有输出"Child invoked"，因为不是对Child的主动使用

![1569329694905](D:\Typora\MKDFiles\media\1569329694905.png)



![1568896044163](D:\Typora\MKDFiles\media\1568896044163.png)

![1568896067356](D:\Typora\MKDFiles\media\1568896067356.png)

![1568896320698](D:\Typora\MKDFiles\media\1568896320698.png)

- 反射是对类的主动使用，会导致类的初始化。相反，调用ClassLoader类的loadClass方法加载一个类，并不是对类的主动使用，不会导致类的初始化。

```java
public class test {
    public static void main(String[] args) throws Exception {
        ClassLoader loader = ClassLoader.getSystemClassLoader();
        Class<?> clazz = loader.loadClass("Parent");
        System.out.println(clazz);
        
        System.out.println("-----------");

        clazz = Class.forName("Parent");
        System.out.println(clazz);
    }
}


class Parent{
   static {
       System.out.println("Parent invoked");
   }
}
```

![1569330468577](D:\Typora\MKDFiles\media\1569330468577.png)



### 5. 使用

### 6. 卸载



> -XX:+TraceClassLoading， 用于追踪类的加载信息并打印出来
>
>  
>
> 设置JVM参数的三种方式：
>
> - `-XX:+<option>`表示开启option选项
> - `-XX:-<option>`表示关闭option选项
> - `-XX:<option>=<value>`表示将option选项的值设置为value



![1568894428891](D:\Typora\MKDFiles\media\1568894428891.png)



## Ⅱ. 常量的本质含义

### 1. 常量在***编译阶段*** 会存入到调用这个常量的方法所在的类的常量池中。本质上，调用类并没有直接引用到定义该常量的类，因此并不会触发定义该常量类的初始化。

> 注意：
>
> ​	这里指的是将常量存放到test的常量池中，之后test与test3就没有任何关系了。甚至，可以将test3编译生成的class文件删除，依然可以取到该常量。



```java
public class test {
    static {
        System.out.println("****** test static block");
    }

    public static void main(String[] args) {
        System.out.println(test3.str);
    }
}

class test3{
    public static String str = "hello world!";
    static {
        System.out.println("------ test3 static block");
    }
}

```

![1568722987211](D:\Typora\MKDFiles\media\1568722987211.png)

```java
public class test {
    static {
        System.out.println("****** test static block");
    }

    public static void main(String[] args) {
        System.out.println(test3.str);
    }
}

class test3{
    public static final String str = "hello world!";
    static {
        System.out.println("------ test3 static block");
    }
}
```

![1568722944481](D:\Typora\MKDFiles\media\1568722944481.png)



### 2. 当一个常量的值并非编译期可以确定，那么其值就不会被放到调用类的常量池中。这时在程序运行时，会导致主动使用这个常量所在的类，显然会导致这个类被初始化。

```java
import java.util.UUID;

public class test {
    static {
        System.out.println("****** test static block");
    }

    public static void main(String[] args) {
        System.out.println(test3.str);
    }
}

class test3{
    public static String str = UUID.randomUUID().toString();
    static {
        System.out.println("------ test3 static block");
    }
}

```

![1568724964713](D:\Typora\MKDFiles\media\1568724964713.png)



### 3. 对于数组实例来说，其类型是由JVM在运行期动态生成的（***仅有数组类对象不是由ClassLoader来创建的，而是由JVM在运行期动态生成的***）。动态生成的类型其父类型就是Object。

> Class objects for array classes are not created by class loaders, but are created automatically as required by the Java runtime. 
>
> The class loader for an array class, as returned by Class.getClassLoader() is the same as the class loader for its element type; if the element type is a primitive type, then the array class has no class loader.

```java
import java.util.UUID;

public class test {
    static {
        System.out.println("****** test static block");
    }

    public static void main(String[] args) {
       test3[] test3 = new test3[1];

        System.out.println("----------------------");
        System.out.println(test3.getClass());
        System.out.println(test3.getClass().getSuperclass());
        System.out.println(test3.getClass().getClassLoader());
    }
}

class test3{
    public static String str = UUID.randomUUID().toString();
    static {
        System.out.println("------ test3 static block");
    }
}
```

> 没有主动使用test3类，所以不会初始化该类

![1569415873636](D:\Typora\MKDFiles\media\1569415873636.png)



### 4. 当一个接口在初始化时并不要求其父接口都完成初始化（有类加载但是没有初始化）

![1568896197404](D:\Typora\MKDFiles\media\1568896197404.png)

- Interface extends interface

```java
import java.util.UUID;

public class test {
    public static void main(String[] args) {
        System.out.println(Child.a);
    }
}


interface Parent{
    public static Thread thread = new Thread(){
        {
            System.out.println("Parent invoked");
        }
    };
}

interface Child extends Parent{
    public static String a = UUID.randomUUID().toString();
}
```

![1569326455668](D:\Typora\MKDFiles\media\1569326455668.png)





- class implement interface

  ```java
  import java.util.UUID;
  
  public class test {
      public static void main(String[] args) {
          System.out.println(Child.a);
      }
  }
  
  
  interface Parent{
      public static Thread thread = new Thread(){
          {
              System.out.println("Parent invoked");
          }
      };
  }
  
  class Child implements Parent{
      public static String a = UUID.randomUUID().toString();
  }
  ```

  ![1569326503295](D:\Typora\MKDFiles\media\1569326503295.png)

  

- class extends class

```java
import java.util.UUID;

public class test {
    public static void main(String[] args) {
        System.out.println(Child.a);
    }
}


class Parent{
    public static Thread thread = new Thread(){
        {
            System.out.println("Parent invoked");
        }
    };
}

class Child extends Parent{
    public static String a = UUID.randomUUID().toString();
}
```

![1569326555173](D:\Typora\MKDFiles\media\1569326555173.png)





## Ⅲ. 类加载器准备阶段和初始化阶段的重要意义

```java
public class test {
    public static void main(String[] args) {
        Singleton singleton = Singleton.getInstance();
        System.out.println("a: " + Singleton.a);
        System.out.println("b: " + Singleton.b);
    }
}

class Singleton{
    public static int a;
    public static int b = 0;

    private static Singleton singleton = new Singleton();

    public Singleton() {
        a++;
        b++;

        System.out.println(">>>a: " + a);
        System.out.println(">>>b: " + b);
    }

    public static Singleton getInstance(){
        return singleton;
    }
}

```

![1568811055803](D:\Typora\MKDFiles\media\1568811055803.png)

---

---



```java
public class test {
    public static void main(String[] args) {
        Singleton singleton = Singleton.getInstance();
        System.out.println("a: " + Singleton.a);
        System.out.println("b: " + Singleton.b);
    }
}

class Singleton{
    public static int a;

    private static Singleton singleton = new Singleton();

    public Singleton() {
        a++;
        b++;

        System.out.println(">>>a: " + a);
        System.out.println(">>>b: " + b);
    }
    public static int b = 0;

    public static Singleton getInstance(){
        return singleton;
    }
}
```

> 因为准备阶段赋初始值0，然后在初始化阶段按顺序执行代码。

![1568811120676](D:\Typora\MKDFiles\media\1568811120676.png)

```java
public class test {
    public static void main(String[] args) {
        Singleton singleton = Singleton.getInstance();
        System.out.println("a: " + Singleton.a);
        System.out.println("b: " + Singleton.b);
    }
}

class Singleton{
    public static int a;

    private static Singleton singleton = new Singleton();

    public Singleton() {
        a++;
        b++;

        System.out.println(">>>a: " + a);
        System.out.println(">>>b: " + b);
    }
    public static int b;

    public static Singleton getInstance(){
        return singleton;
    }
}
```

![1568811462193](D:\Typora\MKDFiles\media\1568811462193.png)

## Ⅳ. 类加载器的双亲委托机制

### 1. 获取ClassLoader的途径

![1569413478086](D:\Typora\MKDFiles\media\1569413478086.png)

- 基本数据类型以及String都是由启动类加载器加载的
- 15

