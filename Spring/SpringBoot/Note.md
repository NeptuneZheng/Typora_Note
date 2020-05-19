# 			Spring Usages Notes

## Ⅰ. How to create bean

- [在SpringBoot中将properties配置文件中的信息注入到实体Bean中][https://blog.csdn.net/qq_40378034/article/details/88054095]

## Ⅱ. How to use JMS

- [JMS 在 SpringBoot 中的使用](https://segmentfault.com/a/1190000009682775)
- [手把手教你如何玩转消息中间件（ActiveMQ）][https://blog.csdn.net/cs_hnu_scw/article/details/81040834]

### 一. use ymal setup 

```gradle
implementation 'org.fusesource.hawtbuf:hawtbuf'
implementation 'org.springframework.boot:spring-boot-starter-activemq'
```



```yml
spring:
   activemq:
     broker-url: tcp://127.0.0.1:61616
     user: admin
     password: admin
```

```java
@Service
class JMSService {
	@Resource
	JmsTemplate jmsTemplate

	void sendMessage(String message){
		jmsTemplate.convertAndSend("TEST.IN.QUE",message)
	}


}
```

### 二.use ConnectionFactory

```java
package csdp.b2b.load.test.factory


import org.apache.activemq.ActiveMQConnectionFactory
import org.springframework.beans.factory.annotation.Autowired
import org.springframework.context.annotation.Bean
import org.springframework.jms.core.JmsTemplate
import org.springframework.stereotype.Component

import javax.jms.ConnectionFactory

@Component
class JMSConnectFactory {
	@Autowired
	MyBeanFactory beanFactory

	@Bean
	ConnectionFactory tibConnectFactory(){
		ActiveMQConnectionFactory connectionFactory = new ActiveMQConnectionFactory()
		connectionFactory.setBrokerURL("tcp://127.0.0.1:61616")
		connectionFactory.setUserName("admin")
		connectionFactory.setPassword("admin")

		println("ConnectionFactory")
		return connectionFactory
	}

	@Bean(name = "myJmsTemplate")
	JmsTemplate jmsTemplate(){
		println("JmsTemplate")
		return new JmsTemplate(tibConnectFactory())
	}
}
```

```java
package csdp.b2b.load.test.service


import org.springframework.jms.core.JmsTemplate
import org.springframework.stereotype.Service

import javax.annotation.Resource

@Service
class JMSService {
	@Resource(name = "myJmsTemplate")
	JmsTemplate jmsTemplate

	void sendMessage(String message){
		jmsTemplate.convertAndSend("TEST.IN.QUE",message)
	}


}

```



## Ⅲ.  how to get yml properties

- [@ConfigurationProperties获取值和@Value获取值比较](https://blog.csdn.net/clmmei_123/article/details/81871836)

|                     | @ConfigurationProperties | @Value       |
| ------------------- | ------------------------ | ------------ |
| 功能                | 批量注入配置文件中的属性 | 一个一个注入 |
| 松散绑定（松散语法) | 支持                     | 不支持       |
| SpEL语法( #{ } )    | 不支持                   | 支持         |
| JSR303数据校验      | 支持                     | 不支持       |
| 复杂类型封装        | 支持                     | 不支持       |





```yml
jwt:
  header: test
  secret: kkkkk
  expire: 3600
```

```groovy
class Test {
	private String header
	private String secret
	private long expire
}
```



### 一. use @Value

```groovy
class Test {
    @Value(value = '${jwt.header}')
	private String header
    @Value(value = '${jwt.secret}')
	private String secret
    @Value(value = '${jwt.expire}')
	private long expire
}
```



### 二. use @ConfigurationProperties

```gradle
implementation 'org.springframework.boot:spring-boot-configuration-processor'
```

```groovy
@Component
@ConfigurationProperties(prefix = "jwt", ignoreUnknownFields = true)
class Test {
	private String header
	private String secret
	private long expire
    
    //@ConfigurationProperties will injection by setter functions
    //can't use lombok's @Setter annotation as an alternative
    void setHeader(String header) {
		this.header = header
	}

	void setSecret(String secret) {
		this.secret = secret
	}

	void setExpire(long expire) {
		this.expire = expire
	}
}


```

