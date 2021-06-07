# @ConfigurationProperties(配置绑定)的使用.
参考资料:https://www.bilibili.com/video/BV19K4y1L7MT?p=13

### 理解:
在需要被注册到容器中的Bean对象类上声明该注解,在注解中配置上前缀,然后再主配置文件中用 前缀.属性 的方式注入属性.

### 使用:
创建Car类:
```java
//将此类交给spring管理
@Component
//配置绑定,prefix属性表示在主配置文件中的前缀
@ConfigurationProperties(prefix = "mycar")
public class Car {
    private String name;
    private Integer price;

    public Car(String name, Integer price) {
        this.name = name;
        this.price = price;
    }

    public Car() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getPrice() {
        return price;
    }

    public void setPrice(Integer price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "name='" + name + '\'' +
                ", price=" + price +
                '}';
    }
}
```
编写主配置文件类:
```java
mycar.name=tesila
mycar.price=10000
```
编写BaseController控制类:
```java
@Controller
public class BaseController {

    private Car car;
    @Autowired
    public void setCar(Car car) {
        this.car = car;
    }

    @RequestMapping(path = "/getCar")
    public @ResponseBody Car getCar(){
        return car;
    }
}
```