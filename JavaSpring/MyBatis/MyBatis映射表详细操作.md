### OGNL表达式:
	假设User类中有getId()方法,则取到id可以用#{id}
	假设QueryVo类中有getUser()方法,则取到User属性Id则可以用#{user.id}

--------------------------------------

### resultMap标签(设置查询结果和实体类的属性名的对应关系,写在映射表中mapper标签中)：
	id属性:resultMap的唯一标识符,让别人调用的时候用到.
	type属性:查询结果之后封装成的类.
	
	例:

- ####	NewUser类中属性:userId,userName.查询结果返回的列名:id,name.
```xml
<resultMap id="newUserMap" type="com.zgh.entity.NewUser">
		<!--主键字段的对应-->
		<id property="userId" column="id"></id>
		<!--非主键字段的对应-->
		<result property="userName" column="name"></result>
</resultMap>
```
	
- ####	别人调用时:
```xml
<!--设置resultMap属性设置实体类与查询列名对应关系-->
<select id="findAllFromNewUser" resultMap="newUserMap">
		SELECT * FROM user_information
</select>
```

--------------------------------------


### 动态拼接sql语句:

- #### where与if标签:
```xml
<select id="findUserByCondition" parameterType="com.zgh.entity.User" resultType="com.zgh.entity.User">
		SELECT * FROM user_information
<!--连接一个where关键字-->
		<where>
	<!--条件成立就把if里面的语句连接在sql语句后面-->
				<if test="name != null">
						AND name=#{name}
				</if>
				<if test="id != 0">
						AND id=#{id}
				</if>
		</where>
</select>
```	
	
- #### foreach标签:
首先创建中间类来接收List:
```java
public class NewQueryVo {
	List<Integer> ids;
	public List<Integer> getIds() {
		return ids;
	}
	public void setIds(List<Integer> ids) {
		this.ids = ids;
	}
}
```
xml注册实现方法
```xml
<select id="findUserByListId" parameterType="com.zgh.entity.NewQueryVo" resultType="user">
			SELECT * FROM user_information
			<where>
					<if test="ids != null and ids.size()>0">
							<!--collection属性表示需要遍历的类中的属性-->
							<!--open表示开始的sql语句-->
							<!--item表示遍历的实体,其中名字与下方#{value}需要一样-->
							<!--close表示结束的sql语句-->
							<!--separator表示其中语句的分隔符-->
							<foreach collection="ids" open="and id in(" item="id" close=")" separator=",">
									#{id}
							</foreach>
					</if>
			</where>
	</select>
```

--------------------------------------

- #### 多表查询:
一对一时的多表查询:
在Account类中添加个User属性user,现在Account类中有属性:int id,int uid,float money,User user.
声明resultMap:
```xml
<resultMap id="accountAndUser" type="com.zgh.entity.Account">
		<id property="id" column="id"></id>
		<result property="uid" column="uid"></result>
		<result property="money" column="money"></result>
<!--iproperty表示需要注入的属性的名字(符合javaBean),javaType表示该属性的类型-->
		<association property="user" column="uid" javaType="com.zgh.entity.User">
				<id property="id" column="uid"></id>
				<result property="name" column="name"></result>
				<result property="password" column="password"></result>
		</association>
</resultMap>
```	
声明SELECT语句:
```xml
<select id="findAllAccountToUser" resultMap="accountAndUser">
		SELECT account.*,user_information.`name`,user_information.`password` FROM account INNER JOIN user_information
		WHERE user_information.id = account.uid
</select>
```
	
	
	
一对多时的多表查询:
在User类中添加个List<Account>属性accounts,现在User类中属性有:int id,String name,List<Account> accounts.
声明resultMap:
```xml
<resultMap id="userAndAccount" type="com.zgh.entity.User">
		<id property="id" column="id"></id>
		<result property="name" column="name"></result>
		<result property="password" column="password"></result>
<!--iproperty表示需要注入的属性的名字(符合javaBean),ofType表示该属性的类型-->
		<collection property="accounts" column="aid" ofType="com.zgh.entity.Account">
				<id property="id" column="aid"></id>
				<result property="uid" column="id"></result>
				<result property="money" column="money"></result>
		</collection>
</resultMap>
```
声明SELECT语句:
```xml
<select id="findUserAndAccount" resultMap="userAndAccount">
		SELECT user_information.*,account.id as aid,account.money FROM user_information INNER JOIN account
		WHERE user_information.id=account.uid
</select>
```

多对多查询(类似于一对多查询):
新声明两张表:一张表为:role,列有ID,ROLE_NAME,ROLE_DESC.另外一张表为user_role,列有UID,RID.
构建相对应的实体类完成.
声明resultMap:
```xml
<resultMap id="roleAndUsers" type="com.zgh.entity.Role">
			<id property="id" column="ID"></id>
			<result property="name" column="ROLE_NAME"></result>
			<result property="desc" column="ROLE_DESC"></result>
			<collection property="users" column="UID" ofType="com.zgh.entity.User">
					<id property="id" column="UID"></id>
					<result property="name" column="name"></result>
					<result property="password" column="password"></result>
			</collection>
	</resultMap>
```
声明SELECT语句:
```xml
<select id="findRoleAndUser" resultMap="roleAndUsers">
	SELECT * FROM role LEFT OUTER JOIN user_role
	ON role.ID = user_role.RID
	LEFT OUTER JOIN user_information
	ON user_information.id = user_role.UID
	</select>
```
	