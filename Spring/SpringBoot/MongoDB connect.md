# Setup MongoDB connect with JPA YMAL

- **date source info**

| properties      | details         |
| :-------------- | :-------------- |
| URL : Port      | localhost:27017 |
| DB              | MySpringBoot    |
| Name / Password | test / test123  |

1. add mongoDB starter 

   ```gr
   implementation 'org.springframework.boot:spring-boot-starter-data-mongodb'
   ```

2. add yml setup

   ```yaml
   spring:
     autoconfigure:
       exclude: //since mongodb not use DataSource
         - org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaConfiguration
     data:
       mongodb:
         uri: mongodb://localhost:27017/MySpringBoot
         username: test
         password: test123
   ```

   **Tips:**

​                  a. use spring.autoconfigure.exclude unused or conflict bean(here is no use since MongoDB no use jpa )

3. Entity & Dao code

   --------

   ***Entity***

   ```java
   package cn.zheng.neptune.MySpringBoot.vo
   
   import org.springframework.data.annotation.Id
   import org.springframework.data.mongodb.core.mapping.Document
   
   @Document
   class Consumer {
   	@Id
   	private int id
   	private String name
   	private String password
   	private int age
   
   	Consumer() {
   	}
   
   	Consumer(String name, String password, int age) {
   		this.name = name
   		this.password = password
   		this.age = age
   	}
   
   	int getId() {
   		return id
   	}
   
   	void setId(int id) {
   		this.id = id
   	}
   
   	String getName() {
   		return name
   	}
   
   	void setName(String name) {
   		this.name = name
   	}
   
   	String getPassword() {
   		return password
   	}
   
   	void setPassword(String password) {
   		this.password = password
   	}
   
   	int getAge() {
   		return age
   	}
   
   	void setAge(int age) {
   		this.age = age
   	}
   
   
   	@Override
   	public String toString() {
   		return "Consumer{" +
   				"id=" + id +
   				", name='" + name + '\'' +
   				", password='" + password + '\'' +
   				", age=" + age +
   				'}';
   	}
   }
   
   ```

   ----

   ***Dao***

   ```java
   package cn.zheng.neptune.MySpringBoot.dao
   
   import cn.zheng.neptune.MySpringBoot.vo.Consumer
   import org.springframework.data.mongodb.repository.MongoRepository
   import org.springframework.data.mongodb.repository.Query
   import org.springframework.stereotype.Repository
   
   @Repository
   interface ConsumerDao extends MongoRepository<Consumer,Integer> {
   }
   ```

   

   