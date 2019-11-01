---
title: 每天一道Js基础：Vue
copyright: true
abbrlink: 48aa69b8
date: 2019-10-06 11:14:46
tags: Javascript
categories: Javascript
---
Vue实战
<!--more-->
**安装vue,创建项目**

```
npm install --global vue-cli

vue init webpack vue-demo01  /vue init webpack-simple vuedemo2

cd vue-demo01

npm install /cnpm install 创建项目没报错，省略

npm run dev //运行  vue-cli2.0    vue-cli3.0  => npm run serve 官网: https://cli.vuejs.org/
```
**vue ui 图形化创建项目**

开发看API即可。

### VueCLI
vue init webpack 名字  初始化项目



### eslint
   代码检测

runtimecompiler 和runtimeonly

* runtime-compiler(v1)
  template->abstract syntax tree->render(function)->virtual dom->UI

* runtime-only(v2)(性能更高，代码量更少)
render-> vdom-> UI

template是由vue-template-compiler编译 开发时依赖

### Vue-router



### vue程序运行过程
  template->abstract syntax tree->render(function)->virtual dom->UI
                抽象语法树                              
    