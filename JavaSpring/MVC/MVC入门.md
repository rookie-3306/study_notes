- #### 在Meven中导入包:
```xml
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

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

  </dependencies>
```
-------------------------------------------

- #### 在创建WEB-INF文件下新建pages文件来保存网页.
   在pages文件夹下创建success.jsp文件来返回访问成功之后跳转的网页。
```xml
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>成功!</title>
</head>
<body>
    <h1>成功跳转!</h1>
</body>
</html>
```
-------------------------------------------

- #### 创建spring-mvc.xml配置文件:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/cache http://www.springframework.org/schema/cache/spring-cache.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 开启注解扫描 -->
    <context:component-scan base-package="com.zgh"></context:component-scan>

    <!-- 配置视图解析器 -->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 配置参数 -->
        <!-- 文件所在的目录 -->
        <property name="prefix" value="/WEB-INF/pages/"></property>
        <!-- 文件的后缀名 -->
        <property name="suffix" value=".jsp"></property>
    </bean>

    <!-- 开启spring-mvc框架注解支持 -->
    <mvc:annotation-driven/>
</beans>
```
-------------------------------------------

- #### 创建HelloController控制器类:
```java
//声明此类是个控制器类
@Controller
public class HelloController {

    //声明此方法的请求路径
    @RequestMapping(path = "/hello")
    //这个类事控制器类时,且次方法时返回String类型的时候,这个返回值就是返回你在配置文件配置的视图解析器中的页面命
    public String sayHello(){
        System.out.println("hello!");
        return "success";
    }

}
```
-------------------------------------------


- #### 编写index.jsp页面:
```xml
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false"%>
<html>
    <head>
        <title>你好!</title>
    </head>
    <body>
        <h2>入门程序</h2>
        <a href="hello">你好</a>
    </body>
</html>
```
-------------------------------------------

- #### 在web.xml文件中读取spring_mvc.xml配置文件:
```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
  
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <!-- 配置上方声明类中的属性. -->
      <!-- contextConfigLocation参数为加载配置文件的属性 -->
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring_mvc.xml</param-value>
    </init-param>
	<!-- 在服务器启动时加载此servlet -->
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
	<!-- 声明/表示拦截所有请求 -->
    <url-pattern>/</url-pattern>
  </servlet-mapping>
  
</web-app>
```
-------------------------------------------