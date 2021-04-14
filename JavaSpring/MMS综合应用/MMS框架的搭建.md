首先用Maven导入相关的包:
	--------------------------------------
	<dependencies>
		<!-- 测试 -->
		<dependency>
		  <groupId>junit</groupId>
		  <artifactId>junit</artifactId>
		  <version>4.11</version>
		</dependency>

		<!-- springMVC -->
		<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
		<dependency>
		  <groupId>javax.servlet</groupId>
		  <artifactId>javax.servlet-api</artifactId>
		  <version>4.0.1</version>
		  <scope>provided</scope>
		</dependency>

		<!-- https://mvnrepository.com/artifact/javax.servlet.jsp/jsp-api -->
		<dependency>
		  <groupId>javax.servlet.jsp</groupId>
		  <artifactId>jsp-api</artifactId>
		  <version>2.2</version>
		  <scope>provided</scope>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.springframework/spring-web -->
		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-web</artifactId>
		  <version>5.3.4</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-webmvc</artifactId>
		  <version>5.3.4</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
		<dependency>
		  <groupId>com.fasterxml.jackson.core</groupId>
		  <artifactId>jackson-databind</artifactId>
		  <version>2.12.1</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/com.google.code.gson/gson -->
		<dependency>
		  <groupId>com.google.code.gson</groupId>
		  <artifactId>gson</artifactId>
		  <version>2.8.5</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
		<dependency>
		  <groupId>commons-fileupload</groupId>
		  <artifactId>commons-fileupload</artifactId>
		  <version>1.3.3</version>
		</dependency>

		<!-- MyBatis -->
		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
		<dependency>
		  <groupId>org.mybatis</groupId>
		  <artifactId>mybatis</artifactId>
		  <version>3.4.6</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
		<dependency>
		  <groupId>mysql</groupId>
		  <artifactId>mysql-connector-java</artifactId>
		  <version>8.0.16</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
		<dependency>
		  <groupId>com.mchange</groupId>
		  <artifactId>c3p0</artifactId>
		  <version>0.9.5.2</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
		<dependency>
		  <groupId>org.mybatis</groupId>
		  <artifactId>mybatis-spring</artifactId>
		  <version>2.0.6</version>
		</dependency>

		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-tx</artifactId>
		  <version>4.3.18.RELEASE</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-jdbc</artifactId>
		  <version>5.3.4</version>
		</dependency>




		<!-- SpringAOP,IOC -->
		<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
		<dependency>
		  <groupId>org.aspectj</groupId>
		  <artifactId>aspectjweaver</artifactId>
		  <version>1.9.6</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-core</artifactId>
		  <version>5.3.4</version>
		</dependency>


		<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
		<dependency>
		  <groupId>org.springframework</groupId>
		  <artifactId>spring-context</artifactId>
		  <version>5.3.4</version>
		</dependency>

	  </dependencies>
	--------------------------------------
	
创建spring_mvc配置文件：
	--------------------------------------
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		   xmlns:context="http://www.springframework.org/schema/context"
		   xmlns:mvc="http://www.springframework.org/schema/mvc"
		   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/cache http://www.springframework.org/schema/cache/spring-cache.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

		<!-- 开启注解扫描 -->
		<context:component-scan base-package="com.zgh.controller"></context:component-scan>

		<!-- 配置视图解析器 -->
		<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			<property name="prefix" value="/WEB-INF/pages/"></property>
			<property name="suffix" value=".jsp"></property>
		</bean>

		<mvc:resources mapping="/js/**" location="/js/"/> <!-- js文件 -->
		<mvc:resources mapping="/images/**" location="/images/" /> <!-- 图片文件 -->
		<mvc:resources mapping="/css/**" location="/css/"/> <!-- css文件 -->

		<!-- 开启spring-mvc框架注解支持 -->
		<mvc:annotation-driven/>
	</beans>
	--------------------------------------
	
创建springIOC与AOP的配置文件,其中还包括交给spring托管的myBatin:
	--------------------------------------
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		   xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
		   xmlns:aop="http://www.springframework.org/schema/aop"
		   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/cache http://www.springframework.org/schema/cache/spring-cache.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

		<!-- 开启注解扫描 -->
		<context:component-scan base-package="com.zgh">
			<!-- 配置那些注解不扫描,我们希望控制器类不被spring处理 -->
			<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
		</context:component-scan>

		<!-- 交给spring创建MyBatis -->
		<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
			<property name="driverClass" value="com.mysql.cj.jdbc.Driver"></property>
			<property name="jdbcUrl" value="jdbc:mysql://localhost:3306/flower_shop?characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai"/>
			<property name="user" value="root"/>
			<property name="password" value="root"/>
		</bean>
		<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
			<property name="dataSource" ref="dataSource"></property>
		</bean>
		<bean id="mapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
			<property name="basePackage" value="com.zgh.dao"></property>
		</bean>

		<!-- 配置事务管理器 -->
		<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
			<property name="dataSource" ref="dataSource"></property>
		</bean>

		<!-- 配置事务通知 -->
		<tx:advice id="txAdvice" transaction-manager="transactionManager">
			<tx:attributes>
				<tx:method name="find*" read-only="true"/>
				<!-- 配置隔离级别 -->
				<tx:method name="*" isolation="DEFAULT"/>
			</tx:attributes>
		</tx:advice>

		<!-- 配置事务增强 -->
		<aop:config>
			<aop:advisor advice-ref="txAdvice" pointcut="execution(* com.zgh.service.imp.UserServiceImp.*(..))"></aop:advisor>
		</aop:config>
	</beans>
	--------------------------------------
	
配置web.xml文件读取上面两个配置文件:
	--------------------------------------
	<web-app>
	  <display-name>Archetype Created Web Application</display-name>
	  <!-- 在javaWeb创建ServletContext的时候读取标签内的类,配合listener使用加载spring配置文件 -->
	  <context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	  </context-param>

	  <!-- 配置中文乱码过滤器 -->
	  <filter>
		<filter-name>characterEncodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	  </filter>
	  <filter-mapping>
		<filter-name>characterEncodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	  </filter-mapping>

	  <!-- 设置监听器,来监听Context的创建 -->
	  <listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	  </listener>

	  <!-- 初始化读取spring_mvc的配置文件 -->
	  <servlet>
		<servlet-name>dispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
		  <param-name>contextConfigLocation</param-name>
		  <param-value>classpath:spring_mvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	  </servlet>

	  <servlet-mapping>
		<servlet-name>dispatcherServlet</servlet-name>
		<url-pattern>/</url-pattern>
	  </servlet-mapping>

	</web-app>
	--------------------------------------