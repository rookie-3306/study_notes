# @Conditional(条件装配)注解的使用.
参考资料:https://www.bilibili.com/video/BV19K4y1L7MT?p=10

### 理解:
Conditional翻译过来就是条件做...也即是满足条件就执行,例如继承该注解的子类注解@ConditionalOnBean就是当Spring容器中存在该Bean就执行,否则就不执行,相反的就有注解@ConditionalOnMissingBean,当不存在该Bean就执行.

### 使用:
具体查看上面的参考资料网址.