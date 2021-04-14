#配置端口号
server.port=8080
#配置上下文根
server.servlet.context-path=/springBoot
#另外的一种写法:
server:
	port:8080
	servlet:
		context-path:/springBoot
------------------------------------------------------------------

#配置文件的读取.
#编写新的配置文件需要命名如下application-xxxxx
#这是命名规范,当要读取配置文件的时候就需要去核心配置文件中spring.profiles.active=xxxxx中读取配置文件
spring.profiles.active=test
------------------------------------------------------------------

#获取在配置文件中自定义的值.
#首先在配置文件中写入自己的值
#自定义数值:
school.name=YeJiDaXue
school.url=www.baidu.com
---------------------------------
//在类中获取自定义的数值:
@Value("${school.name}")
    String schoolName;

@Value("${school.url}")
String schoolUrl;
------------------------------------------------------------------

#自定义配置类:
#需要在之后被注入的类上名声明注释,交给spring管理
#自定义配置类的数值:
person.name=ZhangSan
person.age=20
---------------------------------
//声明该类交给spring管理
@Component
//声明该类是个配置类,配置的属性就在springBoot核心配置文件中,其中prefix为该类在配置文件中的前缀名,这里为person
@ConfigurationProperties(prefix = "person")
public class Person {
    String name;
    int age;

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
}
---------------------------------
	//声明配置类
	Person person;
	//由于类交给spring管理了,这里自动注入就行
    @Autowired
    public void setPerson(Person person) {
        this.person = person;
    }

    @RequestMapping(path = "/customConfigurationProperties")
    public @ResponseBody String customConfigurationProperties(){
        return "person.name => " + person.getName() + "\nperson.age => " + person.getAge();
    }
------------------------------------------------------------------

