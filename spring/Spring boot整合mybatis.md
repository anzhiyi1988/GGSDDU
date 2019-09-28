# 环境依赖

```xml
<dependency>
  <groupId>org.mybatis.spring.boot</groupId>
  <artifactId>mybatis-spring-boot-starter</artifactId>
  <version>1.1.1</version>
</dependency>

<dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>42.2.8</version>
</dependency>

<dependency>
    <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.0.14</version>
</dependency>
```

# 数据源

有两种配置数据源的方式

#### Spring boot 默认数据源

在resources/application.properties中配置数据源信息

```properties
#数据库
spring.datasource.url=jdbc:postgresql://172.16.33.242:5432/d2d_anzhy
spring.datasource.username=postgres
spring.datasource.password=postgres
spring.datasource.driver-class-name=org.postgresql.Driver
```



#### 自定义数据源

在resources/config/source.properties中配置数据源信息

```properties
# mysql
source.driverClassName = com.mysql.jdbc.Driver
source.url = jdbc:mysql://localhost:3306/springboot_db?useUnicode = true&characterEncoding=UTF-8&zeroDateTimeBehavior=convertToNull
source.username = root
source.password = root
```

通过java config创建datasource和jdbcTemplate

```java
@Configuration
@EnableTransactionManagement
@PropertySource(value = {"classpath:config/source.properties"})
public class BeanConfig {
 
    @Autowired
    private Environment env;
 
    @Bean(destroyMethod = "close")
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(env.getProperty("source.driverClassName").trim());
        dataSource.setUrl(env.getProperty("source.url").trim());
        dataSource.setUsername(env.getProperty("source.username").trim());
        dataSource.setPassword(env.getProperty("source.password").trim());
        return dataSource;
    }
 
@Bean
    public JdbcTemplate jdbcTemplate() {
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        jdbcTemplate.setDataSource(dataSource());
        return jdbcTemplate;
    }
}
```





http://blog.720ui.com/2016/springboot_02_data_mybatis/