# SpringBoot使用Serlvet原生组件.
使用原生组件编写的Servlet不经过你在SpringBoot中注册的Filter,你需要编写原生的Servlet中的拦截器来拦截.

### 第一种使用方式(直接声明然后加上注释):
- #### 编写Servlet类:
```java
// 声明该类是Servlet,并设置请求路径
@WebServlet(urlPatterns = "/my")
public class MyServlet extends HttpServlet{
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=utf-8");
        resp.getWriter().write("MyServlet.");
    }
}
```
- #### 编写Filter类:
```java
//声明该类为Filter类,并在里面配置拦截路径(这里表示拦截所有请求)
@WebFilter(urlPatterns = {"/*"})
public class MyFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("MyFilter初始化成功!");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("MyFilter工作");
        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {

    }
}
```
- #### 编写ServletContextListener类:
```java
// 声明该类是ServletContextListener
@WebListener
public class MyServletContextListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("web容器初始化成功!");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("web容器销毁成功!");
    }
```
- #### 最后在SpringBoot启动类声明需要扫描Servlet组件存放的包:
```java
@SpringBootApplication
//指定SpringBoot要扫描Servlet基本组件所在的包
@ServletComponentScan(basePackages = "com.zgh")
public class OriginalApplication {

    public static void main(String[] args) {
        SpringApplication.run(OriginalApplication.class, args);
    }

}
```

### 第二种方式(SpringBoot注册组件的方式):
- #### 首先编写类都与上面一样,然后把注解给去掉, 不要让SpringBoot扫描.
- #### 编写配置类,注册组件:
```java
//声明该类为配置类
@Configuration
public class MyRegistrationConfig {
    @Bean
    public ServletRegistrationBean myServlet(){
        MyServlet myServlet = new MyServlet();
        //把改Servlet交给SpringBoot管理
        //后面的参数是请求路径,是一个字符串数组,表示一个Servlet可以处理多个请求
        return new ServletRegistrationBean(myServlet,"/my","my01");
    }

    @Bean
    public FilterRegistrationBean myFilter(){
        MyFilter myFilter = new MyFilter();
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(myFilter);
        //设置拦截器拦截路径
        filterRegistrationBean.setUrlPatterns(Arrays.asList("/*"));
        return filterRegistrationBean;
    }

    @Bean
    public ServletListenerRegistrationBean myListener(){
        MyServletContextListener myServletContextListener = new MyServletContextListener();
        return new ServletListenerRegistrationBean(myServletContextListener);
    }
}
```