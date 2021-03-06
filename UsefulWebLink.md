1. [正则表达式性能测试](https://regex101.com/)
2. [正则表达式全集](http://tool.oschina.net/uploads/apidocs/jquery/regexp.html)
3. [ HTTP状态码详解 ](https://tool.oschina.net/commons?type=5)
4. [ASCII码表](https://www.litefeel.com/tools/ascii.php)
5. [Unicode字符集](https://www.rapidtables.com/code/text/unicode-characters.html)
6. [GC(Allocation Failure)引发的一些JVM知识点梳理](https://blog.csdn.net/zc19921215/article/details/83029952)
7. [Groovy里自定义JSON输出-JsonGenerator](https://my.oschina.net/wstone/blog/3094449)
8. [用于浏览源代码的网站](http://grepcode.com/)
9. [如何画好架构图](https://www.sohu.com/a/399858959_465221)
10. [七种开源许可证](https://www.jianshu.com/p/86251523e898)

![5420598-95ed1ef9be4caf3f](media/5420598-95ed1ef9be4caf3f.webp)

### UI

1. [BgRemover - 在线图片去底工具 - 将纯色背景的图片转换为背景透明的图片](http://www.aigei.com/bgremover/)

2. [SVG Sprites还原工具，内置Font Awesome小图标](https://www.zhangxinxu.com/sp/icon/)

3. [前端数据可视化echarts.js使用指南](https://www.cnblogs.com/st-leslie/p/5771241.html)

4. [vue项目中vue-echarts讲解及常用图表方案实现](https://blog.csdn.net/zhongguohaoshaonian/article/details/89405546)

5. [数据可视化echarts](https://echarts.apache.org/examples/zh/index.html)

6. [数据可视化vue-echarts](https://github.com/ecomfe/vue-echarts)

7. > [vue 项目使用echarts异步加载，xAxis坐标轴不显示](https://blog.csdn.net/weixin_39581226/article/details/83755746) 
   >
   > ![1592788454955](media/1592788454955.png)
   >
   > ``` js
   >     myEcharts () {
   >       var myChart = this.$echarts.init(document.getElementById('main'))
   > 
   >       myChart.setOption(this.option);
   >       myChart.setOption({
   >         xAxis: [
   >           {
   >             data: this.option.xAxis.data,
   >           }
   >         ]
   >       });
   >     }
   > ```
   >
   > 

8. [数据可视化AntV](https://antv.vision/zh)

9. [echarts antv 区别比较？](https://www.zhihu.com/question/57388387?sort=created)

10. [比站酷网更好的设计网站有哪些？](https://www.zhihu.com/question/20369808/answer/1218150388)

11. [D3.js：交互式操作](https://www.cnblogs.com/koto/p/5980693.html)

12. [在vue中使用基于d3为基础的dagre-d3.js搞定一个流程图组件](https://www.cnblogs.com/liushusong/p/11996770.html)

13. [jsPlumb 与 vue 学习总结](https://zhuanlan.zhihu.com/p/43642654)

14. [jsplumb 中文教程](https://wdd.js.org/jsplumb-chinese-tutorial/#/)

15. [jstree树形文件夹用vue.js组件格式做出来](https://blog.csdn.net/lx_axiao/article/details/54288455)

16. [npm: vue-tree](https://www.npmjs.com/package/vue-jstree)

17. [用JavaScript修改CSS属性的代码](https://www.cnblogs.com/aademeng/articles/6279042.html)

18. [vue中 getElementsByClassName().innerHtml无法给元素赋值 跪求大佬！](https://segmentfault.com/q/1010000014821304)



### Js

1. [js对象拷贝的方法](https://www.cnblogs.com/hjson/p/10243806.html)

   >  //不同的引用 var obj3 = JSON.parse(JSON.stringify(obj)); 

2. > :tipping_hand_man: **find**:
   >
   > ```js
   > ['x','xa','ax'].find(value => value.startsWith('x'))
   > ```
   >
   > ![1598421560413](media/1598421560413.png)
   >
   > :tipping_hand_man:**findAll:**
   >
   > ```js
   > ['x','xa','ax'].filter(value => value.startsWith('x'))
   > ```
   >
   > ![1598421653334](media/1598421653334.png)

3. [JS如何在数组指定位置插入元素](https://www.jb51.net/article/182300.htm)





### UI-Vue

1. [Vue的computed和watch的细节全面分析](https://segmentfault.com/a/1190000012948175?utm_source=tag-newest)

2. [vue中watch的几种用法](https://blog.csdn.net/wangbinXMU/article/details/97619725)

3. [Vue子组件调用父组件的方法](https://www.cnblogs.com/jin-zhe/p/9523782.html)

4. [解决vue keep-alive 数据更新的问题](https://www.jb51.net/article/147815.htm)

   > 在项目中使用<keep-alive>包含<router-view>实现页面缓存，加速页面加载，
   >
   > **同时，这种方式带来一些弊端，请看如下大神解释：**
   >
   > ***********************************
   >
   > 当引入keep-alive的时候，页面第一次进入，钩子的触发顺序created-> mounted-> activated，退出时触发deactivated。
   >
   > 当再次进入（前进或者后退）时，只触发activated。
   >
   > ***********************************
   >
   > 这就带来一个问题，之前在项目中使用mounted在页面加载时获取数据，使用<keep-alive>后方法不再生效，
   >
   > 根据上面的解释，将mounted替换为activated即可。

5. [Vue forEach 内方法递归调用](https://zhouyunfang.blog.csdn.net/article/details/104979037)

6. [Blog](https://github.com/ljianshu/Blog)

7. [vue中不能直接在html中使用import的js方法的问题和解决方法](https://blog.csdn.net/zongmaomx/article/details/107407504)

   > ![1600311607596](media/1600311607596.png)





### Oracle

1. [Oracle建立表空间和用户](https://blog.csdn.net/starnight_cbj/article/details/6792364)
2. [oracle导出表结构1](https://wenku.baidu.com/view/94d33a95a0116c175f0e4824.html)
3. [Oracle数据库联合主键](https://blog.csdn.net/long_long_ago1/article/details/82670911)
4. [Oracle之唯一性约束(UNIQUE Constraint)用法详解](https://blog.csdn.net/liuxiangke0210/article/details/78752275)
5. [oracle添加外键约束的两种方式](https://blog.csdn.net/lydia88/article/details/84500812)
6. [Oracle修改字段名、字段数据类型](https://www.cnblogs.com/fx-blog/p/7133538.html)
7. [Oracle创建表、删除表、修改表（添加字段、修改字段、删除字段）语句总结](https://www.linuxidc.com/Linux/2019-07/159430.htm)



### SpringBoot&Java

1. [ spring-boot-starters](https://github.com/spring-projects/spring-boot/tree/v2.1.0.RELEASE/spring-boot-project/spring-boot-starters)

2. > [模板引擎: FreeMarker](http://freemarker.foofun.cn/)   
   >
   > FreeMarker 是一款 *模板引擎*： 即一种基于模板和要改变的数据， 并用来生成输出文本(HTML网页，电子邮件，配置文件，源代码等)的通用工具。 它不是面向最终用户的，而是一个Java类库，是一款程序员可以嵌入他们所开发产品的组件。

3. [CAS原子操作以及其在Java中的应用](https://www.jianshu.com/p/973efae31be3)

4. [SpringBoot中使用SpringDataJPA](https://www.cnblogs.com/wadmwz/p/10313495.html)

5. [Spring Boot JPA 使用以及设置多个主键](https://blog.csdn.net/xx326664162/article/details/80053719)

6. [自动维护创建时间和更新时间](https://www.bbsmax.com/A/GBJre1DWz0/)

7. [jdbc:oracle:thin:@192.168.3.98:1521:orcl（详解）](https://blog.csdn.net/qingfeng45697/article/details/47779093)

8. [SpringBoot 统一时区的方案](https://www.jianshu.com/p/504c17b35e17)

9. [Springboot调用外部RestFul接口的几种方法](https://www.cnblogs.com/umrx/p/9387484.html)

10. [SpringBoot-RestTemplate实现调用第三方API](https://blog.csdn.net/a1032818891/article/details/81172478)

11. [Java实现将文件或者文件夹压缩成zip](https://www.cnblogs.com/zeng1994/p/7862288.html)

12. [spring-data-mongodb-remove-_class-define-explicitly](http://athlan.pl/spring-data-mongodb-remove-_class-define-explicitly/)

    ```java
    @Configuration
    public class AuthDataSourceConfiguration {
     
    	// ...
     
    	@Bean
    	public MongoClient mongoDbClient() throws Exception {
    		return new MongoClient(new ServerAddress("127.0.0.1"));
    	}
     
    	@Bean
    	public MongoDbFactory mongoDbFactory() throws Exception {
    		return new SimpleMongoDbFactory(mongoDbClient(), "dbname");
    	}
     
    	@Bean
    	public MongoTemplate mongoTemplate() throws Exception {
    		MongoTypeMapper typeMapper = new DefaultMongoTypeMapper(null);
            MappingMongoConverter converter = new MappingMongoConverter(mongoDbFactory(), new MongoMappingContext());
            converter.setTypeMapper(typeMapper);
     
    		MongoTemplate mongoTemplate = new MongoTemplate(mongoDbFactory(), converter);
    		return mongoTemplate;
    	}
    }
    ```

13. [Spring为IOC容器注入Bean的五种方式详解](https://www.jb51.net/article/173024.htm)

14. [SpringBoot+SpringSecurity+jwt整合及初体验](https://blog.csdn.net/qq_19006223/article/details/90732630)

15. [Spring Boot 中使用 Spring Security, OAuth2 跨域问题 (自己挖的坑)](https://www.cnblogs.com/victorbu/p/11178696.html)

16. [SpringBoot过滤器中的异常处理](https://zhuanlan.zhihu.com/p/134214309)

17. [springboot(五)——springboot中的拦截器和过滤器小结](https://segmentfault.com/a/1190000019005504)




### SpringBoot Data JPA

1. [Spring Data JPA，一种动态条件查询的写法](https://www.cnblogs.com/derry9005/p/6282571.html)
2. [SpringBoot中使用SpringDataJPA](https://www.cnblogs.com/wadmwz/p/10313495.html)
3. [java-jpa-criteriaBuilder使用](https://www.cnblogs.com/g-smile/p/9177841.html)
4. [spring mongodb分页，动态条件、字段查询](https://www.bbsmax.com/A/1O5EnjYbd7/)
5. [springboot mongodb jpa常用方法整理](https://www.cnblogs.com/zincredible/p/9206655.html)



### SpringBoot Mybatis

1. [spring boot 如何优雅的使用mybatis-spring-boot-starter](https://blog.csdn.net/zmx729618/article/details/80773887)
2. [Spring Boot中集成Mybatis(动态拼接SQL)](https://blog.csdn.net/hdn_kb/article/details/100139885)
3. [MyBatis中的@results注解使用](https://blog.csdn.net/weixin_44149454/article/details/90373036)
4. [Spring Data Specification的用法,group by,order by及复杂情况和Pageable的注意事项](https://blog.csdn.net/qq_36564291/article/details/88717082)



### Springboot MongoDB

1. [Spring Data Jpa：分页、Specification、Criteria](https://www.jianshu.com/p/e7882c4f29b6)
2. [spring data jpa Specification 复杂查询+分页查询](https://www.cnblogs.com/hankuikui/p/11414316.html)
3. [MongoDB操作符之$elemMatch](https://www.cnblogs.com/SwordArt/p/12588365.html)
4. [ MongoDB——$elemMatch(内嵌文档查询匹配) ](https://blog.csdn.net/shiyaru1314/article/details/68496642)
5. [mongodb详细优化策略方案](https://blog.csdn.net/Felix_CB/article/details/86296890)
6. [MongoDB第八讲查询优化](https://www.jianshu.com/p/3ae79de4caae)
7. [MongoDB查询优化-MongoDB Profiler](https://www.cnblogs.com/operationhome/p/10728654.html)
8. [spring-data-mongodb查询结果返回指定字段](https://www.cnblogs.com/usual2013blog/p/4103549.html)
9. [java大文件分块上传分享](https://blog.csdn.net/weixin_42584752/article/details/80873376)
10. [大文件上传第二弹(分片、秒传、断点续传)](https://blog.csdn.net/haohao123nana/article/details/54692669)
11. [SpringBoot下载文件实现及速度对比](https://blog.csdn.net/m0_38001814/article/details/89182120)
12. [mongodb海量数据CRUD优化](https://www.cnblogs.com/xiaoqi/p/java-mongo-crud.html)



### Excel

1. [Excel 找出两列中的相同或不同的数据](https://jingyan.baidu.com/article/19020a0a68bd5d529d28429d.html)



### JSON

1. [Free Online JSON to JSON Schema Converter](https://www.liquid-technologies.com/online-json-to-schema-converter)

   > include 
   >
   > ​	XML format,valid, XML <-----> XSD
   >
   > ​	JSON format,valid, JSON<-----> JSON Schema



### Docker

1. [【docker-compose】docker-compose.yml文本内容详解 + docker-compose命令详解 + docker-compose启动服务容器时区设置](https://www.cnblogs.com/sxdcgaq8080/p/10072040.html)



### Nginx

1. [每天一个linux命令13之curl发送http请求](https://www.cnblogs.com/edgedance/p/7096660.html)
2. [nginx docker容器配置https(ssl)](https://segmentfault.com/a/1190000017753319)
3. [HTTPs setup - Certbot + Docker + Nginx](https://www.jianshu.com/p/a4692f1e3208)
4. [Nginx https配置 和 反向代理到spring boot和vue.js](https://segmentfault.com/a/1190000016760251)(return 301 https://$server_name$request_uri;有bug),可参考 [nginx.conf](files\nginx.conf) 



### Java 23种设计模式

1. [工厂模式](https://www.cnblogs.com/long88-club/p/11055555.html)

2. [Java将域名转换成IP](https://blog.csdn.net/liang_k/article/details/40623911)

   ```java
   InetAddress.getByName(domainName).getHostAddress()
   ```

3. [HttpRequest获得服务端和客户端的详细信息](https://www.cnblogs.com/lhwblog/p/6670308.html)

   ```
   request.setCharacterEncoding("utf-8");//设置request编码方式
   request.getLocalAddr();//获取本地IP，即服务器IP
   request.getLocalName();//获取本地名称，即服务器名称
   request.getLocalPort();//获取本地端口号，即Tomcat端口号
   request.getLocale();//用户的语言环境
   request.getContextPath();//context路径
   request.getMethod();//GET还是POST
   request.getProtocol();//协议，http协议
   request.getQueryString();//查询字符串
   request.getRemoteAddr();//远程IP，即客户端IP
   request.getRemotePort();//远程端口，即客户端端口
   request.getRemoteUser();//远程用户
   request.getRequestedSessionId();//客户端的Session的ID
   request.getRequestURI();//用户请求的URL
   request.getScheme();//协议头，例如http
   request.getServerName();//服务器名称
   request.getServerPort();//服务器端口
   
   request.getServletPath();//Servlet路径
   ```

   

### Unit Test

1. [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations)

2. [JUnit 5学习心得](https://www.jianshu.com/p/675b74549f77)

3. [Junit5简介、构成、新特性及基本使用-常用注解、套件执行](https://juejin.im/post/6844903968955432967#heading-0)

4. [JUnit@Before失效问题](https://www.cnblogs.com/king0207/p/13576894.html)

5. [@InjectMocks 类内部@Autowired bean 为null的解决办法](https://stackoverflow.com/questions/52450754/null-pointer-on-an-autowired-bean-which-is-not-mocked-by-mockito)

   > Usually when unit testing you want to mock *all* external dependencies of a class. That way the unit test can remain independent and focused on the class under test.
   >
   > Nevertheless, if you want to mix Spring autowiring with Mockito mocks, an easy solution is to annotate with both `@InjectMocks` and `@Autowired`:
   >
   > ```java
   > @BeforeEach
   >     public void init(){
   >         MockitoAnnotations.initMocks(this); //在测试方法运行之前，自动把用@Mock标注过的field实例化成Mock对象(https://www.jianshu.com/p/7f6a1d3aa516)
   >     }  
   > 
   > @InjectMocks
   >   @Autowired
   >   private UploadServiceImpl uploadService;
   > ```

6. [Mockito when(...).thenReturn(...)和doReturn(...).when(...)的区别](https://www.cnblogs.com/lanqi/p/7865163.html)



### Typora

1. [ Typora 完全使用详解 ](https://sspai.com/post/54912)
2. [Mermaid Live Editor(画图)](https://mermaid-js.github.io/mermaid-live-editor/#/edit/eyJjb2RlIjoiZ3JhcGggVERcbkFbQ2hyaXN0bWFzXSAtLT58R2V0IG1vbmV5fCBCKEdvIHNob3BwaW5nKVxuQiAtLT4gQ3tMZXQgbWUgdGhpbmt9XG5DIC0tPnxPbmV8IERbTGFwdG9wXVxuQyAtLT58VHdvfCBFW2lQaG9uZV1cbkMgLS0-fFRocmVlfCBGW2ZhOmZhLWNhciBDYXJdXG4iLCJtZXJtYWlkIjp7InRoZW1lIjoiZGVmYXVsdCJ9fQ)







