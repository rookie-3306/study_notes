RequestMapping注解:
	作用:用于建立请求URL和处理请求方法之间的对应关系.
	可以作用在方法上与类上,作用在类上就为一级目录,作用在类上就是二级目录.
	其中拥有的属性:
	
	path,value:都是声明此方法的请求路径
	-------------------------------------------
	@RequestMapping(path = "/hello"）
	-------------------------------------------
	
	method:声明此方法所接受的请求类型:
	其中属性值有:GET,HEAD,POST,PUT,PATCH,DELETE,OPTIONS,TRACE;
	-------------------------------------------
	@RequestMapping(path = "/hello",method = {RequestMethod.GET,RequestMethod.POST})
	-------------------------------------------
	
	params:声明此方法所接收的参数列表,当请求参数不匹配时返回400.
	当然你也可以指定需要匹配参数的值,例如key=value,就是代表现在不单单要匹配参数名,还要匹配参数的值.
	-------------------------------------------
	@RequestMapping(path = "/hello",params = {"username=aaa111","password"})
	-------------------------------------------
	
	headers:表示接收到的请求的请求头必须包含其中的属性.
	-------------------------------------------
	@RequestMapping(path = "/hello",headers = {"Accept"})
	-------------------------------------------
--------------------------------------------------------------------------------------



MVC的参数获取:
	在RequestMapping注释的方法上的参数列表就是获取的参数.
	例如UserController类中有个saveUser(String username,String password)方法,spring就会获取请求参数然后
		通过反射的方法填参数.
	例:
	创建UserController类:
	-------------------------------------------
	@Controller
	@RequestMapping(path = "/user")
	public class UserController {
		@RequestMapping(path = "/save")
		public String saveUser(String username,String password){
			System.out.println("用户名 => " + username);
			System.out.println("密码 => " + password);
			return "success";
		}
	}
	-------------------------------------------
	当请求url:地址:端口号/工程名/user/save?username=123&password
		之后spring就会通过反射的方法填充参数来让方法获取username与password参数的值.
	-------------------------------------------
--------------------------------------------------------------------------------------



	当需要封装的类符合javaBean要求的时候spring就可以帮我们封装对象,基本属性的封装就不用说,
		如果其中有属性是引用类型,就可以用 属性.属性的属性 来封装此属性下的属性。
	例:
	创建Accouot类:
	-------------------------------------------
	public class Account implements Serializable {
		float money;
		String describe;
		@Override
		public String toString() {
			return "Account{" +
					"money=" + money +
					", describe='" + describe + '\'' +
					'}';
		}
		public float getMoney() {
			return money;
		}
		public void setMoney(float money) {
			this.money = money;
		}
		public String getDescribe() {
			return describe;
		}
		public void setDescribe(String describe) {
			this.describe = describe;
		}
	}
	-------------------------------------------
	
	创建User类:
	-------------------------------------------
	public class User implements Serializable {
		int id;
		String username;
		String password;
		Account account;
		@Override
		public String toString() {
			return "User{" +
					"id=" + id +
					", username='" + username + '\'' +
					", password='" + password + '\'' +
					", account=" + account +
					'}';
		}
		public Account getAccount() {
			return account;
		}
		public void setAccount(Account account) {
			this.account = account;
		}
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
		public String getPassword() {
			return password;
		}
		public void setPassword(String password) {
			this.password = password;
		}
	}
	-------------------------------------------
	
	在控制器中声明方法:
	-------------------------------------------
	@RequestMapping(path = "/save")
		public String saveUser(User user){
			System.out.println(user);
			return "success";
		}
	-------------------------------------------
	
	网页中的表单提交:
	-------------------------------------------
	<form action="user/save" method="post">
		账号:<input type="text" name="username"><br/>
		密码:<input type="password" name="password"><br/>
		<!-- 用 属性.属性 的方法封装应用类型属性下的属性 -->
		账户存储金额:<input type="text" name="account.money"><br/>
		账户描述:<input type="text" name="account.describe"><br/>
		<input type="submit">
		<input type="reset">
	</form>
	-------------------------------------------
--------------------------------------------------------------------------------------



	当用spring封装lsit,map这样等复杂的类型时,我们请求的参数可以设置成name="list[index].属性"与name="map['key'].value"的形式
	例:
	创建ComplexUser类其中有属性List<User> users,Map<String,User> userMap;符合javaBean的形式.
	声明保存ComplexUser类的方法saveListAndMap:
	-------------------------------------------
	@RequestMapping(path = "/saveComplexUser")
    public String saveListAndMap(ComplexUser complexUser){
        System.out.println(complexUser);
        return "success";
    }
	-------------------------------------------
	
	创建saveUsers.jsp文件:
	-------------------------------------------
	 <body>
        <form action="user/saveComplexUser" method="post">
            账号1:<input type="text" name="users[0].username"><br/>
            密码1:<input type="password" name="users[0].password"><br/>
            账户存储金额1:<input type="text" name="users[0].account.money"><br/>
            账户描述1:<input type="text" name="users[0].account.describe"><br/>
    
            账号1:<input type="text" name="userMap['one'].username"><br/>
            密码1:<input type="password" name="userMap['one'].password"><br/>
            账户存储金额1:<input type="text" name="userMap['one'].account.money"><br/>
            账户描述1:<input type="text" name="userMap['one'].account.describe"><br/>
    
            账号2:<input type="text" name="users[1].username"><br/>
            密码2:<input type="password" name="users[1].password"><br/>
            账户存储金额2:<input type="text" name="users[1].account.money"><br/>
            账户描述2:<input type="text" name="users[1].account.describe"><br/>
    
            账号2:<input type="text" name="userMap['two'].username"><br/>
            密码2:<input type="password" name="userMap['two'].password"><br/>
            账户存储金额2:<input type="text" name="userMap['two'].account.money"><br/>
            账户描述2:<input type="text" name="userMap['two'].account.describe"><br/>
    
            <input type="submit">
            <input type="reset">
        </form>
    </body>
	-------------------------------------------
--------------------------------------------------------------------------------------



如果想要获取原生的Servlet API的时候可以直接在方法里面写上HttpServletRequest request, HttpServletResponse response
	就可以直接获取原生Servlet API:
		-------------------------------------------
		@RequestMapping(path = "/testRequest")
		public String testRequest(HttpServletRequest request, HttpServletResponse response){
			System.out.println(request);
			System.out.println(response);
			return "success";
		}
		-------------------------------------------	
--------------------------------------------------------------------------------------



请求转发与重定向:
	只要在返回的字符串前面加上修饰符就可以实现跳转与重定向.
	-------------------------------------------
	@RequestMapping(path = "/forwardPage")
    public String forwardPage(Model model){
        User user = new User();
        user.setId(1);
        user.setName("13549");
        user.setPassword("12138");
        model.addAttribute("user",user);
        return "forward:/WEB-INF/pages/success.jsp";
    }

    @RequestMapping(path = "/redirectPage")
    public String redirectPage(){
        //重定向不能请求WEB-INF文件下的文件,而且一般重定向需要写上完整的url,但是这里当我们写上相对路径的时候spring会帮我们补全
        return "redirect:/index.jsp";
    }
	-------------------------------------------
	
	也可以用RequestMapping注解跳转:
	-------------------------------------------
	@RequestMapping(path = "/testModelAndView")
    public ModelAndView testModelAndView(){
        ModelAndView mv = new ModelAndView();
        User user = new User();
        user.setId(1);
        user.setName("13549");
        user.setPassword("12138");
        //ModelAndView也会把对象存入request域中
        mv.addObject("user",user);
        //设置跳转的页面
        mv.setViewName("success");
        return mv;
    }
	-------------------------------------------