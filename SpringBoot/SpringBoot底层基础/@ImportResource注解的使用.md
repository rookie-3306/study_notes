# @ImportResource注解的使用.

### 理解:
@ImportResource用来引入在.xml文件中注册的JavaBean对象.

### 使用:
创建User对象:
```java
public class User {
    private String name;
    private int age;

    public User() {
    }

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}

```
在resource文件夹(资源文件夹)下编写配置文件beans.xml:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="hehe" class="com.zgh.springboot.pojo.User">
        <property name="name" value="zhangsan"></property>
        <property name="age" value="19"></property>
    </bean>
</beans>
```
编写配置类MyConfig:
```java
//声明此类为配置类
@ImportResource("classpath:beans.xml")
@Configuration()
public class MyConfig {
    @Bean //给容器中添加组件。value属性是设置此组件的名称,默认为方法名字.
    public User user01(){
        return new User("zhaogonghong",20);
    }
}
```
编写主启动类:
```java
//SpringBoot的主配置文件
@SpringBootApplication
public class BasicsApplication {

    public static void main(String[] args) {
        //获取IOC容器
        ConfigurableApplicationContext run = SpringApplication.run(BasicsApplication.class, args);

        //查看容器中的组件
        String names[] = run.getBeanDefinitionNames();
        for(String name:names){
            System.out.println(name);
        }
        System.out.println("====================================");

        //获取指定类型的组件名称数组
        String[] beanNamesForType = run.getBeanNamesForType(User.class);
        for(String name:beanNamesForType)
        {
            System.out.println(name);
        }
    }

}
```