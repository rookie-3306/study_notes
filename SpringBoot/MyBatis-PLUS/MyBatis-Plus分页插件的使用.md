# MyBatis-Plus分页插件的使用.
默认MyBatis-Plus环境安装成功.



### 在SpringBoot配置类加入分页插件:
```java
@MapperScan("com.zgh.springboot.mapper")
@Configuration
public class MyConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        //添加乐观锁插件
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        //添加分页插件
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }

}
```

### 使用:
```java
@SpringBootTest
class MybatisPlusApplicationTests {

    private UserMapper userMapper;
    @Autowired
    public void setUserMapper(UserMapper userMapper) {
        this.userMapper = userMapper;
    }


    @Test
    public void testPage(){
        //参数列表:current当前页,size每页显示记录数
        Page<User> page = new Page<>(1,3);
        //调用MyBatis-Plus分页查询方法
        //封装进page对象中
        userMapper.selectPage(page,null);

        //通过page对象获取分页数据信息
        System.out.println(page.getCurrent());//获取当前页
        page.getRecords().forEach(System.out::println);//每页数据的List集合
        System.out.println(page.getSize());//当前页的记录数
        System.out.println(page.getTotal());//总记录数
        System.out.println(page.getPages());//总页数
        System.out.println(page.hasNext());//是否有下一页
        System.out.println(page.hasPrevious());//是否有上一页
    }
}

```