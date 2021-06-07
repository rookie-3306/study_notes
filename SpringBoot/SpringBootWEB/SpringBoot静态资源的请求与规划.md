# SpringBoot静态资源请求与规划:
参考资料:https://www.bilibili.com/video/BV19K4y1L7MT?p=23

### 静态资源的访问:
在类路径下:`/static`or`/public`or`/resources`or`/META-/resources`文件夹下的文件都能直接用 当前项目路径/ + 静态资源名 就能直接访问.
原理:静态映射/**
请求进来,先去找Controller能不能处理,不能处理再交给静态资源处理器,如果静态资源处理器也无法请求就返回404.

### 静态资源访问前缀:
在核心配置文件中设置访问静态资源文件时的访问前缀:
```properties
spring.mvc.static-path-pattern=/resources/**
```
配置了以上信息的时候访问静态资源文件的时候就需要 当前项目路径/ + resources/ + 静态资源名 .
一般配置了之后是防止过滤器拦截了静态资源文件,直接让过滤器放行指定路径的请求就行.

### 配置静态资源文件夹:
在核心配置文件中设置静态资源文件夹:
```properties
spring.web.resources.static-locations=classpath:/temp/
```
当然,配置了上面的静态资源文件夹之后,那些默认的静态资源文件夹例如`/static`、`/public`之类的静态资源文件夹就失去了效果.

