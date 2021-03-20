首先在Maven中导入能解析execution表达式的包:
-------------------------------------------
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.6</version>
        </dependency>
-------------------------------------------

创建类:
	IAccountService接口类:
-------------------------------------------
public interface IAccountService {
    void saveAccount();
    void updateAccount(int id);
    int deleteAccount();
}
-------------------------------------------

	IAccountService实现类实现IAccountService接口:
-------------------------------------------
public class IAccountServiceImp implements IAccountService {
    public void saveAccount() {
        System.out.println("执行了保存账户!");
    }

    public void updateAccount(int id) {
        System.out.println("执行了更新账户,用户id => " + id);
    }

    public int deleteAccount() {
        System.out.println("执行了删除账户!");
        return 0;
    }
}
-------------------------------------------
	
	创建日志类,来打印日志:
-------------------------------------------
public class Logger {
    public void printLog(){
        System.out.println("执行了保存日志方法!");
    }
}
-------------------------------------------

	配置bean.xml文件(在其中声明AOP文件)
-------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
    <bean id="accountServiceImp" class="com.zgh.imp.IAccountServiceImp"></bean>
    <bean id="logger" class="com.zgh.log.Logger"></bean>

    <!-- 配置AOP -->
    <aop:config>
        <!-- id是当前配置的aop的唯一标识符,ref为取得的bean对象的类 -->
        <aop:aspect id="logAdvice" ref="logger">
            <!-- before为前置通知 -->
            <!-- method为上方取到类中的方法,pointcut表示需要被增强的方法 -->
            <!-- pointcut中配置的是execution表达式来 -->
            <!-- execution写法:execution(访问修饰符 返回值 包名.....类名.方法名(参数列表)) -->
            <aop:before method="printLog" pointcut="(execution(public  void com.zgh.imp.IAccountServiceImp.saveAccount()))"></aop:before>
        </aop:aspect>
    </aop:config>
</beans>
-------------------------------------------