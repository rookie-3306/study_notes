# MyBatis-Plus条件构造器.
各种条件构造器条件请查看官网:https://mp.baomidou.com/guide/wrapper.html#abstractwrapper

### 使用:
条件构造器就是一个一个加条件,很简单.
```java
@SpringBootTest
class MybatisPlusApplicationTests {
    private ProductMapper productMapper;
    @Autowired
    public void setProductMapper(ProductMapper productMapper) {
        this.productMapper = productMapper;
    }
    @Test
    public void testSelectQuery(){
        //创建条件构造器
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        //设置属性age值大于18
        wrapper.gt("age",18);
        //设置属性name like %J%
        wrapper.like("name","%J%");
        //加入条件构造器来查询
        List<User> users = userMapper.selectList(wrapper);
        users.forEach(System.out::println);
    }
}
```