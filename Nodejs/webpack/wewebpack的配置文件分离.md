## webpack的配置文件分离:
- #### 用npm安装webpack-merge包.
`npm install webpack-merge@4.1.5 --save-dev`
- #### 在项目目录下新建build文件夹来保存配置文件.
- #### 在build文件夹下创建base.config.js、prod.config.js与dev.config.js文件.
_base.config.js:用来保存最基本的配置信息._
_prod.config.js:用来保存发布时的配置信息._
_dev.config.js:用来保存开发环境时的配置信息._
- ###### base.config.js文件:

```
//这个文件是webpack的配置文件,当在此路径下直接输入webpack命令会读取此js文件中的配置属性

const webpack = require('webpack')
const path = require('path')
const htmlWebpackPlugin = require('html-webpack-plugin')
const uglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
module.exports = {
	
	entry:'./src/main.js',
	output:{
		//设置打包出来的路径(看好文件打包出来的位置)
		path:path.resolve(__dirname,'../dist'),
		//设置打包出来的文件名
		filename:'bundle.js',
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
				{
					test:/\.vue$/,
					use:['vue-loader']
				}
			]
	},
	resolve:{
		alias:{
			'vue$':'vue/dist/vue.esm.js'
		}
	},
	plugins:[
		new webpack.BannerPlugin('最终版权由所有人拥有!'),
		new htmlWebpackPlugin({
			template:'index.html'
		}),
}
```

- #### prod.config.js文件:

```
const uglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config.js')
module.exports = webpackMerge(baseConfig,{
	plugins:[
		//对打包出来的js文件进行压缩
		new uglifyjsWebpackPlugin()
	],
})
```

- #### dev.config.js文件:

```
const webpackMerge = require('webpack-merge')
const baseConfig = require('./base.config.js')
module.exports = webpackMerge(baseConfig,{
	devServer:{
		//本地服务绑定的文件加(为那个文件夹服务)
		contentBase:'./dist',
		//是否时时监控代码的改变
		inline:true
	}
})
```

- #### 修改packge.json文件,手动设置配置文件位置:

```
...
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --config ./build/prod.config.js",
    "dev": "webpack-dev-server --open --config ./build/dev.config.js"
  },
...
```