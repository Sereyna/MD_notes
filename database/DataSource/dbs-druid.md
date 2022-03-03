# 数据源（Druid）
Druid是由阿里巴巴数据库事业部出品的开源项目。它除了是一个高性能数据库连接池之外，更是一个自带监控的数据库连接池。虽然HikariCP已经很优秀，但是对于国内用户来说，可能对于Druid更为熟悉。所以，对于如何在Spring Boot中使用Druid是后端开发人员必须要掌握的基本技能。
## 配置Druid数据源
第一步：在`pom.xml`中引入druid官方提供的Spring Boot Starter封装。

```java
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.21</version>
</dependency>
```

第二步：在`application.properties`中配置数据库连接信息。

Druid的配置都以`spring.datasource.druid`作为前缀，所以根据之前的配置，稍作修改即可：

```java
spring.datasource.druid.url=jdbc:mysql://localhost:3306/test
spring.datasource.druid.username=root
spring.datasource.druid.password=
spring.datasource.druid.driver-class-name=com.mysql.cj.jdbc.Driver
```

第三步：配置Druid的连接池。

与Hikari一样，要用好一个数据源，就要对其连接池做好相应的配置，比如下面这样：
```java
spring.datasource.druid.initialSize=10
spring.datasource.druid.maxActive=20
spring.datasource.druid.maxWait=60000
spring.datasource.druid.minIdle=1
spring.datasource.druid.timeBetweenEvictionRunsMillis=60000
spring.datasource.druid.minEvictableIdleTimeMillis=300000
spring.datasource.druid.testWhileIdle=true
spring.datasource.druid.testOnBorrow=true
spring.datasource.druid.testOnReturn=false
spring.datasource.druid.poolPreparedStatements=true
spring.datasource.druid.maxOpenPreparedStatements=20
spring.datasource.druid.validationQuery=SELECT 1
spring.datasource.druid.validation-query-timeout=500
spring.datasource.druid.filters=stat
```

## 配置Druid监控

第一步：在`pom.xml`中引入`spring-boot-starter-actuator`模块
```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

第二步：在`application.properties`中添加Druid的监控配置。

```java
spring.datasource.druid.stat-view-servlet.enabled=true
spring.datasource.druid.stat-view-servlet.url-pattern=/druid/*
spring.datasource.druid.stat-view-servlet.reset-enable=true
spring.datasource.druid.stat-view-servlet.login-username=admin
spring.datasource.druid.stat-view-servlet.login-password=admin
```

上面的配置主要用于开启stat监控统计的界面以及监控内容的相关配置，具体释意如下：

- `spring.datasource.druid.stat-view-servlet.url-pattern`：访问地址规则
- `spring.datasource.druid.stat-view-servlet.reset-enable`：是否允许清空统计数据
- `spring.datasource.druid.stat-view-servlet.login-username`：监控页面的登录账户
- `spring.datasource.druid.stat-view-servlet.login-password`：监控页面的登录密码

**注意**：这里的所有监控信息是对这个应用实例的数据源而言的，而并不是数据库全局层面的，可以视为应用层的监控，不可能作为中间件层的监控。

【引用】

https://blog.didispace.com/spring-boot-learning-21-3-3/