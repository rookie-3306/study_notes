IOC入门:
首先创建xml文件bean.xml:
-------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
-------------------------------------------

在xml注册bean对象(写在beans标签中):
-------------------------------------------
<bean id="IUserDaoImp" class="com.zgh.imp.IUserDaoImp"></bean>
-------------------------------------------

编写测试类:
-------------------------------------------
public class IOCTest {
    @Test
    public void iocTest(){
        //ClassPathXmlApplicationContext方法加载类路径下的配置文件，要求配置文件必须在类路径下。
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //FileSystemXmlApplicationContext方法加载磁盘任意路径下的配置文件(必须要有访问权限)
        //ApplicationContext ac = new FileSystemXmlApplicationContext("path");
        //AnnotationConfigApplicationContext它是同于读取注解创建容器的
        //ApplicationContext ac = new AnnotationConfigApplicationContext("path");
		
		//根据id获取bean对象的方法。
        IUserDao userDao = (IUserDao) ac.getBean("IUserDaoImp");
        System.out.println(userDao);
    }
}
-------------------------------------------
---------------------------------------------------------------------------------------