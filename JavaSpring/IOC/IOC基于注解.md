先前准备在xml配置文件中注册需要扫描的包:
-------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!--  告知spring在创建容器时要扫描的包(专门用于扫描注解)  -->
    <context:component-scan base-package="com.zgh.imp"></context:component-scan>
</beans>
-------------------------------------------

Component注释:
-------------------------------------------
/**
 * Component注释:作用与XML配置文件中写一个bean标签效果是一样的(把当前类放入spring容器中)
 *      属性:
 *          value:用于指定当前bean的id,当我们没写时就是当前类名，?且开头字母小写。?
 */
-------------------------------------------

Controller,Service,Repository注释:
-------------------------------------------
/**
 * 下面三个注释与Component效果是一样的
 * Controller:一般用于表现层
 * Service:一般用在业务层
 * Repository:一般用在持久层
 */
-------------------------------------------
----------------------------------------------------------------------------

Autowired注释(用在需要被注入的属性上):
-------------------------------------------
/**
 * Autowired注释,用来自动注入.
 * 当注入的类型在bean容器中只存在一种时注入成功.(就是一种类存在不同的实例)
 * 当注入的类型在bean容器中存在多个的时候,spring会选择id与需要注入的变量的变量名相同的bean对象.
 * 当都没有时注入失败
 * Repository:一般用在持久层
 */
-------------------------------------------

编写需要被注入的类:
-------------------------------------------
@Component("iUserDaoImp")
public class IUserDaoImp implements IUserDao {
    public IUserDaoImp(){
        System.out.println("创建了IUserDao类!");
    }

    public void saveUser() {
        System.out.println("IUserDaoImp保存用户!");
    }
}
-------------------------------------------

使用注释:
-------------------------------------------
@Component("test01")
public class Test01 {
    @Autowired
    IUserDao userDao;
    public IUserDao getUserDao() {
        return userDao;
    }
    public void setUserDao(IUserDao userDao) {
        this.userDao = userDao;
    }
}
-------------------------------------------
----------------------------------------------------------------------------


Qualifier注释(里面填入的value值为需要注入的bean对象id,需要配合Autowired注释使用):
-------------------------------------------
@Component("test01")
public class Test01 {
    @Autowired
    @Qualifier("iUserDaoImp")
    IUserDao userDao;
    public IUserDao getUserDao() {
        return userDao;
    }
    public void setUserDao(IUserDao userDao) {
        this.userDao = userDao;
    }
}
-------------------------------------------

Resource注解(与上面Qualifier注释类似):
-------------------------------------------
@Component("test01")
public class Test01 {
    @Resource(name = "iUserDaoImp")
    IUserDao userDao;
    public IUserDao getUserDao() {
        return userDao;
    }
    public void setUserDao(IUserDao userDao) {
        this.userDao = userDao;
    }
}
-------------------------------------------
----------------------------------------------------------------------------


作用范围标签Scope:
-------------------------------------------
/**
 * Scope注释:作用与XML配置文件中bean对象的Scope类似
 * 里面value的值与XML配置文件中bean对象的Scope属性取值相同
 */
-------------------------------------------

PostConstruct,PreDestro标签(写在方法上)：
-------------------------------------------
/**
 * PostConstruct注释:指定类初始化调用的方法
 * PreDestro注释:指定类销毁时调用的方法
 */
-------------------------------------------
----------------------------------------------------------------------------

Bean标签(写在方法上):
-------------------------------------------
/**
 * 告诉spring容器能从下方的方法中拿到一个id为beanName的bean对象
 */
@Bean(name="beanName")
-------------------------------------------



ComponentScan注解(扫描此包下面的类):
-------------------------------------------
/**
 * 用法一:
 * 		ComponentScan("com.zgh")
 * 用法二:
 * 		ComponentScan({"com.zgh.dao","com.zgh.entity"})
 */
-------------------------------------------

import注释(导入其他配置类,中间填入类的字节码):
------------------------------------------
@import(JdbcConfig.claa)
------------------------------------------

PropertySource注释(导入配置文件,详情请看https://www.bilibili.com/video/BV1mE411X7yp?p=118&spm_id_from=pageDriver)
------------------------------------------