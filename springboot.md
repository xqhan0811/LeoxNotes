# SpringBoot 



## 1. 创建对象的方式

springboot 去 xml 配置,使用注解配置

#### 1.1 创建自定义的对象

- @Component
- @Service
- @Controller
- @Repository



#### 1.2 创建已有的对象,第三方提供的对象

- ##### @Configuration 管理别人写的类

创建一个配置类,创建方法

```java
@Configuration //相当于 xml 配置文件
public class BeanConfig {

    @Bean //每一个方法代表一个创建的对象
    public Calendar getCalendar(){
        return Calendar.getInstance();
    }
}
```





## 2. springboot 中注入方式

yml配置文件不会自动提示自定义对象的属性,可以加入如下坐标:[官方地址](https://docs.spring.io/spring-boot/docs/2.3.5.RELEASE/reference/html/appendix-configuration-metadata.html#configuration-metadata-annotation-processor)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

重启idea即可



#### 2.1 @Value 

给类中注入注入属性



#### 2.2 @ConfigurationProperties

如果属性过多,使用`@Value`注解不太方便,则可以使用`@ConfigurationProperties`注解



## 3. 集成jsp(目前实际项目中基本不使用)

#### 3.1 引入 jsp 依赖 jar

```xml
<!--jsp 中使用jstl-->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
</dependency>
<!--核心-->
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
    <scope>provided</scope>
</dependency>
<!--这个不需要引入,因为在引入springboot的web时候已经有相关的依赖引入了这个jar-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
</dependency>
```



#### 3.2 配置视图解析

yml配置文件中配置

```yml
spring:
  mvc:
    view:
      prefix: /WEB-INF/view/
      suffix: .jsp
```



#### 3.3 运行jsp



###### 3.3.1 方式一:插件

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

双击 `spring-boot:run`



#### 3.4 开启jsp热部署

yml配置文件中 加入

```yaml
server:
  servlet:
    jsp:
      init-parameters:
        development: true 
```





## 4. 整合 MyBatis

> 回顾 spring 整合 mybatis

1. 引入依赖
   1. spring mybatis mybatis-spring druid mysql lombok log4j
2. 建表
3. 开发实体类
4. DAO 接口
5. Mapper 映射文件
6. service 接口
7. service 实体类    @Service      @Transactional        注入DAO相关对象
8. 配置文件    spring.xml
   1. 引入小配置文件
   2. 开启注解扫描
   3. 创建数据源对象    DruidDataSource    driverClassName    url     username   password
   4. 创建 SqlSessionFactory    注入DataSource       注入Mapper配置文件位置       注入别名等
   5. 创建DAO  MapperScannerConfigurer   注入SqlSessionFactory    注入DAO接口所在的包
   6. 创建事务管理器    DataSourceTransactionManager    注入DataSource
   7. 开启注解事务生效





#### 4.1 引入依赖

######  4.1.1 mybatis 和 springboot的依赖

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.3</version>
</dependency>
```



###### 4.1.2 mysql 依赖

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.49</version>
</dependency>
```



###### 4.1.2 druid 依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.3</version>
</dependency>
```



#### 4.2 配置文件

###### 4.2.1 yml

```yaml
spring:
  profiles:
    active: prod
  mvc:
    view:
      prefix: /WEB-INF/view/
      suffix: .jsp

  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm
    username: root
    password: 123456

mybatis:
  mapper-locations: classpath:com/leox/mapper/*.xml
  type-aliases-package: com.leox.pojo
```



###### 4.2.2 启动类中添加 dao 接口的扫描注解

>  @MapperScan("com.leox.dao")

```java
@SpringBootApplication
@MapperScan("com.leox.dao")
public class SbootApplication {
	public static void main(String[] args) {
		SpringApplication.run(SbootApplication.class, args);
	}
}

```



完成以上步骤后,接下去我们就可以

- 建表

- 实体类

- DAO接口

- Mapper配置文件
- service接口
- service实现类     @Service      @Transactional        注入DAO相关对象





## 5. springboot 整合 junit 测试

###### 5.1 引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```



###### 5.2 基础测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = SbootApplication.class)
public class BaseTest {
}
```

其他测试类集成基础测试类



## 6. springboot 日志 logback

