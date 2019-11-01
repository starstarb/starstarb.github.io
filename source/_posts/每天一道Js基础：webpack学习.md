---
title: 每天一道Js基础：webpack学习
copyright: true
abbrlink: 849c883
date: 2019-10-02 12:41:07
tags: Javascipt
categories: Javascript
---
webpack
<!--more-->
### webpack的基础
At its core, webpack is a static module bundle for modern JavaScript applications.
webpack是一个现代的Javascript应用的静态 **模块化打包工具**。

> 前端模块化解决方案: CommonJS、CMD、AMD、ES6

webpack处理模块间的依赖关系

将各种资源模块进行打包合并成一个或多个包(Bundle).

gulp/grunt强调自动化
webpack强调模块化，依赖于node环境
**安装**
全局
npm install webpack@版本号  -g
局部
npm install  webpack@版本号  --save-dev

dist:存放打包后的文件

