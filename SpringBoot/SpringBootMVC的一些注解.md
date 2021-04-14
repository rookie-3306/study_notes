#SpringBoot一些新增注解:
---------------------------------
//此标签的作用首先是声明该类是控制器类@Controller,然后表示里面方法返回值都加了@ResponseBody注解
@RestController
@RequestMapping(path = "/index")
public class IndexController {

    //此标签的作用相当于:@RequestMapping(path = "/getUser",method = {RequestMethod.GET})
    //一般用于查询数据的时候用
    @GetMapping(path = "/getUser")
    public User getUser(Integer id){
        User user = new User();
        user.setId(id);
        return user;
    }

    //此标签作用相当于:@RequestMapping(path = "/saveUser",method = {RequestMethod.POST})
    //一般用于增加数据时候用
    @PostMapping(path = "/saveUser")
    public String saveUser(){
        return "保存用户成功!";
    }

    //此标签作用相当于:@RequestMapping(path = "/updateUser",method = {RequestMethod.PUT})
    //一般用于更新数据时候用
    @PutMapping(path = "/updateUser")
    public String updateUser(){
        return "更新数据成功!";
    }

    //此标签作用相当于:@RequestMapping(path = "/deleteUser",method = {RequestMethod.DELETE})
    //一般用于删除数据时候用
    @DeleteMapping(path = "/deleteUser")
    public String deleteUser(){
        return "删除数据成功!";
    }

    //RESTful编程风格，用的是@PathVariable注解,里面的value值是url中对应的参数名
    //一般使用RESTful编程风格编程的时候会出现路径上的参数混淆,比如插入与查询的都是只有参数id
    //这时候我们就可以用不同的请求方式区分
    //@GetMapping,@PostMapping,@PutMapping,@DeleteMapping
    @GetMapping(path = "/rest/{id}")
    public String restGet(@PathVariable("id") Integer id){
        return "RESTful查询用户成功,id => " + id;
    }

    @PostMapping(path = "/rest/{id}")
    public String restSave(@PathVariable("id") Integer id){
        return "RESTful保存用户成功,id => " + id;
    }

    @PutMapping(path = "/rest/{id}")
    public String restUpdate(@PathVariable("id") Integer id){
        return "RESTful更新用户成功,id => " + id;
    }

    @DeleteMapping(path = "/rest/{id}")
    public String restDelete(@PathVariable("id") Integer id){
        return "RESTful删除用户成功,id => " + id;
    }
}

---------------------------------