# lombok的开发与安装.

### lombok:
- #### 简介:
lombok能让我们编辑注解的方式让一个类直接变为JavaBean对象,来简化开发.
- #### 使用方式:
##### 首先在pom文件中导入lombok坐标:
```xml
        <!-- 引入lombokm,由于这是springboot项目所以版本号由springboot管理,我们就用父文件所指定的版本 -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```
##### 在IDEA中加入lombok插件:
File -> Settings -> Plugin 中搜索lombok插件安装,之后重启IDEA就可以.
##### 使用:
```java
//将所有属性设置get与set方法
@Data
//创建设置所有属性的构造方法.当然有些时候不需要设置所有属性的时候我们就需要自己手动写上适合构造方法.
@AllArgsConstructor
//创建空参的构造方法
@NoArgsConstructor
//设置toString方法
@ToString
public class Pat {
    private String name;
}
```

### dev-tools
- #### 介绍:
热部署(其实是自动重启)项目
- #### 使用方式:
##### 在pom文件中导入坐标:
```xml
        <!-- 导入devTools坐标 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
```
##### 一般用修改完项目之后用Ctrl+F9.

### 消除SpringBoot自定义配置类绑定提示:
- #### 使用方式:
##### 在pom文件中导入坐标:
```xml
        <!-- 消除自定义配置类的绑定提示 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
        </dependency>
```
##### 由于这个包是开发的时候引用,所以要然Maven打包时不需要添加:
```xml
<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <!-- 告诉SpringBoot打包时不要加入的jar包,一般这些包都是开发时依赖,运行时不依赖 -->
                            <groupId>org.springframework.boot</groupId>
                            <artifactId>spring-boot-configuration-processor</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
```