# SpringBoot整合MyBatis

### 以配置文件的方式:
- #### 首先需要在Maven中导入MyBatis的以来坐标
```xml
	<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.2</version>
        </dependency>
```
	
- #### 编写封装类(这里用了lombok简化了开发):
```java
import lombok.Data;
import java.io.Serializable;
@Data
public class Product implements Serializable {
    private Integer id;
    private String name;
    private String type;
    private Float price;
    private Float weight;
}
```

- #### 编写Mapper接口类:
```java
// 声明该类胃Mapper类,交给SpringBoot容器接管
@Mapper
public interface ProductMapper {
    Product getProductByID(int id);
}
```

- #### 在resources文件夹下面创建mybatis/mybatis-config.xml文件(由于SprongBoot在底层帮我们写好了配置文件,所以我们一般不创建),与mybatis/mapper/ProductMapper.xml文件
```xml
<!-- MyBatis核心配置文件 -->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
</configuration>
```
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.zgh.springboot.mapper.ProductMapper">
    <select id="getProductByID" resultType="com.zgh.springboot.bean.Product">
        SELECT * FROM t_product WHERE id=#{id}
    </select>
</mapper>
```

- #### 在SpringBoot核心配置文件中配置MyBatis信息:
```properties
#设置连接属性
spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#设置映射文件
#mybatis.config-location=classpath:mybatis/mybatis-config.xml
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
#开启驼峰命名规则(当在核心配置文件中编写时,就不能编写自己的mybatis核心配置文件,会冲突)
#我们也推荐不写自己的核心配置文件.xml文件,直接在SpringBoot核心配置文件中配置
mybatis.configuration.map-underscore-to-camel-case=true
```

- #### 使用:
```java
@Controller
public class MyController {

    ProductMapper productMapper;
    @Autowired
    public void setProductMapper(ProductMapper productMapper) {
        this.productMapper = productMapper;
    }

    @GetMapping(path = "/product")
    public @ResponseBody Product getProductByID(Integer id){
        return productMapper.getProductByID(id);
    }
}
```

### 使用注解的方式(推荐):
- #### 引入坐标(与上面相同).
- #### 编写封装类(与上面相同).
- #### 编写Mapper接口类(在其中注入SQL语句):
```java
@Mapper
public interface ProductMapper {
    @Select("SELECT * FROM t_product WHERE id=#{id}")
    Product getProductByID(int id);
    //插入新数据,并且返回插入后的主键id
    @Insert("INSERT INTO t_product(name,type,price,weight) VALUES(#{name},#{type},#{price},#{weight})")
    @Options(useGeneratedKeys = true,keyProperty = "id")
    void insertProduct(Product product);
}
```

- #### 在SpringBoot核心配置文件中配置MyBatis信息(与上面类似):
```properties
#设置连接属性
spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#设置映射文件(可以使用注解与配置混合的模式,这里我就不用了)
#mybatis.config-location=classpath:mybatis/mybatis-config.xml
#mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
#开启驼峰命名规则(当在核心配置文件中编写时,就不能编写自己的mybatis核心配置文件,会冲突)
#我们也推荐不写自己的核心配置文件.xml文件,直接在SpringBoot核心配置文件中配置
mybatis.configuration.map-underscore-to-camel-case=true
```

- #### 使用:
```java
@Controller
public class MyController {

    ProductMapper productMapper;
    @Autowired
    public void setProductMapper(ProductMapper productMapper) {
        this.productMapper = productMapper;
    }

    @GetMapping(path = "/product")
    public @ResponseBody Product getProductByID(Integer id){
        return productMapper.getProductByID(id);
    }

    @PostMapping(path = "/product")
    public @ResponseBody Product insertProduct(Product product){
        productMapper.insertProduct(product);
        return product;
    }
}
```