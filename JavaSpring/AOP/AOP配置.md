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
    void errorAccount();
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

    public void errorAccount() {
        int x = 1/0;
    }
}
-------------------------------------------
	
	创建日志类,来打印日志:
-------------------------------------------
public class Logger {
    public void printLog(){
        System.out.println("执行了保存日志方法(前置通知)!");
    }

    public void afterReturningPrintLog(){
        System.out.println("后置通知!");
    }

    public void afterThrowingPrintLog(){
        System.out.println("异常通知通知!");
    }

    public void afterPrintLog(){
        System.out.println("最终通知!");
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

	execution表达式的统配写法:
-------------------------------------------
<aop:config>
        <!-- id是当前配置的aop的唯一标识符,ref为取得的bean对象的类 -->
        <aop:aspect id="logAdvice" ref="logger">
            <!-- before为前置通知 -->
            <!-- method为上方取到类中的方法,pointcut表示需要被增强的方法 -->
            <!-- pointcut中配置的是execution表达式来 -->
            <!-- execution写法:execution(访问修饰符 返回值 包名.....类名.方法名(参数列表)) -->
            <!-- execution通配写法:首先访问修饰符可以省略,返回值可以用*来代替任意返回值,包名用*.来声明,..来声明此包下面所有的包 -->
            <!-- 方法名也能用*来代替,其中参数列表也是写成全限定类名,如果参数列表用..来填充就是任意参数,或者也可以没有参数 -->
            <!-- 下面execution表达式的意思:匹配任意返回值,在包com.zgh.imp下任意的类中拥有任意参数的任意方法 -->
            <aop:before method="printLog" pointcut="(execution(* com.zgh.imp.*.*(..)))"></aop:before>
        </aop:aspect>
    </aop:config>
-------------------------------------------

各种通知:
-------------------------------------------
<aop:config>
        <!-- id是当前配置的aop的唯一标识符,ref为取得的bean对象的类 -->
        <aop:aspect id="logAdvice" ref="logger">
			<!-- befer为前置通知 -->
            <aop:before method="printLog" pointcut="(execution(* com.zgh.imp.IAccountServiceImp.saveAccount()))"></aop:before>
            <!-- after-returning为后置通知 -->
            <aop:after-returning method="afterReturningPrintLog" pointcut="(execution(* com.zgh.imp.IAccountServiceImp.saveAccount()))"></aop:after-returning>
            <!-- after为最终通知 -->
            <aop:after method="afterPrintLog" pointcut="(execution(* com.zgh.imp.IAccountServiceImp.saveAccount()))"></aop:after>
            <!-- after-throwing为异常通知 -->
            <aop:after-throwing method="afterThrowingPrintLog" pointcut="(execution(* com.zgh.imp.IAccountServiceImp.errorAccount()))"></aop:after-throwing>
        </aop:aspect>
    </aop:config>
-------------------------------------------

由于切入点表达式太过于冗长,可以编写pointcut标签然后进行引用:
-------------------------------------------
			<!-- 编写切入点表达式 -->
			<!-- 此标签如果写在aspect标签里面此切入点表达式就只能当前在aspect标签中使用 -->
			<!-- 此标签如果写在aspect标签外面此切入点表达式就可以在整个aop:config标签内使用(!如果写在外面就需要写在aspect标签前面,约束问题) -->
            <aop:pointcut id="saveAccount_pt" expression="(execution(* com.zgh.imp.IAccountServiceImp.saveAccount()))"/>
			<!-- 应用切入点表达式 -->
			<aop:before method="printLog" pointcut-ref="saveAccount_pt"></aop:before>
-------------------------------------------

环绕通知具体看网站:https://www.bilibili.com/video/BV1mE411X7yp?p=139&spm_id_from=pageDriver
