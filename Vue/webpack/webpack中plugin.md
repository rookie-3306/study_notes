#  webpack中plugin的使用.

## weboack的版权声明:
- #### 在webpack.config.js配置,导入webpack包.
`const webpack = require('webpack')`
- #### 在webpack.config.js中的plugins数组中声明.

```
...
	module: {
		...
		plugins:[
		//声明打包文件的版权
		new webpack.BannerPlugin('最终版权由所有人拥有!')
		]
	}
```

## webpack对index.html的打包:
_首先需要确认的时我们的index.html文件是存在文件的主文件目录下面的。(就是与dist,src文件属于同一级)_
- #### 安装html-webpack-plugin插件.
_这里安装的是3.6.0版本的,怕高版本会报错_
`npm install html-webpack-plugin@3.2.0 --save-dev`
- #### 在webpack.config.js中的plugins数组中进行配置.
_记得把上面申明的 `counst publicPath:'dist/'`给注释掉,要不然打包出来的文件使用url路径就会出错._
_声明应用变量:_

```
...
//声明变量
const htmlWebpackPlugin = require('html-webpack-plugin')
...
module: {
		...
		plugins:[
			...
			new htmlWebpackPlugin({
			//指定创建的模板,指定玩模板webpack会根据模板来打包文件
			template:'index.html'
			})
		]
		...
	}
```

## webpack对打包出的js文件进行压缩(丑化):
- #### 安装 html-webpack-plugin插件.
_安装指定版本,高版本怕报错._
`npm install html-webpack-plugin@3.2.0 --save-dev`
- #### 在webpack.config.js中的plugins数组中进行配置.

```
...
//声明变量
const uglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
...
module: {
		...
		plugins:[
			...
			//对文件进行压缩(丑化)
			new uglifyjsWebpackPlugin()
		]
		...
	}
```