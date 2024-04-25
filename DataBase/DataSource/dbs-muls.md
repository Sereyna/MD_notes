# 多数据源配置
## 添加多数据源的配置
先在Spring Boot的配置文件application.properties中设置两个你要链接的数据库配置，比如这样：

```java
spring.datasource.primary.jdbc-url=jdbc:mysql://localhost:3306/test1
spring.datasource.primary.username=root
spring.datasource.primary.password=123456
spring.datasource.primary.driver-class-name=com.mysql.cj.jdbc.Driver

spring.datasource.secondary.jdbc-url=jdbc:mysql://localhost:3306/test2
spring.datasource.secondary.username=root
spring.datasource.secondary.password=123456
spring.datasource.secondary.driver-class-name=com.mysql.cj.jdbc.Driver
```
说明与注意：
1. 多数据源配置的时候，与单数据源不同点在于spring.datasource之后多设置一个数据源名称primary和secondary来区分不同的数据源配置，这个前缀将在后续初始化数据源的时候用到。
2. 数据源连接配置2.x和1.x的配置项是有区别的：2.x使用spring.datasource.secondary.jdbc-url，而1.x版本使用spring.datasource.secondary.url。如果你在配置的时候发生了这个报错java.lang.IllegalArgumentException: jdbcUrl is required with driverClassName.，那么就是这个配置项的问题。
3. 可以看到，不论使用哪一种数据访问框架，对于数据源的配置都是一样的。

## 初始化数据源与MyBatis配置
完成多数据源的配置信息之后，就来创建个配置类来加载这些配置信息，初始化数据源，以及初始化每个数据源要用的MyBatis配置。

这里我们继续将数据源与框架配置做拆分处理：

1. 单独建一个多数据源的配置类，比如下面这样：
```java
@Configuration
public class DataSourceConfiguration {

    @Primary
    @Bean
    @ConfigurationProperties(prefix = "spring.datasource.primary")
    public DataSource primaryDataSource() {
        return DataSourceBuilder.create().build();
    }

    @Bean
    @ConfigurationProperties(prefix = "spring.datasource.secondary")
    public DataSource secondaryDataSource() {
        return DataSourceBuilder.create().build();
    }

}
```

2. 分别创建两个数据源的MyBatis配置。
Primary数据源的MyBatis配置：
```java
@Configuration
@MapperScan(
        basePackages = "com.didispace.chapter39.p",
        sqlSessionFactoryRef = "sqlSessionFactoryPrimary",
        sqlSessionTemplateRef = "sqlSessionTemplatePrimary")
public class PrimaryConfig {

    private DataSource primaryDataSource;

    public PrimaryConfig(@Qualifier("primaryDataSource") DataSource primaryDataSource) {
        this.primaryDataSource = primaryDataSource;
    }

    @Bean
    public SqlSessionFactory sqlSessionFactoryPrimary() throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(primaryDataSource);
        return bean.getObject();
    }

    @Bean
    public SqlSessionTemplate sqlSessionTemplatePrimary() throws Exception {
        return new SqlSessionTemplate(sqlSessionFactoryPrimary());
    }

}
```
Secondary数据源的MyBatis配置：

```java
@Configuration
@MapperScan(
        basePackages = "com.didispace.chapter39.s",
        sqlSessionFactoryRef = "sqlSessionFactorySecondary",
        sqlSessionTemplateRef = "sqlSessionTemplateSecondary")
public class SecondaryConfig {

    private DataSource secondaryDataSource;

    public SecondaryConfig(@Qualifier("secondaryDataSource") DataSource secondaryDataSource) {
        this.secondaryDataSource = secondaryDataSource;
    }

    @Bean
    public SqlSessionFactory sqlSessionFactorySecondary() throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(secondaryDataSource);
        return bean.getObject();
    }

    @Bean
    public SqlSessionTemplate sqlSessionTemplateSecondary() throws Exception {
        return new SqlSessionTemplate(sqlSessionFactorySecondary());
    }

}
```
说明与注意：

1. 配置类上使用@MapperScan注解来指定当前数据源下定义的Entity和Mapper的包路径；另外需要指定sqlSessionFactory和sqlSessionTemplate，这两个具体实现在该配置类中初始化。
2. 配置类的构造函数中，通过@Qualifier注解来指定具体要用哪个数据源，其名字对应在DataSourceConfiguration配置类中的数据源定义的函数名。
3. 配置类中定义SqlSessionFactory和SqlSessionTemplate的实现，注意具体使用的数据源正确（如果使用这里的演示代码，只要第二步没问题就不需要修改）。

根据上面Primary数据源的定义，在com.didispace.chapter39.p包下，定义Primary数据源要用的实体和数据访问对象，比如下面这样：

```java
@Data
@NoArgsConstructor
public class UserPrimary {

    private Long id;

    private String name;
    private Integer age;

    public UserPrimary(String name, Integer age) {
        this.name = name;
        this.age = age;
    }
}

public interface UserMapperPrimary {

    @Select("SELECT * FROM USER WHERE NAME = #{name}")
    UserPrimary findByName(@Param("name") String name);

    @Insert("INSERT INTO USER(NAME, AGE) VALUES(#{name}, #{age})")
    int insert(@Param("name") String name, @Param("age") Integer age);

    @Delete("DELETE FROM USER")
    int deleteAll();

}
```

根据上面Secondary数据源的定义，在com.didispace.chapter39.s包下，定义Secondary数据源要用的实体和数据访问对象，比如下面这样：

```java
@Data
@NoArgsConstructor
public class UserSecondary {

    private Long id;

    private String name;
    private Integer age;

    public UserSecondary(String name, Integer age) {
        this.name = name;
        this.age = age;
    }
}

public interface UserMapperSecondary {

    @Select("SELECT * FROM USER WHERE NAME = #{name}")
    UserSecondary findByName(@Param("name") String name);

    @Insert("INSERT INTO USER(NAME, AGE) VALUES(#{name}, #{age})")
    int insert(@Param("name") String name, @Param("age") Integer age);

    @Delete("DELETE FROM USER")
    int deleteAll();
}
```


【引用】

https://blog.didispace.com/spring-boot-learning-21-3-9/