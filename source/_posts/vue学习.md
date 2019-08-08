---
title: vue学习
categories: vue
tags: vue
abbrlink: b38a0bed
date: 2019-07-13 16:49:12
---
vue
<!--more-->
Vue.js（读音 /vjuː/, 类似于 view） 是一套构建用户界面的 渐进式框架。与其他重量级框架不同的是Vue 的核心库只关注视图层。
Vue.js 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件。
Vue.js是一种MVVM框架，其中html是view层，js是model层，通过vue.js（使用v-model这个指令）完成中间的底层逻辑，实现绑定的效果。改变其中的任何一层，另外一层都会改变；

vue中绑定的data中单双引号用起来无差别，但是，通常习惯用单引号，同时，单引号貌似效率高。。。。

Vue.js就是一个用于搭建类似于网页版知乎这种表单项繁多，且内容需要根据用户的操作进行修改的网页版应用。
查看api文档：
# el
类型： string |Element
限制：只在由new创建的实例中遵守
详细：
1.提供一个在页面上已存在的 DOM 元素作为 Vue 实例的挂载目标。可以是 CSS 选择器，也可以是一个 HTMLElement 实例。 
2.在实例挂载之后，元素可以用 vm.$el 访问。  如果在实例化时存在这个选项，实例将立即进入编译过程，否则，需要显式调用 vm.$mount() 手动开启编译。  3.提供的元素只能作为挂载点。不同于 Vue 1.x，所有的挂载元素会被 Vue 生成的 DOM 替换。因此不推荐挂载 root 实例到 <html> 或者 <body> 上。  
4.如果 render 函数和 template 属性都不存在，挂载 DOM 元素的 HTML 会被提取出来用作模板，此时，必须使用 Runtime + Compiler 构建的 Vue 库。
__重点：如果存在 render 函数或 template 属性，则挂载元素会被 Vue 生成的 DOM 替换；否则，挂载元素所在的 HTML 会被提取出来用作模版__

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <div id="ppp"></div>
  </body>
</html>

```
例一：render函数渲染的DOM替换<div id="ppp"></div>
>DOM:DocumentObjectModel 文件对象模型，标准编程接口，处理可扩展标志语言
```
new Vue({
  el: '#ppp',
  router,
  store, 
  render: h => h(App)
})

```
例二：字符串模板替换<div id="ppp"></div>
```
new Vue({
  el: '#ppp',
  router,
  components: { App },
  template: '<App/>'
})

```

例三：手动挂载且不存在render函数和template属性
```
new Vue({
  router,
  store,
}).$mount('#ppp')

```

### v-show 与 v-if 区别
  v-show和v-if的区别： 
 1. v-show是css切换，v-if是完整的销毁和重新创建。
 2. 使用 频繁切换时用v-show，运行时较少改变时用v-if
 3. v-if=‘false’ v-if是条件渲染，当false的时候不会渲染

### 绑定 class 的数组用法
1. 对象方法 v-bind:class="{'orange': isRipe, 'green': isNotRipe}"
2. 数组方法 v-bind:class="[class1, class2]"
3. 行内 v-bind:style="{color: color, fontSize: fontSize+'px' }"

### 计算属性和 watch 的区别

计算属性是自动监听依赖值的变化，从而动态返回内容，监听是一个过程，在监听的值变化时，可以触发一个回调，并做一些事情。 所以区别来源于用法，只是需要动态值，那就用计算属性；需要知道值的改变后执行业务逻辑，才用 watch，用反或混用虽然可行，但都是不正确的用法。

#### 说出一下区别会加分

computed 是一个对象时，它有哪些选项？

computed 和 methods 有什么区别？

computed 是否能依赖其它组件的数据？

watch 是一个对象时，它有哪些选项？

1. 有get和set两个选项

2. methods是一个方法，它可以接受参数，而computed不能，computed是可以缓存的，methods不会。

3. computed可以依赖其他computed，甚至是其他组件的data

4. watch 配置 handler deep 是否深度 immeditate 是否立即执行

##### 总结

当有一些数据需要随着另外一些数据变化时，建议使用computed。 当有一个通用的响应数据变化的时候，要执行一些业务逻辑或异步操作的时候建议使用watcher

#### 事件修饰符

1. 绑定一个原生的click事件， 加native，
2. 其他事件修饰符
stop prevent self
3. 组合键 click.ctrl.exact 只有ctrl被按下的时候才触发

#### 组件中 data 为什么是函数

> 为什么组件中的 data 必须是一个函数，然后 return 一个对象，而 new Vue 实例里，data 可以直接是一个对象？

__因为组件是用来复用的，JS 里对象是引用关系，这样作用域没有隔离，而 new Vue 的实例，是不会被复用的，因此不存在引用对象的问题。__

Vue的模式是m-v-vm模式，即（model-view-modelView），通过modelView作为中间层（即vm的实例），进行双向数据的绑定与变化。

而实现这种双向绑定的关键就在于：

Object.defineProperty  和    订阅——发布者模式。

#### 理解Vue中的Render渲染函数

VUE一般使用template来创建HTML，然后在有的时候，我们需要使用javascript来创建html，这时候我们需要使用render函数。
render函数return一个createElement组件中的子元素存储在组件实例中 $slots.default 中。
return createElement('h1', this.title); 
createElement返回的是包含的信息会告诉VUE页面上需要渲染什么样的节点及其子节点。我们称这样的节点为虚拟DOM，可以简写为VNode。
##### createElement 参数
```
{String | Object | Function}
```
一个HTML标签字符串，组件选项对象，或者一个返回值类型为String/Object的函数。该参数是必须的。
