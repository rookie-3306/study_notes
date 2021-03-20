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
@Component("accountServiceImp")
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
@Component("logger")
//表示当前是个切面类
@Aspect
public class Logger {

    //配置切入点表达式,引用时的id使用的是下方修饰方法的方法名
    @Pointcut("(execution(* com.zgh.imp.IAccountServiceImp.saveAccount()))")
    void pointcut(){}

    //前置通知(其中填入的可以是切入点表达式,也可以是切入点表达式的id)
    @Before("pointcut()")
    public void printLog(){
        System.out.println("执行了保存日志方法(前置通知)!");
    }

    //后置通知
    @AfterReturning("pointcut()")
    public void afterReturningPrintLog(){
        System.out.println("后置通知!");
    }

    //异常通知
    @AfterThrowing("(execution(* com.zgh.imp.IAccountServiceImp.errorAccount()))")
    public void afterThrowingPrintLog(){
        System.out.println("异常通知通知!");
    }

    //最终通知
    @After("pointcut()")
    public void afterPrintLog(){
        System.out.println("最终通知!");
    }
}
-------------------------------------------

配置bean.xml文件:
-------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 配置要扫描的包 -->
    <context:component-scan base-package="com.zgh"></context:component-scan>

    <!-- 开启AOP注解的支持 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>

</beans>
-------------------------------------------

调用:
-------------------------------------------
public class Test {
    public static void main(String[] args) {
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        IAccountService accountService = ac.getBean("accountServiceImp",IAccountService.class);
        accountService.saveAccount();
        accountService.errorAccount();
    }
}
-------------------------------------------