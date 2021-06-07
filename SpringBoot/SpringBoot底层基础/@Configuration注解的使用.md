# @Configuration注解的使用.
参考资料:https://www.bilibili.com/video/BV19K4y1L7MT?p=8

### 理解:
@Configuration是声明此类为一个配置类,一般起到加载组件的作用.

### 使用:
- #### 查看以下代码:
声明User类

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

声明配置类,然后再配置类中注册User类:

```java
//声明此类为配置类
@Configuration
public class MyConfig {
    @Bean //给容器中添加组件。value属性是设置此组件的名称,默认为方法名字.
    public User user(){
        return new User("zhaogonghong",20);
    }
}
```

到主启动类中运行返回结果:

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

        //从容器中取出在配置类注册的组件
        boolean extendUser = run.containsBean("user");
        System.out.println("容器中是否存在组件user: " + extendUser);
        if(extendUser){
            User user = run.getBean("user",User.class);
            System.out.println(user.toString());
        }
    }

}
```

- #### 特别注意@Configuration注解中的proxyBeanMethods属性:
此属性默认值为true,就是默认让SpringBoot代理此类中的方法,当多次调用注册组件时候的方法就会返回一个引用对象,就是保持组件的单例性,此属性主要解决的是组件依赖的问题.
当然如果没有组件依赖的时候设置为false可以让程序执行效率更快.

请看下面演示:
创建Pat类:

```java
public class Pat {
    private String name;

    public Pat() {
    }

    public Pat(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Pat{" +
                "name='" + name + '\'' +
                '}';
    }
}

```

修改User类,在其中增加Pat属性:

```java
public class User {
    private String name;
    private int age;
    private Pat pat;

    public User() {
    }

    public User(String name, int age, Pat pat) {
        this.name = name;
        this.age = age;
        this.pat = pat;
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

    public Pat getPat() {
        return pat;
    }

    public void setPat(Pat pat) {
        this.pat = pat;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", pat=" + pat +
                '}';
    }
}

```

编写配置类:

```java
//声明此类为配置类
@Configuration(proxyBeanMethods = true)
public class MyConfig {
    @Bean //给容器中添加组件。value属性是设置此组件的名称,默认为方法名字.
    public User user(){
        //该组件注册的时候依赖下方的pat组件,这时候@Configuration中的proxyBeanMethods就要为true
        return new User("zhaogonghong",20,pat());
    }

    @Bean
    public Pat pat(){
        return new Pat("tom");
    }
}
```

编辑主启动类:

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

        //从容器中取出在配置类注册的组件
        boolean extendUser = run.containsBean("user");
        System.out.println("容器中是否存在组件user: " + extendUser);
        if(extendUser){
            User user = run.getBean("user",User.class);
            System.out.println(user.toString());
        }

        //从容器中取出自己的配置类:
        MyConfig myConfig = run.getBean(MyConfig.class);
        //调用MyConfig中的方法
        User user01 = myConfig.user();
        User user02 = myConfig.user();
        System.out.println("user01 == user02: " + (user01==user02));
    }

}
```
