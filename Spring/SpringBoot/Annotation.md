# 					SpringBoot Annotation

## Ⅰ. Annotation classes

> 注解（Annotation）不是程序本身，但是可以对程序作出解释，同时注解还可以让其他程序读取。我们可以把注解加在包（package）、类（class）、方法（method）和变量（field）的上面。



https://blog.csdn.net/lixc_18/article/details/91644896

### 1. 元注解(meta-annotation)   - 4个 

[@Target]:  用来描述注解的使用范围，即注解可以使用在什么地方

| 类型            | 作用域                                                       |
| :-------------- | ------------------------------------------------------------ |
| TYPE            | 类，接口(注解在接口处似乎不起作用，可参考MySpringBoot项目)   |
| FIELD           | 成员变量                                                     |
| METHOD          | 方法                                                         |
| PARAMETER       | 方法参数                                                     |
| CONSTRUCTOR     | 构造方法                                                     |
| LOCAL_VARIABLE  | 局部变量([局部变量不支持运行时通过反射赋值][https://stackoverflow.com/questions/17237813/elementtype-local-variable-annotation-type]) |
| ANNOTATION_TYPE | 注解类                                                       |
| PACKAGE         | 包                                                           |
| TYPE_PARAMETER  | 类型参数                                                     |
| TYPE_USE        | 使用类型的任何地方                                           |

![1564491403839](D:\Typora\MKDFiles\media\1564491403839.png)



[@Retention]: 示需要在什么级别保存该注释信息，用于描述注解的生命周期

| 类型    | 生命周期                         |
| ------- | -------------------------------- |
| SOURCE  | 源文件保留                       |
| CLASS   | 编译器保留                       |
| RUNTIME | 运行期保留，可以反射获取注解信息 |

[@Documented]: 描述在使用javadoc工具为类生成帮助文档时是否要保留其注解信息	"生成DOc文档时候使用"
[@Inherited]: 使用它修饰的注解具有继承性	"子类可以继承父类的注解"



### 2. 内置注解   - 3个

- ***@Override***:   定义在 java.lang.Override 中 , 此注释只适用于修辞方法 , 表示一个方法声明打算 重写超类中的另一个方法声明，简单来说就是**子类在重写父类方法的时候可以加上这个注解**。



- ***@Deprecated***:   定义在java.lang.Deprecated中 , 此注释可以用于修辞方法 , 属性 , 类 , **表示不鼓励程序员使用这样的元素 , 通常是因为它很危险或者存在更好的选择**。即是此方法已经过时，不推使用。



- ***@SuppressWarnings***:   定义在 java.lang.SuppressWarnings 中,**用来抑制编译时的警告信息**，与前两个注释有所不同,你需要添加一个参数才能正确使用,这些参数都是已经定义好了的, 我们选择性的使用就好了 ，一般我们使用 @SuppressWarnings(“all”)



### 3. 自定义注解

自定义注解时使用 @interface 时，自动继承了了 java.lang.annotation.Annotation接口

在自定义注解中还可以设置参数，且只有一个参数的时候,默认使用value当做参数名字

自定义注解中定义参数的时候，后面的括号不能省略，且可以给一个默认的参数



## Ⅱ .  create myself Annotation

> Devided by @Target
>
> [spring aop的@Before,@Around,@After,@AfterReturn,@AfterThrowing的理解][https://blog.csdn.net/zhanglf02/article/details/78132304]
>
> [[Spring AOP @Before @Around @After 等 advice 的执行顺序](http://blog.csdn.net/rainbow702/article/details/52185827)]
>
> [[彻底征服 Spring AOP 之 理论篇](https://segmentfault.com/a/1190000007469968)]

---

***在 Spring AOP 中, join point 总是方法的执行点, 即只有方法连接点.***

---

### advice（通知）注解的执行先后顺序

#### 1. 一个方法只被一个Aspect类拦截

![1564716013496](D:\Typora\MKDFiles\media\1564716013496.png)

![1564716037247](D:\Typora\MKDFiles\media\1564716037247.png)

#### `解释`：执行到核心业务方法或者类时，会先执行AOP。在aop的逻辑内，先走@Around注解的方法。然后是@Before注解的方法，然后这两个都通过了，走核心代码，核心代码走完，无论核心有没有返回值，都会走@After方法。然后如果程序无异常，正常返回就走@AfterReturn,有异常就走@AfterThrowing。



#### 2. 同一个方法被多个Aspect类拦截

如果不指定Aspect的执行顺序，那么实际执行时是随机的。可以用以下方式进行指定执行顺序,不管采用哪种方法，都是**`越小`**的 aspect 越先执行。

- 实现**org.springframework.core.Ordered**接口，实现它的**getOrder()**方法
- 给aspect添加**@Order**注解，该注解全称为：org.springframework.core.annotation.Order

```java
@Order(5)
@Component
@Aspect
public class Aspect1 {
    // ...
}

@Order(6)
@Component
@Aspect
public class Aspect2 {
    // ...
}
```

![1564716707325](D:\Typora\MKDFiles\media\1564716707325.png)

**`注意点`**

- 如果在同一个 aspect 类中，**针对同一个 pointcut**，定义了两个相同的 advice(比如，定义了两个 @Before)，那么这两个 advice 的执行顺序是无法确定的，哪怕你给这两个 advice 添加了 @Order 这个注解，也不行。这点切记。

- 对于@Around这个advice，不管它有没有返回值，但是必须要方法内部，调用一下 pjp.proceed();否则，Controller 中的接口将没有机会被执行，从而也导致了 @Before这个advice不会被触发。

比如，我们假设正常情况下，执行顺序为”aspect2 -> apsect1 -> controller”，如果，我们把 **aspect1**中的**@Around**中的 **pjp.proceed();**给删掉，那么，我们看到的输出结果将是：

```txt
[Aspect2] around advise 1
[Aspect2] before advise
[Aspect1] around advise 1
[Aspect1] around advise2
[Aspect1] after advise
[Aspect1] afterReturning advise
[Aspect2] around advise2
[Aspect2] after advise
[Aspect2] afterReturning advise
```



---



### 1.  @Target({ ElementType.METHOD })  --- 日志打印

- Annotation

  ```groovy
  package cn.zheng.neptune.MySpringBoot.configuration.annotation
  
  import java.lang.annotation.Documented
  import java.lang.annotation.ElementType
  import java.lang.annotation.Retention
  import java.lang.annotation.RetentionPolicy
  import java.lang.annotation.Target
  
  @Target(ElementType.METHOD)
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  @interface AutoLog {
  	String level() default "info"
  }
  ```

- AOP

  ```groovy
  package cn.zheng.neptune.MySpringBoot.configuration.aspect
  
  import cn.zheng.neptune.MySpringBoot.configuration.annotation.AutoLog
  import org.apache.logging.log4j.LogManager
  import org.apache.logging.log4j.Logger
  import org.aspectj.lang.JoinPoint
  import org.aspectj.lang.annotation.Aspect
  import org.aspectj.lang.annotation.Before
  import org.aspectj.lang.annotation.Pointcut
  import org.aspectj.lang.reflect.MethodSignature
  import org.springframework.stereotype.Component
  
  import java.lang.reflect.Method
  
  @Aspect
  @Component
  class AutoLogAsp {
  	Logger logger = LogManager.getLogger(this)
  	@Pointcut("@annotation(cn.zheng.neptune.MySpringBoot.configuration.annotation.AutoLog)")
  	void pointCut(){}
  
  	@Before("pointCut()")
  	void beforeLog(JoinPoint joinPoint){
  		Class clazz = joinPoint.getClass() // 获取代理类
  
  
  		MethodSignature signature = joinPoint.getSignature() as MethodSignature  // 获取连接点方法签名
  		Method method = signature.getMethod() // 获取连接点方法名
  		Object object = joinPoint.getTarget() // 获取连接点类信息
  		Object[] param = joinPoint.getArgs()  // 获取连接点对象的参数信息
  
  		String messages = "Aspclass: ${clazz.getName()}. class: ${object.class}, method: ${method.getName()}, + Anno param: ${param} "
  
  		AutoLog autoLog = method.getAnnotation(AutoLog.class)
  		String level = autoLog.level()
  
  		switch (level){
  			case 'warn': logger.warn(messages);break
  			case 'error': logger.error(messages);break
  			default: logger.info(messages);break
  		}
  	}
  }
  ```

- Usage

  ```groovy
  package cn.zheng.neptune.MySpringBoot.controller
  
  import cn.zheng.neptune.MySpringBoot.configuration.annotation.AutoLog
  import cn.zheng.neptune.MySpringBoot.configuration.annotation.SystemDateTime
  import cn.zheng.neptune.MySpringBoot.dao.ConsumerDao
  import org.springframework.beans.factory.annotation.Autowired
  import org.springframework.stereotype.Controller
  import org.springframework.web.bind.annotation.RequestMapping
  import org.springframework.web.bind.annotation.RequestParam
  import org.springframework.web.bind.annotation.ResponseBody
  
  @Controller
  class MyController {
  
  	@Autowired
  	ConsumerDao dao
  
  	@RequestMapping("/")
  	@ResponseBody
  	@AutoLog(level = "warn")
  	public String test(@RequestParam name, @RequestParam password){
  		println("controller received the request: ${name}, ${password} ...")
  		return "success!"
  	}
  }
  ```

- Result

  ![1564711237864](D:\Typora\MKDFiles\media\1564711237864.png)



### 2. @Target({ ElementType.METHOD }) --- 拦截和校验

- Annotation

```groovy
package cn.zheng.neptune.MySpringBoot.configuration.annotation

import java.lang.annotation.Documented
import java.lang.annotation.ElementType
import java.lang.annotation.Retention
import java.lang.annotation.RetentionPolicy
import java.lang.annotation.Target

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@interface AutoLogFilter {
	String level() default "info"
	String access() default "User"
}
```

- AOP

```groovy
package cn.zheng.neptune.MySpringBoot.configuration.aspect


import cn.zheng.neptune.MySpringBoot.configuration.annotation.AutoLogFilter
import org.apache.logging.log4j.LogManager
import org.apache.logging.log4j.Logger
import org.aspectj.lang.JoinPoint
import org.aspectj.lang.ProceedingJoinPoint
import org.aspectj.lang.annotation.After
import org.aspectj.lang.annotation.Around
import org.aspectj.lang.annotation.Aspect
import org.aspectj.lang.annotation.Before
import org.aspectj.lang.annotation.Pointcut
import org.aspectj.lang.reflect.MethodSignature
import org.springframework.stereotype.Component

import java.lang.reflect.Method

@Aspect
@Component
class AutoLogFulterAsp {
	Logger logger = LogManager.getLogger(this)
	AutoLogFilter autoLog

	@Pointcut("@annotation(cn.zheng.neptune.MySpringBoot.configuration.annotation.AutoLogFilter)")
	void pointCut(){}

	@Before("pointCut()")
	void beforeLog(JoinPoint joinPoint){
		Class clazz = joinPoint.getClass() // 获取代理类


		MethodSignature signature = joinPoint.getSignature() as MethodSignature  // 获取连接点方法签名
		Method method = signature.getMethod() // 获取连接点方法名
		Object object = joinPoint.getTarget() // 获取连接点类信息
		Object[] param = joinPoint.getArgs()  // 获取连接点对象的参数信息

		String messages = "Aspclass: ${clazz.getName()}. class: ${object.class}, method: ${method.getName()}, + Anno param: ${param} "

		String level = autoLog.level()
		switch (level){
			case 'warn': logger.warn(messages);break
			case 'error': logger.error(messages);break
			default: logger.info(messages);break
		}
	}

	@Around("pointCut()")
	Object around(ProceedingJoinPoint joinPoint){ // ProceedingJoinPoint is only supported for around advice
		MethodSignature signature = joinPoint.getSignature() as MethodSignature  // 获取连接点方法签名
		Method method = signature.getMethod() // 获取连接点方法名
		autoLog = method.getAnnotation(AutoLogFilter.class)
		String role = autoLog.access()
		if(role == 'admin'){
			logger.info("Accreditation Success, proceeding your request now...")
			return joinPoint.proceed()
		}else {
			logger.error("Accreditation Fail...")
			return  "fail"
		}
	}



	@After("pointCut()")
	void after(){
		logger.info("ASP finished all function")
	}
}

```

- Usage

```groovy
package cn.zheng.neptune.MySpringBoot.controller


import cn.zheng.neptune.MySpringBoot.configuration.annotation.AutoLogFilter
import cn.zheng.neptune.MySpringBoot.dao.ConsumerDao
import org.springframework.beans.factory.annotation.Autowired
import org.springframework.stereotype.Controller
import org.springframework.web.bind.annotation.RequestMapping
import org.springframework.web.bind.annotation.RequestParam
import org.springframework.web.bind.annotation.ResponseBody

@Controller
class MyController {

	@Autowired
	ConsumerDao dao

	@RequestMapping("/")
	@ResponseBody
	@AutoLogFilter(level = "warn", access = "admin")
	public String test(@RequestParam name, @RequestParam password){
		println("controller received the request: ${name}, ${password} ...")
		return "success!"
	}
}
```



- Result

  - @AutoLogFilter(level = "warn", access = "admin")

  ![1564723821843](D:\Typora\MKDFiles\media\1564723821843.png)

  - @AutoLogFilter(level = "warn")

  ![1564723986107](D:\Typora\MKDFiles\media\1564723986107.png)



###### 3. @Target(value = [ElementType.FIELD, ElementType.LOCAL_VARIABLE])  --- 属性赋值

​	这个通过SpringAOP是做不到的，因为：在 Spring AOP 中, join point 总是方法的执行点, 即只有方法连接点.

