# MyBatis-Plus插入时主键生成策略.

### 介绍:
在用MyBatis-Plus的方法插入数据的时候,我们没有填入主键的时候,MyBatis-Plus就会帮我们自动生成主键,**默认生成的算法为雪花算法(雪花每片都不一样不是么),当然我们能设置他的生成策略**.

**雪花算法**:雪花算法生成的最终结果其实就是一个long类型的Java长整型数字，这是一个大前提！算法所有的内容都是针对这个数字进行运算的。Java基础类型相信都很熟悉，有32位的整型int类型，和64位的长整型long类型。我们单说long类型，64位说的是数字转换为二进制形式时候的表现，其中第一位表示的是正负，也就是符号，剩下的63位表示的是字面数字。针对每个公司，随着服务化演进，单个服务越来越多，数据库分的越来越细，有的时候一个业务需要分成好几个库，这时候自增主键或者序列之类的主键id生成方式已经不再满足需求，分布式系统中需要的是一个全局唯一的id生成规则。

###  使用方法:
- #### 使用默认的雪花算法:
```java
@SpringBootTest
class MybatisPlusApplicationTests {

    private UserMapper userMapper;
    @Autowired
    public void setUserMapper(UserMapper userMapper) {
        this.userMapper = userMapper;
    }

    @Test
    public void insertUser(){
        //当我们没有设置ID属性的时候,MyBatis-Plus就会帮我们自动生成ID,自动生成的ID参考雪花算法
        User user = new User();
        user.setAge(20);
        user.setEmail("1376705547@qq.com");
        user.setName("zgh");
        int count = userMapper.insert(user);
        System.out.println(count);
    }
}
```
- #### 自己设置主键生成策略(在实体类的主键上设置):
```java
@Data
public class User {
    //设置主键自动增长
    @TableId(type = IdType.AUTO)
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```
当然`IdType`中有五个枚举:
AUTO:自动增长(当然,数据库创建的时候需要设置主键自动增长,要不然会报没有插入主键默认值)
INPUT:自己输入
NONE:没有生成策略
ASSIGN_UUID:随机生成全局唯一ID(主键为String类型的时候)
ASSIGN_ID:默认生成策略,就是使用雪花算法
