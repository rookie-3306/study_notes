# MyBatis-Plus 实现逻辑删除.

### 物理删除与逻辑删除:
**物理删除**:就是从数据库中真正删除了该数据,从物理层面删除了数据.
**逻辑删除**:就是用一个标志位标记数据,当数据被删除时只是改变了标志位,并没有被真正删除,在物理结构上这条数据还是存在的.

### 用MyBatis-Plus实现逻辑删除:
- #### 在最新版本中MyBatis-Plus已经自带插件了,不需要让我们再从配置类中引入了。
- #### 在数据库中创建标志位属性,这里为deleted(创建标志位的时候记得防止与sql关键字delete冲突!)
- #### 在实体类中的标志位上声明注解`@TableLogic`:
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
    @TableField(fill = FieldFill.INSERT)
    @Version
    private Integer version;
    @TableField(fill = FieldFill.INSERT)
    //声明该属性是逻辑删除的标志位
    @TableLogic
    private Integer deleted;
}
```
- #### 使用:
```java
@SpringBootTest
class MybatisPlusApplicationTests {
    private ProductMapper productMapper;
    @Autowired
    public void setProductMapper(ProductMapper productMapper) {
        this.productMapper = productMapper;
    }
     @Test
    public void  deleteUserById(){
        //当配置完逻辑删除的标志位的时候,这时候删除就会设置逻辑位的值,并不会执行物理删除
        //小提示:设置逻辑删除的标志位属性名字的时候别和SQL关键字delete冲突了.
        int result = userMapper.deleteById(1402467966521196545L);
        System.out.println(result);
    }

}
```