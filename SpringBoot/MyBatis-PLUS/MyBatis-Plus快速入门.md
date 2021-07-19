# MyBatis-Plus快速入门.
MyBatis-Plus官网:https://mybatis.plus/
此项目参考地址:https://mybatis.plus/guide/quick-start.html#%E5%88%9D%E5%A7%8B%E5%8C%96%E5%B7%A5%E7%A8%8B

### 首先创建一张user表:
```sql
DROP TABLE IF EXISTS user;
CREATE TABLE user
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);
--插入数据
INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

### 在Maven中引入MyBatis-Plus与其他坐标:
```html
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <!-- 引入MyBatis-PLUS依赖 -->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>Latest Version</version>
    </dependency>
    <!-- 引入MyBatis驱动依赖 -->
    <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.0</version>
    </dependency>
    <!-- 引入MySQL驱动依赖 -->
     <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
    </dependency>
    <!-- 引入lombok依赖 -->
    <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
    </dependency>
</dependencies>
```

### 在SpringBoot核心配置文件中配置连接属性:
```properties
#连接字符串 
spring.datasource.url=jdbc:mysql://localhost:3306/数据库名?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#设置MyBatis驼峰命名模式
mybatis.configuration.map-underscore-to-camel-case=true
```

### 编写实体类User:
```java
@Data
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

### 编写Mapper类继承与BaseMapper<类型>:
```java
public interface UserMapper extends BaseMapper<User> {

}
```

### 在Spring Boot 启动类中添加`@MapperScan`注解，扫描Mapper所存放文件夹:
```java
@SpringBootApplication
@MapperScan("com.zgh.springboot.mapper")
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(QuickStartApplication.class, args);
    }

}
```

### 开始使用(这里是通过测试类开启使用):
```java
@SpringBootTest
public class SampleTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void testSelect() {
        System.out.println(("----- selectAll method test ------"));
        List<User> userList = userMapper.selectList(null);
        Assert.assertEquals(5, userList.size());
        userList.forEach(System.out::println);
    }
    
    @Test
    public void selectBatchUserByIds(){
        //多个ID查询
        List<User> users = userMapper.selectBatchIds(Arrays.asList(1,2,3));
        users.forEach(System.out::println);
    }
}
```