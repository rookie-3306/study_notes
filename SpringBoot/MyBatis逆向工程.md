用MyBatis逆向工程自动生成实体类,接口类与映射表.
	首先需要在Maven文件中配置MyBatis逆向工程所需要的文件:
	---------------------------------
	<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

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
    </dependencies>

    <build>
        <plugins>

            <!-- myBatis代码自动生成插件 -->
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.6</version>
                <configuration>
                    <!-- 配置文件的位置 -->
                    <configurationFile>GeneratorMapper.xml</configurationFile>
                    <verbose>true</verbose>
                    <overwrite>true</overwrite>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
	---------------------------------
	
	然后在上方标签配置的文件位置创建配置文件,编写:
	---------------------------------
	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

	<generatorConfiguration>
		<!-- 指定JDBC驱动包所在位置 -->
		<classPathEntry location="E:\mvenPackage\mysql\mysql-connector-java\8.0.23\mysql-connector-java-8.0.23.jar"/>
		<context id="DB2Tables" targetRuntime="MyBatis3">
			<commentGenerator>
				<!-- 是否去除自动生成的注释 -->
				<property name="suppressAllComments" value="true"/>
			</commentGenerator>
			<!-- Mysql数据库连接的信息：驱动类、连接地址、用户名、密码 -->
			<jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
							connectionURL="jdbc:mysql://localhost:3306/flower_shop?characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai"
							userId="root"
							password="root">
			</jdbcConnection>
			<!-- Oracle数据库
				<jdbcConnection driverClass="oracle.jdbc.OracleDriver"
					connectionURL="jdbc:oracle:thin:@127.0.0.1:1521:yycg"
					userId="yycg"
					password="yycg">
				</jdbcConnection>
			-->

			<!-- 默认为false，把JDBC DECIMAL 和NUMERIC类型解析为Integer，为true时
			把JDBC DECIMAL 和NUMERIC类型解析为java.math.BigDecimal -->
			<javaTypeResolver >
				<property name="forceBigDecimals" value="false" />
			</javaTypeResolver>

			<!-- targetProject:生成Model类的位置 -->
			<!-- targetProject:生成的model放在哪个工程下面  -->
			<javaModelGenerator targetPackage="com.zgh.springboot.model" targetProject="src/main/java">
				<!-- enableSubPackages:是否让schema作为包的后缀 -->
				<property name="enableSubPackages" value="false" />
				<!-- 从数据库返回的值被清理前后的空格 -->
				<property name="trimStrings" value="true" />
			</javaModelGenerator>

			<!-- targetProject：mapper映射文件生成的位置 -->
			<!-- targetProject:生成的model放在哪个工程下面  -->
			<sqlMapGenerator targetPackage="com.zgh.springboot.mapper"  targetProject="src/main/java">
				<!-- enableSubPackages:是否让schema作为包的后缀 -->
				<property name="enableSubPackages" value="false" />
			</sqlMapGenerator>

			<!-- targetProject：mapper接口生成的的位置 -->
			<!-- targetProject:生成的model放在哪个工程下面  -->
			<javaClientGenerator type="XMLMAPPER" targetPackage="com.zgh.springboot.mapper"  targetProject="src/main/java">
				<!-- enableSubPackages:是否让schema作为包的后缀 -->
				<property name="enableSubPackages" value="false" />
			</javaClientGenerator>



			<!-- 数据表名以及对应的Java模型名 -->
			<table tableName="user_information" domainObjectName="User"
				   enableCountByExample="false"
				   enableUpdateByExample="false"
				   enableDeleteByExample="false"
				   enableSelectByExample="false"
				   selectByExampleQueryId="false"/>


		</context>
	</generatorConfiguration>
	---------------------------------