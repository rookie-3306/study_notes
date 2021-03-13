MyBatis配置文件:
	一般文件放在目录文件下的resources文件下面,以.xml的文件形式保存.
	下面是文件的一般形式:
---------------------------------------------------------------------------------------
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
---------------------------------------------------------------------------------------



MyBatis映射文件:
	一般也是保存在resouces文件下面(最好与对应的Dao Java文件路径相似例如都是处于com.zgh.dao包下).
	下面是文件的一般形式:
---------------------------------------------------------------------------------------
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--这里设置的全限定类名是要实现的Dao的全限定类名-->
<mapper namespace="com.zgh.dao.IUserDao">
</mapper>
---------------------------------------------------------------------------------------



MyBatis的CUDR操作:
	配置的方法(一下标签均写在MyBatis映射文件的mapper标签中):
	
	select:
	-------------------------------------------
	<!--id设置的是对应要实现的Dao类中的方法名,resultType为返回值类型的全限定类名-->
	<select id="findAll" resultType="com.zgh.entity.User">
        SELECT * FROM user_information
    </select>
	-------------------------------------------

	insert:
	-------------------------------------------
	<!--parameterType属性表示是对应要实现的Dao类中的方法的参数.-->
	<!--#{value}中的value表示该参数的属性,当然该参数要符合javaBean的形式-->
	<insert id="saveUser" parameterType="com.zgh.entity.User">
        INSERT INTO user_information(name,password) VALUES(#{name},#{password})
    </insert>
	-------------------------------------------

	update:
	-------------------------------------------
	<update id="updateUser" parameterType="com.zgh.entity.User">
        UPDATE user_information SET name=#{name},password=#{password} WHERE id=#{id}
    </update>
	-------------------------------------------

	delete:
	-------------------------------------------
	<delete id="deleteUserById" parameterType="java.lang.Integer">
        DELETE FROM user_information WHERE id=#{id}
    </delete>
	-------------------------------------------	
	
	注释的方法(以下的方法均写在需要时功能的Dao类中的方法上):
	
	select:
	-------------------------------------------
	//其中value填写的是查询语句
	@Select("SELECT * FROM user_information")
	-------------------------------------------
	
	insert:
	-------------------------------------------
	//其中#{value}的用法与上面基于配置的用法一样
	@Insert("INSERT INTO user_information(name,password) VALUES(#{name},#{password})")
	-------------------------------------------
	
	update:
	-------------------------------------------
	@Update("UPDATE user_information SET name=#{name},password=#{password} WHERE id=#{id}")
	-------------------------------------------
	
	delete:
	-------------------------------------------
	@Delete("DELETE FROM user_information WHERE id=#{id}")
	-------------------------------------------