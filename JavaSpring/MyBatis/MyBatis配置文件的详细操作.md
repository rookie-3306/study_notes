- #### properties标签(写在MyBatis配置文件中configuration标签里面):
```xml
<!--resource属性，指定配置文件的位置,这里配置文件写在resources文件夹下面-->
<!--url属性，不详细说明,具体可以去查资料-->
<properties resource="jdbcConfig.properties"></properties>
```
	
- #### jdbcConfig.properties配置文件(一般放在resources文件下面):
```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=dbc:mysql://localhost:3306/book?characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai
jdbc.username=root
jdbc.password=root
```
	
- #### 配置完之后的使用的方法:
```xml
<!-- value="${jdbc.driver}效果等于value="com.mysql.cj.jdbc.Driver" -->
<property name="driver" value="${jdbc.driver}"/>
<property name="url" value="${jdbc.url}"/>
<property name="username" value="${jdbc.username}"/>
<property name="password" value="${jdbc.password}"/>
```
----------------------------

- #### typeAliases标签(写在MyBatis配置文件中configuration标签里面):
	用来指定全限定类名的别名,增加开发速度:
```xml
<typeAliases>
		<!-- typeAlias标签配置别名,type属性定义的是全限定类名.alias指定的是别名,指定别名之后就不再区分大小写 -->
		<typeAlias type="com.zgh.entity.User" alias="user"></typeAlias>
		<!-- package标签配置该包下所有类的别名,并且类名就是别名 -->
		<package name="com.zgh.entity"/>
</typeAliases>
```
	
- ####	配置完之后的使用的方法:
```xml
<!-- resultType="user"效果等于resultType="com.zgh.entity.user" -->
<select id="findAll" resultType="user">
		SELECT * FROM user_information
</select>
```
----------------------------

- #### package标签(写在MyBatis配置文件中mappers标签里面):
	用来指定dao接口所在的包,代替resour或者class标签的作用:
```xml
<package name="com.zgh.dao"/>
```
----------------------------

- #### 开启延迟加载(就是在真正用到值的时候再查询，一般多用于一对多):
设置settings标签(在configuration标签中配置):
```xml
<settings>
		<!-- 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 -->
		<setting name="lazyLoadingEnabled" value="true"/>
		<!-- 开启时，任一方法的调用都会加载该对象的所有延迟加载属性。 -->
		<setting name="aggressiveLazyLoading" value="false"/>
</settings>
```
设置当查询时使用调用方法来查询(此例子是一对多查询):
```xml
<resultMap id="DelayedLoading" type="com.zgh.entity.Role">
		<id property="id" column="ID"></id>
		<result property="name" column="ROLE_NAME"></result>
		<result property="desc" column="ROLE_DESC"></result>
		<collection property="users" column="UID" ofType="com.zgh.entity.User" select="com.zgh.dao.IUserDao.findUserById"></collection>
</resultMap>
```
查询:
```xml
<select id="findRoleAndUserDelayedLoading" resultMap="DelayedLoading">
SELECT * FROM role LEFT OUTER JOIN user_role
ON role.ID = user_role.RID
LEFT OUTER JOIN user_information
ON user_information.id = user_role.UID
</select>
```
	