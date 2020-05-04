# Groovy Usage Skills

### 一、 Groovy Web Console Implement

- [Java动态调用Groove代码（2）-GroovyScriptEngine](https://blog.csdn.net/chy2z/article/details/82729691)

>  use  **Binding**  to pass the params
>
>  use  **engine.run("FunArgGroove.groovy", binding2)**  to execute the script

***couldn't support invoke specific method of groovy script***

- [Integrating Groovy into Java Applications](https://www.baeldung.com/groovy-java-applications)

```java
		Class<GroovyObject> calcClass = engine.loadScriptByName(scriptName)
		GroovyObject calc = (GroovyObject)calcClass.newInstance()
		calc.invokeMethod("main", variables)
```



> use **engine.loadScriptByName(scriptName)** to load groovy script
>
> use **calc.invokeMethod("main", variables)** to invoke params and execute specific function

