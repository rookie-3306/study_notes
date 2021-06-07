# @Import注解的使用.
参考资料:https://www.bilibili.com/video/BV19K4y1L7MT?p=9

### 理解:
@Import注解可以声明在任意配置类或者组件上,默认的value值为字节数组,SpringBoot会自动调用此字节数组中的类的无参构造参数创建对象,然后把此对象加入容器中.

### 使用:
创建User类:

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

创建配置类:

```java
//声明此类为配置类
@Configuration(proxyBeanMethods = true)
@Import({User.class,Pat.class}) //调用该类无参构造方法注册组件加入到容器中,名字为此类的全限定类名，例如这里为com.zgh.springboot.pojo.User
public class MyConfig {
}
```

编辑主启动类:

```java
/SpringBoot的主配置文件
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