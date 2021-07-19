# MyBatis-Plus自动注入与乐观锁.
自动注入参考官网网址:https://mp.baomidou.com/guide/auto-fill-metainfo.html#%E8%87%AA%E5%8A%A8%E5%A1%AB%E5%85%85%E5%8A%9F%E8%83%BD

### 自动注入属性:
当我们用MyBatis-Plus用实体类来更新数据库数据的时候,MyBatis-Plus会根据实体类的属性是否为NULL来自动更新.这时候我们就能设置些自动注入的属性.
- #### 创建实体类:
```java
@Data
public class User {
    @TableId(type = IdType.ASSIGN_ID)
    private Long id;
    private String name;
    private Integer age;
    private String email;
    //下面这两个属性就是使用自动注入来插入数值的
    //TableField来声明自动注入的类型
    //这里FieldFill.INSERT就是表示插入时自动注入
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;
    //FieldFill.INSERT_UPDATE表示跟新与插入的时候自动注入
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
}
```
- #### 编写自定义处理器类来实现自动注入的接口:
```java
//记得将类交给SpringBoot管理
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
        //当使用MyBatis-Plus执行插入的时候，这个方法执行
        //metaObject为源数据

        //根据属性名设置属性值
        this.setFieldValByName("createTime",new Date(),metaObject);
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        //当使用MyBatis-Plus执行更新的时候，这个方法执行

        this.setFieldValByName("updateTime",new Date(),metaObject);
    }
}
```
- #### 使用:
```java
    @Test
    public void insertUser(){
        User user = new User();
        user.setAge(20);
        user.setEmail("1376705547@qq.com");
        user.setName("zgh");
        int count = userMapper.insert(user);
        System.out.println(count);
    }
```

### 用MyBatis-Plus实现乐观锁:
当程序中可能出现并发的情况时，就需要保证在并发情况下数据的准确性，以此确保当前用户和其他用户一起操作时，所得到的结果和他单独操作时的结果是一样的。这就叫做并发控制。并发控制的目的是保证一个用户的工作不会对另一个用户的工作产生不合理的影响。
**没有做好并发控制，就可能导致脏读、幻读和不可重复读等问题。**
乐观锁的实现:
**版本号控制**：一般是在数据表中加上一个数据版本号 version 字段，表示数据被修改的次数。当数据被修改时，version 值会 +1。当线程 A 要更新数据时，在读取数据的同时也会读取 version 值，在提交更新时，若刚才读取到的 version 值与当前数据库中的 version 值相等时才更新，否则重试更新操作，直到更新成功。

- #### 首先在实体类与数据库属性中都添加一个属性version(版本号):
```java
@Data
public class User {
    @TableId(type = IdType.ASSIGN_ID)
    private Long id;
    private String name;
    private Integer age;
    private String email;
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
    //在版本号的属性上加入@Version注解来实现乐观锁
    //当然记得在插入的初始化版本号
    @Version
    @TableField(fill = FieldFill.INSERT)
    private Integer version;
}
```
```java
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
        //当使用MyBatis-Plus执行插入的时候，这个方法执行
        //metaObject为源数据

        //根据属性名设置属性值
        this.setFieldValByName("createTime",new Date(),metaObject);
        this.setFieldValByName("updateTime",new Date(),metaObject);
        this.setFieldValByName("version",1,metaObject);
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        this.setFieldValByName("updateTime",new Date(),metaObject);
    }
}
```
```sql
--数据库自己想象
```

- #### 编写SpringBoot配置类加入MyBatis-Plus实现乐观锁的插件:
```java
@MapperScan("com.zgh.springboot.mapper")
@Configuration
public class MyConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        //乐观锁插件
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }

}
```

- #### 测试:
```java
@SpringBootTest
class MybatisPlusApplicationTests {
    private ProductMapper productMapper;
    @Autowired
    public void setProductMapper(ProductMapper productMapper) {
        this.productMapper = productMapper;
    }

    @Test
    public void insertUser(){
        User user = new User();
        user.setAge(20);
        user.setEmail("1376705547@qq.com");
        user.setName("zgh");
        int count = userMapper.insert(user);
        System.out.println(count);
    }

    @Test
    public void updateUser(){
        //记得先查再改才能自动修改版本号
        User user = userMapper.selectById(1402265665493798914L);
        user.setName("zzxx");
        userMapper.updateById(user);
    }

}

```