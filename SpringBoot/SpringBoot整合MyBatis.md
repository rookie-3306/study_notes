#首先需要在Maven中导入MyBatis的以来坐标
	---------------------------------
	<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.2</version>
        </dependency>
	---------------------------------
	
#然后根据数据库生成实体类,Dao接口与对应的映射文件，与各种Service与Conller,
	---------------------------------
	省略,具体可以看看上面的myBatis的逆向工程
	---------------------------------
	
#在Dao接口类上声明@Mapper注释
	---------------------------------
	@Repository
	public interface StudentMapper {
		int deleteByPrimaryKey(Integer id);

		int insert(Student record);

		int insertSelective(Student record);

		Student selectByPrimaryKey(Integer id);

		int updateByPrimaryKeySelective(Student record);

		int updateByPrimaryKey(Student record);
	}
	---------------------------------
	
#之后把生成的映射文件放在resource文件夹下的mapper文件(自己可以创建)
#然后在springboot的核心配置文件中声明数据库的相关属性:
	---------------------------------
	#配置连接数据库的配置
	spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
	spring.datasource.url=jdbc:mysql://localhost:3306/reverse_my_batis?characterEncoding=utf8&serverTimezone=Asia/Shanghai
	spring.datasource.username=root
	spring.datasource.password=root

	#设置MyBatis映射文件的路径
	mybatis.mapper-locations=classpath:mapper/*.xml
	---------------------------------
	
#在springBoot类上写上注释,扫描dao接口所在的包
	---------------------------------
	//开启扫描Mapper接口
	@MapperScan("com.zgh.springboot.mapper")
	@SpringBootApplication
	public class MybatisreverseApplication {
		public static void main(String[] args) {
			SpringApplication.run(MybatisreverseApplication.class, args);
		}
	}
	---------------------------------
------------------------------------------------------------------

#还有另外一种用法,这种用法会把Dao接口与对应的映射文件放在同一个目录下面.
#不用吧映射文件移动到resource中去,而且也不需要在springboot核心配置文件中写MyBatis映射文件的路径.
#但是连接数据库的属性还是要设置的.

#首先导入坐标与生成实体类之类的都是一样的.
#之后在Dao接口类上声明@Mapper注释
#在Mevan文件中的build标签之中声明资源文件的路径(就是我们的映射文件),这样就能让我们的映射文件被扫描
	---------------------------------
	<resources>
		<resource>
			<directory>src/main/java</directory>
			<includes>
				<include>**/*.xml</include>
			</includes>
		</resource>
    </resources>
	---------------------------------
#其他的就都一样