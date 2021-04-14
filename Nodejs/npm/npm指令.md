###nmp的一些常用指令
---------------------------------
#安装webpack,@后面是版本,-g是全局安装的意思
npm install webpack@3.6.0 -g
---------------------------------
#初始化
npm init
----------------
#一些参数名称:
package name:设置包的名称
version:版本号
description:描述
entry point:入口
test command:测试指令
git repository: git仓库
keywords:关键词
author: 作者
license: 协议
---------------------------------
#package.json的一些参数的解析:
#首先声明使用的时候必须把 //注释 删掉
{
  "name": "webpackconfig",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
	//这里面存放的是npm run xxx的指令
	//key就是run后面的xxx
	//key后面的值就是本质上需要执行的指令,例如npm run build就是执行webpack命令
	//在这里面声明的指令会先寻找本地的包来执行命令
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
	//这里保存的是开发时需要保存的包
    "webpack": "^3.6.0"
  }
}
---------------------------------
#在本地安装webpack
#--save-dev表示在开发时依赖
npm install webpack@3.6.0 --save-dev
---------------------------------
