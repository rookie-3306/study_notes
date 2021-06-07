## SpringBoot整合Redis:

### 在pom.xml文件引入SpringBoot集成redis起步依赖:

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
        <version>2.3.4.RELEASE</version>
    </dependency>
```

### 在核心文件中配置redus:

```java
#设置redis
spring.redis.host=47.103.203.91
spring.redis.port=6379
spring.redis.password=zhao254128661
```

### 编写类来调用SpringBoot对redis的操纵方法:

```java
package com.example.redisdemo.web;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class StudentController {

    //SpringBoot提供的操作Redis的模板
    private RedisTemplate<Object,Object> redisTemplate;
    @Autowired
    public void setRedisTemplate(RedisTemplate<Object,Object> redisTemplate){
        this.redisTemplate = redisTemplate;
    }

    @RequestMapping(path = "/put")
    public @ResponseBody Object put(String key, String value){
        //Redis中存入键值对
        redisTemplate.opsForValue().set(key, value);
        return "值已经放入redis";
    }

    @RequestMapping(path = "/get")
    public @ResponseBody Object get(String key){
        //Redis中取出键值对
        Object value = redisTemplate.opsForValue().get(key);
        return "返回的value值为:" + value;
    }
}

```

### 另外:
- #### 关于自己linux服务器上安装radis服务:
https://blog.csdn.net/qq_39135287/article/details/83474865
- #### 如果是阿里云服务器连接不上radis问题:
https://blog.csdn.net/qq_41570843/article/details/107254460