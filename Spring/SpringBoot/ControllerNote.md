# 					Tips for Controller level

### 1. @CrossOrigin(跨域请求)

>@CrossOrigin  is used for front and back end separation（前后端分离），Cross-domain requests

```groovy
@RestController  // for html, use RestController(@Responsebody + @Controller); for jsp, could only use @Controller
@CrossOrigin
public class LoginController {
    protected final Logger logger = LogManager.getLogger(this.getClass());

    @Autowired
    private UserService userService;
 }
    
```

### 2. RedirectView(视图重定向)  --- 重定向一定是get请求方式

- https://www.cnblogs.com/fangjian0423/p/springMVC-redirectView-analysis.html
- https://www.baeldung.com/spring-redirect-and-forward

> RedirectView params usage:
>
> - @param url:  the URL to redirect to
> - @param contextRelative:  whether to interpret the given URL asrelative to the current ServletContext
> - @param http10: Compatible whether to stay compatible with HTTP 1.0 clients
> - @param exposeModelAttributes:  whether or not model attributes should be (false: POST, true: GET)

```groovy
    @RequestMapping(value = {"/"})
    public RedirectView indexPage(){
        logger.warn("get the root route and redirect to login method");
        return new RedirectView("/loginVerify",true,false,false);
    }
```

### 3.How to get json data with @RequestBody params

[REF]: https://www.cnblogs.com/xuyatao/p/8296095.html

![1564133305177](C:\Users\ZHENGNE\AppData\Roaming\Typora\typora-user-images\1564133305177.png)

- **@RequestBody JSONObject**

  ```java
  import com.alibaba.fastjson.JSONObject;
  
  @RequestMapping(value = {"/loginVerify"}, method {RequestMethod.POST,RequestMethod.GET})
      @ResponseBody
      public boolean loginPage(@RequestBody JSONObject json){
          logger.info("received loginVerify post request ..... " + json);
          User user = json.toJavaObject(User.class);
          return userService.loginOrRegister(user.getName(),user.getPassword());
      }
  ```

- **@RequestBody Object**

  ```java
  @RequestMapping(value = {"/loginVerify"}, method = {RequestMethod.POST,RequestMethod.GET})
      @ResponseBody
      public boolean loginPage(@RequestBody Object object){
          logger.info("received loginVerify post request ..... " + object);
          User user = (User) object;
          return userService.loginOrRegister(user.getName(),user.getPassword());
      }
  ```

- **@RequestBody User**

  ```java
  @RequestMapping(value = {"/loginVerify"}, method = {RequestMethod.POST,RequestMethod.GET})
      @ResponseBody
      public boolean loginPage(@RequestBody User user){
          logger.info("received loginVerify post request ..... " + user);
          return userService.loginOrRegister(user.getName(),user.getPassword());
      }
  ```

  





