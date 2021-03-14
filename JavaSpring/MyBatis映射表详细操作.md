OGNL表达式:
	假设User类中有getId()方法,则取到id可以用#{id}
	假设QueryVo类中有getUser()方法,则取到User属性Id则可以用#{user.id}
----------------------------------------------------------------------------
resultMap标签(设置查询结果和实体类的属性名的对应关系,写在映射表中mapper标签中)：
	id属性:resultMap的唯一标识符,让别人调用的时候用到.
	type属性:查询结果之后封装成的类.
	
	例:
	NewUser类中属性:userId,userName.
	查询结果返回的列名:id,name.
	--------------------------------------
	<resultMap id="newUserMap" type="com.zgh.entity.NewUser">
        <!--主键字段的对应-->
        <id property="userId" column="id"></id>
        <!--非主键字段的对应-->
        <result property="userName" column="name"></result>
    </resultMap>
	--------------------------------------
	
	别人调用时:
	--------------------------------------
	<!--设置resultMap属性设置实体类与查询列名对应关系-->
	<select id="findAllFromNewUser" resultMap="newUserMap">
        SELECT * FROM user_information
    </select>
	--------------------------------------
----------------------------------------------------------------------------

	