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
		jmsTemplate.convertAndSend("CS.B2B.FWK2.MCI2.JOB_REQ.IN.QUE",message)
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
		jmsTemplate.convertAndSend("CS.B2B.FWK2.MCI2.JOB_REQ.IN.QUE",message)
	}


}

```

