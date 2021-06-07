# SpringBoot的文件上传.

### 介绍:
SpringBoot文件上传的时候,已经帮我们封装好了处理类,我们只要把需要接收的文件的变量类型设置为`MultipartFile`就可以很愉快的处理了.

### 前期准备:
- #### 设置上传时的表单:
```html
<!-- 需要设置必要的属性enctype="multipart/form-data"与method="post" -->
<form action="upload" method="post" enctype="multipart/form-data">
    <input type="file" name="image"><br/>
    <!-- multiple:当设置了属性之后就能实现同时上传多个文件 -->
    <input type="file" multiple name="files"><br/>
    <input type="submit" value="提交">
</form>
```
- #### 设置SpringBoot核心配置文件,把上传文件的大小最大值调到合适的值:
```properties
#设置上传文件的最大大小
spring.servlet.multipart.max-file-size=10MB
#设置上传请求的最大大小
spring.servlet.multipart.max-request-size=10MB
```

### 使用:
- #### 接收文件并保存:
```java
@Controller
public class MutiparFileController {
    @RequestMapping(path = "/upload")
    public @ResponseBody Object upload(@RequestPart("image") MultipartFile image, @RequestPart("files") MultipartFile[] files) throws IOException {
        System.out.println("接收到传送文件请求!");
        if(!image.isEmpty()){
            //获取上传时的文件名(包括扩展名),就是文件的名称
            String originalFileName = image.getOriginalFilename();
            System.out.println(originalFileName);
            //调用次方法就可以直接保存到指定的文件夹下
            image.transferTo(new File("E:/上传的文件/" + originalFileName));
        }
        if(files.length > 0){
            for(MultipartFile item:files){
                String originalFilename = item.getOriginalFilename();
                System.out.println(originalFilename);
                item.transferTo(new File("E:/上传的文件/" + originalFilename));
            }
        }
        return "上传成功!";
    }
}
```