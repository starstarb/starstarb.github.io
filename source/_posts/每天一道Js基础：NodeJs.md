---
title: 每天一道Js基础：NodeJs
copyright: true
abbrlink: 46df0a98
date: 2019-10-06 11:15:14
tags: Javascript
categories: Javascript
---
Node.js
<!--more-->
### Node特点
1. 异步IO
2. 事件和回调函数
3. 单线程
4. 跨平台兼容
###  CommonJS的模块规范
1. 模块引用
var math=require('math');
  require方法接受模块标识，以此引入一个模块的API到当前上下文。
2. 模块定义
exports对象用户导出当前模块的方法或变量，并且它是唯一导出的出口。

在Node中，一个文件就是一个模块，将方法挂载在exports对象上作为属性即可定义导出的方式。
3. 模块标识
    传递给require()方法的参数，它必须是符合小驼峰命名的字符串，或者以.、..开头的相对路径，或者绝对路径，它可以没有文件名后缀.js。
