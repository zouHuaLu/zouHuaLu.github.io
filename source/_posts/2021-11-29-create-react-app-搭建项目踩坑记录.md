---
title: create-react-app 搭建项目踩坑记录
date: 2021-11-29 19:44:25
tags: React
categories: 技术类-React
---
<meta name="referrer" content="no-referrer"/>



# 中文说明

## 搭建前端开发环境笔记

由`npx create-react-app articles_published_system`创建的项目

后来漏掉了`typescript`支持，原本可以由命令`npx create-react-app articles_published_system --template typescript`可以直接创建支持ts的应用

后续要将ts添加到已有项目中，用以下命令：

`npm install --save typescript @types/node @types/react @types/react-dom @types/jest`

项目中配置`sass`:

1. 执行`yarn add node-sass`下载包（按理说执行`npm install node-sass --save`也应该有用，但是我这边报错了）。

    报错如下：

    ![npm安装sass报错](https://files.catbox.moe/p8buw3.png)

    我的vscode和node版本都是最新的

    建议大家用`yarn`来安装项目

    **发现启动后sass会报错**

    解决办法：sass指定版本为`^1.43.5`，postcss-pxtorem为`^5.1.1`，不要安装node-sass了

    前端生态配置仍然复杂

2. 将样式文件后缀改为`.scss`并在tsx或者js文件中引入，项目会自动编译。

为了实现自适应，给项目配置postcss-pxtorem

1. 执行`yarn add lib-flexible postcss-pxtorem`

2. 在应用入口引入`import 'lib-flexible'`

3. 执行`npm run eject`可以打开`create-react-app`应用的配置文件

4. 在`config/webpack.config.js`文件中配置postcss，位置及方法如下图：

![引入postcss](https://files.catbox.moe/pwa11l.png)

![配置postcss](https://files.catbox.moe/o71hjj.png)

UI库使用antdesign

1. 安装antd，`yarn add antd`

2. 在App.css 中导入样式`@import '~antd/dist/antd.css'`，然后在组件中引入相应的ui组件就可以用了。

---

完~