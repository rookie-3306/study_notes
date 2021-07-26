#### 在web.xml文件中可以在创建Servlet容器的时候读取spring的配置文件.
```xml
	<!-- 在javaWeb创建ServletContext的时候读取标签内的类,配合listener使用加载spring配置文件 -->
	  <context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	  </context-param>
	  
	  <!-- 设置监听器,来监听Context的创建 -->
	  <listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	  </listener>
```