## webpack本地服务器的搭建(用于调试):
- #### 首先通过npm安装webpack-dev-server.
_这里指定版本是因为为了配个webpack版本._
`npm install webpack-dev-server@2.9.3 --save-dev`
- #### 在package.json的scripts标签配置自定义指令:

```
...
"scripts": {
	...
	"dev":"webpack-dev-server --open"
	...
  }
...
```
- #### 启动本地服务器:
`npm run dev`
