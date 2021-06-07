## SpringBoot使用Filter.

### 第一种使用注解的方式:
- #### 编写过滤器类MyFilter:

```java
//声明此类是过滤器类
@WebFilter(urlPatterns = "/myFilter")
public class MyFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("访问到MyFilter过滤器.");
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {

    }
}
```

- #### 在SpringBoot启动文件中扫描Filter所在的包:

```java
@SpringBootApplication
//扫描过滤器所在的包
@ServletComponentScan(basePackages = "com.zgh.springboot.filter")
public class SpringbootFilterApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootFilterApplication.class, args);
    }

}
```

## 第二种使用注册组件的方式：

- #### 创建过滤器类UserFilter:

```java
public class UserFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("你已经进入UserFilter.");
        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {

    }
}
```

- #### 创建配置类FilterConfig:

```java
//声明此类为配置类
@Configuration
public class FilterConfig {
    @Bean
    public FilterRegistrationBean myFilterRegistrationBean(){
        //注册配置器类
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(new UserFilter());
        //设置过滤路径
        filterRegistrationBean.addUrlPatterns("/user/*");
        return filterRegistrationBean;
    }
}
```