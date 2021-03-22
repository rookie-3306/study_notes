如果你的DispatcherServlet拦截 *.do这样的URL，就不存在访问不到静态资源的问题。如果你的DispatcherServlet拦截“/”，
	拦截了所有的请求，同时对*.js,*.jpg的访问也就被拦截了。
问题原因：罪魁祸首是web.xml下对spring的DispatcherServlet请求url映射的配置，原配置如下：
	-------------------------------------------
	<servlet>
		<servlet-name>spring</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	 </servlet>
	 <servlet-mapping>
			<servlet-name>spring</servlet-name>
			<url-pattern>/</url-pattern>
	</span> </servlet-mapping>
	-------------------------------------------
在spring3.0.4以后版本提供了mvc:resources
	<mvc:resources> 的使用方法(写在spring配置文件中)：
	-------------------------------------------
	<!-- 前端控制器,不拦截那些静态资源 -->
    <mvc:resources mapping="/js/**" location="/js/"/> <!-- js文件 -->
    <mvc:resources mapping="/images/**" location="/images/" /> <!-- 图片文件 -->
    <mvc:resources mapping="/css/**" location="/css/"/> <!-- css文件 -->
	-------------------------------------------