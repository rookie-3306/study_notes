# webpack的一些常用指令


---------------------------------
### 查看webpack版本
`webpack --version`
---------------------------------


---------------------------------
### 将js打包,无需考虑js文件中各种错综复杂的相互引用,webpack会自己帮我们打包好.
	_后面第一个参数是需要打包的文件位置,后面的是打包完成之后生成的文件位置._
	`webpack ./src/main.js ./dist/bundle.js`
### webpack配置文件:
	_首先我们需要在需要运行webpakc的文件夹下面创建文件webpack.config.js_
	_要初始化npm(npm init)_
	_下面时这个配置文件中的东西:_
```
//这个文件是webpack的配置文件,当在此路径下直接输入webpack命令会读取此js文件中的配置属性
//导入全局的path包
//去node包中寻找path这个包
const path = require('path')

module.exports = {
	//导入入口
	entry:'./src/main.js',
	//导出出口
	output:{
		//导出的文件路径
		//resolve()方法是拼接路径
		//__dirname是在node中的一个全局变量,表示当前所在路径
		path:path.resolve(__dirname,'dist'),
		//导出的文件名
		filename:'bundle.js'
	},
}
```
### 再main.js中导入js文件.
`import {xxx} from './js/mathUtil.js'`
---------------------------------


--------------------------------
## webpack打包css文件
	_如果遇到报错:UnhandledPromiseRejectionWarning: TypeError: this.getResolve is not a function 问题._
	_这是因为style-loader与css-loader版本过高,可以查看网址解决:https://blog.csdn.net/qq_43377853/article/details/108485499_
### 首先安装style-loader(将模块的导出作为样式添加到 DOM 中)
`npm install style-loader --save-dev`
### 继续安装css-loader(让webpack拥有解析css文件的能力,没有安装style-loader就不能将css导入到dom中)
`npm install --save-dev css-loader`
### 然后需要在webpack.config.js配置css-loader
```
const path = require('path')
module.exports = {
	
	entry:'./src/main.js',
	output:{
		path:path.resolve(__dirname,'dist'),
		filename:'bundle.js'
	},
	//配置css-loader,让webpack拥有打包css文件的能力
	module.exports = {
	  module: {
	    rules: [
	      {
	        test: /\.css$/,
	        use: [ 'style-loader', 'css-loader' ]
	      }
	    ]
	  }
	}
	
}
```
### 然后再main.js导入css文件
`import css from './css/normal.css'`

## webpack打包less文件:
	_如果遇到问题: Module build failed: TypeError: this.getOptions is not a function_
	_这是less-loader版本过高，降级到5.0.0即可，调整方法类似上方调整style-loader与css-loader版本_
### 首先时安装less-loader(打包less文件),与less(解析less文件)
`npm install --save-dev less-loader less`
### 在webpack.config.js配置webpack.config.js
```
	...
    module: {
        rules: [{
            test: /\.less$/,
            use: [{
                loader: "style-loader" // creates style nodes from JS strings
            }, {
                loader: "css-loader" // translates CSS into CommonJS
            }, {
                loader: "less-loader" // compiles Less to CSS
            }]
        }]
    }
```

## webpack打包图片文件
	_如果遇到问题:  The "from" argument must be of type string. Received undefined_
	_就是url-loader要依赖file-loader，但因file-loader版本过高报错，我将版本降到5。1.0_
### 安装url-loader与file-loader
`npm install --save-dev url-loader`
`npm install --save-dev file-loader`
### 在webpack.config.js配置webpack.config.js
```
const path = require('path')
module.exports = {
	
	entry:'./src/main.js',
	output:{
		path:path.resolve(__dirname,'dist'),
		filename:'bundle.js',
		//publicPath属性是打包时但凡涉及到url的时候会在url前面拼接上publicPath中定义的路径
		//不想用的时候删掉即可
		publicPath:'dist/'
	},
	//配置css-loader,让webpack拥有打包css文件的能力
	module: {
		rules: [
				...,
				{
					//增加后缀为.jfif与.jpeg的图片
					test: /\.(png|jpg|gif|jfif|jpeg)$/,
					use: [
							{
								loader: 'url-loader',
								options: {
									//当图片小于limit就把图片以base64编码的形式返回给浏览器,直接在浏览器上解析显示蹄片
									//当图片大于limit就把图片以引用url的形式来读取图片
									limit: 8192
									//对图片的命名扩展规范
									//img/意思是将匹配到的文件打包之后的文件放到img文件夹下
									//[name]表示该文件还未打包时的文件名(不含扩展名)
									//[hash:8],表示使用hash随机数,8为生成8位数
									//[ext]表示该文件原来还未打包的文件扩展名
									name:'img/[name].[hash:8].[ext]'
								}
							},
						],
				},
			]
	}
}
```
### 然后引入带有url文件的css文件即可.

## 用webpack把ES6语法转换为ES5,由于现在所有浏览器都能支持ES6语法所以了解即可。
	_参考网址:https://www.bilibili.com/video/BV15741177Eh?p=82&spm_id_from=pageDriver_

## webpack打包vue：
### 首先安装vue(--save表示安装在本地,这里少了-dev表示不仅仅时开发时依赖,发布时也需要依赖)
`npm install vue --save`
### 在webpack.config.js配置webpack.config.js,防止vue用runing-only的方式编译
````
const path = require('path')
module.exports = {
	
	entry:'./src/main.js',
	output:{
		path:path.resolve(__dirname,'dist'),
		filename:'bundle.js',
		publicPath:'dist/'
	},
	module: {
		rules: [
				{
					test: /\.css$/,
					use: [ 'style-loader', 'css-loader' ]
				},
				{
					test: /\.less$/,
					use: [{
							loader: "style-loader" // creates style nodes from JS strings
					}, {
							loader: "css-loader" // translates CSS into CommonJS
					}, {
							loader: "less-loader" // compiles Less to CSS
					}]
				},
				{
					test: /\.(png|jpg|gif|jfif|jpeg)$/,
					use: [
							{
								loader: 'url-loader',
								options: {
									limit: 8192,
									name:'img/[name].[hash:8].[ext]'
								}
							},
						],
				},
			]
	},
	//配置这个
	resolve:{
		//alias起别名
		alias:{
			//让打包(就是from 'vue')vue结尾的文件的时候,引用解析文件,文件位置在vue/dist/vue.esm.js下
			//如果不引用会默认用only-runing的方式解析文件
			'vue$':'vue/dist/vue.esm.js'
		}
	}
}
```
### 在main.js中引用vue创建Vue实例(记得在index.html创建vue挂载的id元素)
```
import Vue from 'vue'
const app = new Vue({
	el:'#app',
	data:{
		message:'这是vue实例中data属性中的message.'
	}
})
```

## webpack打包.vue文件.
### 首先安装vue-loader与vue-template-compiler
	_如果遇到问题请把vue-loader版本降到13.0.0_
`npm install --save-dev vue-loader vue-template-compiler`
### 在webpack.config.js配置webpack.config.js
```
	...
	module: {
		rules: [
				...
				{
					//配置解析.vue文件
					test:/\.vue$/,
					use:['vue-loader']
				}
			]
	}
	...
```
### 创建.vue文件
```
<template>
	<div>
		<h2>{{message}}</h2>
		<p>{{message1}}</p>
		<button @click="btnClick">按钮</button>
	</div>
</template>

<script>
	export default{
		data(){
			return{
				message:'这是vue实例中data属性中的message.',
				message1:'这是一个段落.'
			}
		},
		methods:{
			btnClick(){
				console.log('按钮点击了!')
			}
		}
	}
</script>

<style>
</style>
```
### 在main.js文件中引用.vue文件
```
import Vue from 'vue'
import app from './vue/app.vue'
new Vue({
	el:'#app',
	template:'<app></app>',
	components:{
		app
	}
})
```
