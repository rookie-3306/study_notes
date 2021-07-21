### MyBatis配置文件:
	一般文件放在目录文件下的resources文件下面,以.xml的文件形式保存.
	下面是文件的一般形式:
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 配置环境 -->
    <environments default="mysql">
        <!--配置mysql环境-->
        <environment id="mysql">
            <!--配置事务类型-->
            <transactionManager type="JDBC"/>
            <!--配置数据源(连接池)-->
            <dataSource type="POOLED">
                <!-- 四个连接数据库配置 -->
                <!-- 连接驱动(这里的连接驱动使用的时mysql.jar包里面的连接驱动) -->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <!-- 连接url(注意时区问题) -->
                <property name="url" value="jdbc:mysql://localhost:3306/my_batis?characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai"/>
                <!-- 用户名 -->
                <property name="username" value="root"/>
                <!-- 密码 -->
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <!--指定映射配置文件的位置，映射文件是指每个dao的独立配置文件-->
    <mappers>
		<!--设定映射文件，这里表示的时resource文件夹下面的com/zgh/dao/IUserDao.xml文件-->
        <mapper resource="com/zgh/dao/IUserDao.xml"/>
    </mappers>
</configuration>
```
----------------------------------
### 在configuration里面也能写上配置参数(url属性的使用方法这里不细讲,可以自行去查资料):
```xml
<properties resource="jdbcConfig.properties"></properties>
```
- #### jdbcConfig.properties配置文件(一般放在resources文件下面):
```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=dbc:mysql://localhost:3306/book?characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai
jdbc.username=root
jdbc.password=root
```
- #### 配置完之后的调用的方法:
```xml
<property name="driver" value="${jdbc.driver}}"/>
<property name="url" value="${jdbc.url}"/>
<property name="username" value="${jdbc.username}"/>
<property name="password" value="${jdbc.password}"/>
```
----------------------------------

### MyBatis映射文件:
	一般也是保存在resouces文件下面(最好与对应的Dao Java文件路径相似例如都是处于com.zgh.dao包下).
	下面是文件的一般形式:
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--这里设置的全限定类名是要实现的Dao的全限定类名-->
<mapper namespace="com.zgh.dao.IUserDao">
</mapper>
```
- #### MyBatis的CUDR操作:
	**配置的方法(一下标签均写在MyBatis映射文件的mapper标签中):**
- select:
```xml
<!--id设置的是对应要实现的Dao类中的方法名,resultType为返回值类型的全限定类名-->
<select id="findAll" resultType="com.zgh.entity.User">
			SELECT * FROM user_information
	</select>
```

- insert:
```xml
<!--parameterType属性表示是对应要实现的Dao类中的方法的参数.-->
<!--#{value}中的value表示该参数的属性,当然该参数要符合javaBean的形式-->
<insert id="saveUser" parameterType="com.zgh.entity.User">
			INSERT INTO user_information(name,password) VALUES(#{name},#{password})
</insert>
```

- update:
```xml
<update id="updateUser" parameterType="com.zgh.entity.User">
			UPDATE user_information SET name=#{name},password=#{password} WHERE id=#{id}
</update>
```

- delete:
```xml
<delete id="deleteUserById" parameterType="java.lang.Integer">
		DELETE FROM user_information WHERE id=#{id}
</delete>
```
	
**注解配置的方法(以下的方法均写在需要时功能的Dao类中的方法上):**
- select:
```java
//其中value填写的是查询语句
@Select("SELECT * FROM user_information")
```
	
- insert:
```java
//其中#{value}的用法与上面基于配置的用法一样
@Insert("INSERT INTO user_information(name,password) VALUES(#{name},#{password})")
```
	
- update:
```java
@Update("UPDATE user_information SET name=#{name},password=#{password} WHERE id=#{id}")
```
	
- delete:
```java
@Delete("DELETE FROM user_information WHERE id=#{id}")
```
----------------------------------


### MyBatis配置完之后的方法调用:
- #### 第一种方法
```java
public class MyBatisTest {
    InputStream in = null;
    SqlSessionFactory factory = null;
    SqlSession session = null;
    IUserDao userDao = null;
    public static void main(String[] args) throws IOException {
	//1.读取配置文件
        in = Resources.getResourceAsStream("mybatis-config.xml");
        //2.创建SqlSessionFactory工厂
        //SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        factory = new SqlSessionFactoryBuilder().build(in);
        //3.使用工厂生产SqlSession对象
        session = factory.openSession();
        //4.使用SqlSession创建Dao接口的代理对象
        userDao = session.getMapper(IUserDao.class);
		//之后就能使用userDao里面配置完的方法(像增,删,改之类的方法需要session.commit()下)
		User user = new User(-1,"ccc123","123");
        userDao.saveUser(user);
        session.commit();
		//清除内存
		session.close();
        in.close();
    }
}
```
- #### 另外一种实现方法(用的是Session):
IUserDao接口:
```java
public interface IUserDao {
	List<User> findAll();
}
```
IUserDaoImp类实现IuserDao接口:
```java
public class IUserDaoImp implements IUserDao {
	SqlSessionFactory factory = null;
	public IUserDaoImp(SqlSessionFactory factory){
		this.factory =factory;
	}
	@Override
	public List<User> findAll() {
		//根据factory获取sqlSession对象
		SqlSession sqlSession = factory.openSession();
		//调用SqlSession中的方法，实现查询列表
		List<User> users = sqlSession.selectList("com.zgh.dao.IUserDao.findAll");
		sqlSession.close();
		return users;
	}
}
```
然后调用方法:
```java
public class MyBatisTest {
	InputStream in = null;
	SqlSessionFactory factory = null;
	SqlSession session = null;
	IUserDao userDao = null;
	public static void main(String[] args) throws IOException {
		//1.读取配置文件
		in = Resources.getResourceAsStream("mybatis-config.xml");
		//2.创建SqlSessionFactory工厂
		//SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
		factory = new SqlSessionFactoryBuilder().build(in);
		//实例化实现类
		IUserDao userDao = new IUserDaoImp(factory);
		List<User> users = userDao.findAll();
		for(User user:users){
			System.out.println(user);
		}
	}
}
```