# 入门
## 当下载遇到问题的时候可以看看网址:https://www.cnblogs.com/lgx5/p/10732016.html
## Vue CLI的安装:
`npm install -g @vue/cli`
## 拉取旧版本vue模板
`npm install @vue/cli-init`
## 查看Vue CLI的版本:
`vue --version`
## 用Vue CLI.2x的方式创建文件:
`vue init webpack 文件名 `
- #### Project name:项目名称
- #### Project description:项目描述
- #### Author:作者
- #### Vue build:Vue构建方式
- #### Install vue-router:安装路由
- #### Use ESLint to lint your code:是否选择编码规范
- #### Pick an ESLint preset:编码规范的样式(选了启动编码规范才会出现此选项)
- #### Set up unit tests:启用单元测试
- #### Setup e2e tests with Nightwatch: end to end,测试框架
- #### Should we run `npm install` for you after the project has been created?:选择包管理工具(npm或者yarn)

## 用Vue CLI.3x的方式创建文件:
`vue create 文件名`
_然后进入选择界面(按空格选中,其中很多选项更用Vue CLI.2x创建的意思相同这里挑出不同的来说明.):_
- #### Choose Vue version: 选择视图版本.
- #### Babel: 使用Babel(就是ES6转ES5).
- #### TypeScript: 使用TypeScript.
- ####  Progressive Web App (PWA) Support: 使用更先进的WebApp.
_之后进入版本选择:这里选择3.x._
_然后跳出来选择配置文件的存储方式 Where do you prefer placing config for Babel, ESLint, etc.?:_
- #### In dedicated config files: 把配置文件单独出来.
- #### In package.json: 把配置文件放在package.json中.
_Save this as a preset for future projects?:是否把刚刚选中的配置单独保存起来为一种模式?_
_选择之后就会叫你输入刚刚配置的名字.配置保存起来之后就能在创建文件选择模板的时候就能选择你的自定义模板._

## runtime-compiler和runtime-only的区别:
_查看网址:https://www.bilibili.com/video/BV15741177Eh?p=96_

## Vue CLI.3x打开vue配置:
`vue ui`
_然后打开对应项目文件夹导入就可以编辑当前项目的配置._

