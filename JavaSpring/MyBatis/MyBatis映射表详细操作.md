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


动态拼接sql语句:
----------------------------------------------------------------------------
where与if标签:
	--------------------------------------
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
	--------------------------------------
----------------------------------------------------------------------------
	
	
	
foreach标签:

	首先创建中间类来接收List:
	--------------------------------------
	public class NewQueryVo {
		List<Integer> ids;
		public List<Integer> getIds() {
			return ids;
		}
		public void setIds(List<Integer> ids) {
			this.ids = ids;
		}
	}
	--------------------------------------
	
	xml注册实现方法
	--------------------------------------
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
	--------------------------------------
----------------------------------------------------------------------------