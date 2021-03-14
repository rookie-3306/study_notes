properties标签(写在MyBatis配置文件中configuration标签里面):
	-------------------------------------------
	<!--resource属性，指定配置文件的位置,这里配置文件写在resources文件夹下面-->
	<!--url属性，不详细说明,具体可以去查资料-->
	<properties resource="jdbcConfig.properties"></properties>
	-------------------------------------------
	
	jdbcConfig.properties配置文件(一般放在resources文件下面):
	-------------------------------------------
	jdbc.driver=com.mysql.cj.jdbc.Driver
	jdbc.url=dbc:mysql://localhost:3306/book?characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai
	jdbc.username=root
	jdbc.password=root
	-------------------------------------------
	
	配置完之后的使用的方法:
	-------------------------------------------
	<!-- value="${jdbc.driver}效果等于value="com.mysql.cj.jdbc.Driver" -->
	<property name="driver" value="${jdbc.driver}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
	-------------------------------------------
---------------------------------------------------------------------------------------



typeAliases标签(写在MyBatis配置文件中configuration标签里面):
	用来指定全限定类名的别名,增加开发速度:
	-------------------------------------------
	<typeAliases>
        <!-- typeAlias标签配置别名,type属性定义的是全限定类名.alias指定的是别名,指定别名之后就不再区分大小写 -->
        <typeAlias type="com.zgh.entity.User" alias="user"></typeAlias>
        <!-- package标签配置该包下所有类的别名,并且类名就是别名 -->
        <package name="com.zgh.entity"/>
    </typeAliases>
	-------------------------------------------
	
	配置完之后的使用的方法:
	-------------------------------------------
	<!-- resultType="user"效果等于resultType="com.zgh.entity.user" -->
	<select id="findAll" resultType="user">
        SELECT * FROM user_information
    </select>
	-------------------------------------------
---------------------------------------------------------------------------------------



package标签(写在MyBatis配置文件中mappers标签里面):
	用来指定dao接口所在的包,代替resour或者class标签的作用:
	-------------------------------------------
	<package name="com.zgh.dao"/>
	-------------------------------------------
---------------------------------------------------------------------------------------