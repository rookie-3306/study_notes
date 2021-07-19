# 02-SpringBoot集成Swagger与简单使用.
常用注解查看网址:https://blog.csdn.net/kang9399052316/article/details/105765068

### SpringBoot配置Swagger:
- #### 在pom.xml文件中引入swagger坐标:
```xml
    <dependencies>
        <!--swagger-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <scope>provided </scope>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <scope>provided </scope>
        </dependency>
    </dependencies>
```
- #### 创建配置类,引入Swagger组件.
```java
//声明该类为SpringBoot配置类.
@Configuration
//声明启用Swagger
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket webApiConfig(){

        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("webApi")
                .apiInfo(webApiInfo())
                .select()
                .paths(Predicates.not(PathSelectors.regex("/admin/.*")))
                .paths(Predicates.not(PathSelectors.regex("/error.*")))
                .build();

    }

    private ApiInfo webApiInfo(){
        //这里声明的都是Swagger网页最上方的声明信息
        return new ApiInfoBuilder()
                .title("网站-课程中心API文档")
                .description("本文档描述了课程中心微服务接口定义")
                .version("1.0")
                .contact(new Contact("Helen", "http://atguigu.com", "55317332@qq.com"))
                .build();
    }
}
```
---------------------------
### 简单的使用Swagger:
- #### 在写好的Controller中写上Swagger注解:
```java
@Api()
@ApiModel(description = "讲师管理")
@RestController
@RequestMapping("/eduservice/teacher")
public class EduTeacherController {

    private EduTeacherService service;
    @Autowired
    public void setService(EduTeacherService service) {
        this.service = service;
    }

    @ApiOperation(value = "返回所有讲师信息.")
    @GetMapping(path = "/findAll")
    public List<EduTeacher> findAllTeacher(){
        List<EduTeacher> teachers = service.list(null);
        return teachers;
    }

    @ApiOperation(value = "按照传入ID删除讲师信息.")
    @DeleteMapping(path = "/delete/{id}")
    public boolean remoteTeacher(@ApiParam(name = "id",value = "讲师ID",required = true) @PathVariable(name = "id") String id){
        boolean flush = service.removeById(id);
        return flush;
    }
}
```
- #### 然后登录网址`url/工程路径/swagger-ui.html`来查看接口文档.

### 后记:
- @API
使用在类上，表明是swagger资源，@API拥有两个属性:value、tags，源码如下
```java
//If tags is not used,this value will be used to set the tag for the operations described by this resource. Otherwise, the value will be ignored.
 String value() default "";

 //Tags can be used for logical grouping of operations by resources or any other qualifier.
 String[] tags() default {""};
```
生成的api文档会根据tags分类，直白的说就是这个controller中的所有接口生成的接口文档都会在tags这个list下；tags如果有多个值，会生成多个list，每个list都显示所有接口
```java
@Api(tags = "列表1")
@Api(tags = {"列表1","列表2"})
```
value的作用类似tags,但是不能有多个值.

------------------

- @ApiOperation
使用于在方法上，表示一个http请求的操作
源码中属性太多，记几个比较常用
value用于方法描述
notes用于提示内容
tags可以重新分组（视情况而用）

------------------

- @ApiParam
使用在方法上或者参数上，字段说明；表示对参数的添加元数据（说明或是否必填等）
name–参数名
value–参数说明
required–是否必填

------------------

- @ApiModel()
使用在类上，表示对类进行说明，用于参数用实体类接收
value–表示对象名
description–描述

------------------

- @ApiModelProperty()
使用在方法，字段上，表示对model属性的说明或者数据操作更改
value–字段说明
name–重写属性名字
dataType–重写属性类型
required–是否必填
example–举例说明
hidden–隐藏