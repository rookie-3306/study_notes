当服务器端出错的时候我们会给页面返回错误信息,这对于一般用户是很不友好的,所以我们需要配置spring的错误解析器.
	
	首先创建属于自己的异常类:
	-------------------------------------------
	public class ByZoreError extends Exception{
		String message;

		public ByZoreError(String s) {
			this.message = s;
		}

		@Override
		public String getMessage() {
			return message;
		}

		public void setMessage(String message) {
			this.message = message;
		}

	}
	-------------------------------------------
	
	编写出异常的方法:
	-------------------------------------------
	@RequestMapping(path = "/error")
    public String byZeroError() throws ByZoreError {
        try{
            int x = 1/0;
        }
        catch (Exception e){
            e.printStackTrace();
            throw new ByZoreError("0不能作为除数!");
        }
        return "success";
    }
	-------------------------------------------
	
	编写异常处理器类:
	-------------------------------------------
	public class ByZeroExceptionResolver implements HandlerExceptionResolver {
		@Override
		public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
			//ModelAndView的作用就是传递信息与跳转页面
			ModelAndView modelAndView = new ModelAndView();
			if(e instanceof ByZoreError){
				//如果类是ByZoreError就强转
				e = (ByZoreError)e;
			}else{
				e = new ByZoreError("系统未知错误!");
			}
			//在request中加入返回的数据
			modelAndView.addObject("message",e.getMessage());
			//设置跳转的页面
			modelAndView.setViewName("error");
			return modelAndView;
		}
	}
	-------------------------------------------
	
	在spring文件中配置异常处理器:
	-------------------------------------------
	 <!-- 配置异常处理器 -->
    <bean id="byZeroExceptionResolver" class="com.zgh.exception.ByZeroExceptionResolver"></bean>
	-------------------------------------------
	
	之后当异常处理器捕捉到此错误就会跳转到相应的页面.