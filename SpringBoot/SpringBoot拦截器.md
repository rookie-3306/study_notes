## SpringBoot拦截器的使用.
- #### 拦截器就是拦截请求, 对请求进行预处理.

## 创建拦截器.
- #### 声明User类:

```java
public class User {
    private int id;
    private String username;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }
}
```

- #### 声明控制器UserInterceptor类:

```java
public class UserInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //编写业务拦截规则
        User user = (User) request.getSession().getAttribute("user");
        if(user == null){
            //没有权限就跳转到错误界面
            request.getRequestDispatcher("err").forward(request,response);
            return false;
        }
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    }
}
```

## 编写控制器类UserController:

```java
@Controller
@RequestMapping(path = "/user")
public class UserController {

    @RequestMapping(path = "/login")
    public @ResponseBody Object login(HttpServletRequest request){
        //模拟登录
        User user = new User();
        user.setId(1);
        user.setUsername("12138");
        request.getSession().setAttribute("user",user);
        return "登录成功!";
    }

    @RequestMapping(path = "/center")
    public @ResponseBody Object Center(){
        return "登录成功,访问到核心.";
    }

    @RequestMapping(path = "/out")
    public @ResponseBody Object Out(){
        return "访问不用登录的out成功!";
    }

    @RequestMapping(path = "/err")
    public @ResponseBody Object Error(){
        return "权限不足,请登录再尝试!";
    }

}
```

## 在配置类中注册拦截器:

```java
//定义此类为配置类
@Configuration
public class InterceptorConfig implements WebMvcConfigurer {
    //重写添加拦截器方法
    @Override
    public void addInterceptors(InterceptorRegistry registry) {

        //要拦截的请求路径
        String[] addPathPatterns = {
                //拦截/user/下面的所有请求
                "/user/**"
        };

        //要排除的路径
        String[] excludePathPatterns = {
                //排除掉请求路径/user/out
                "/user/out",
                "/user/err",
                "/user/login"
        };

        registry.addInterceptor(new UserInterceptor()).addPathPatterns(addPathPatterns).excludePathPatterns(excludePathPatterns);
    }
}
```