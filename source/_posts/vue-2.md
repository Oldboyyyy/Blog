---
title: 模板语法
date: 2017-03-22 23:12:35
tags: [Vue]
categories: [笔记,Vue]
---
`> Vue.js 使用了基于 HTML 的模版语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML ，所以能被遵循规范的浏览器和 HTML 解析器解析。

> 在底层的实现上， Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，在应用状态改变时， Vue 能够智能地计算出重新渲染组件的最小代价并应用到 DOM 操作上。

##插值
<!--more-->
### #文本
数据绑定最常见的形式就是使用‘Mustache’语法（双大括号）的文本插值
```
<span>message:{{message}}</span> //最简单的插值应用
```
`Mustache`会代替对应数据对象上的`message`属性多对应的值。无论何时只要`message`的值改变了，插值的内容也会跟着更新。
```
<span v-once>{{once}}</span>
```
`once`指令，只会在第一次渲染的时候有效，后面`once`的值变化都不会影响插值的更新

### #纯HTML
双大括号只会讲数据解析成纯文本，而非HTML。想要输出正在的HTML，要用`v-html`指令
```
<div v-html="html"></div>
```
被插入的值都会被当做HTML，数据绑定将会失效。
***注意：站点上动态渲染html是非常危险的，因为很容易被XSS攻击，请给可信任内容提供插值，切勿为用户提供内容插值***

### #属性
Mustache不能在HTML属性中去使用，应该使用v-bind。
```
<span v-bind:class="red">hello Vue!</span> //添加class属性
```
这对布尔值也有效
```
<button v-bind:disabled='result'>Button</button>//如果result等于false，disabled属性就会被移除
```

### #使用javascript表达式
在模板中可以使用js表达式
```
{{number + 1}}
{{ok ? 'yes' : 'no'}}
{{msg.split(',').reverse().join(',')}}
<p v-bind:id="'item' + id"></p>
```
以下几种情况不会成功，因为他们都不是表达式
```
{{var a = 1}}//这是语句
{{if(ok){code}}};//流程控制语句，可用三目运算代替
```
***模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量***

##指令
指令（Directive）是带有V-前缀的特殊属性。

### #参数
一些指令可以接受一个参数，在指令后一冒号指明
```
<p v-bind:id='id'>1111</p>//用相应的更新HTML属性
<p :id='id'></p>//简写
<p v-on:click='fn'>1111</p>//监听dom事件
<p @click='fn'>1111</p>//简写
```

### #修饰符
修饰符（Modifiers）是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定
```
<form v-on:submit.prevent="onSubmit"></form>
```

##过滤器

Vue.js 允许你自定义过滤器，可被用作一些常见的文本格式化。过滤器可以用在两个地方：mustache 插值和 v-bind 表达式
2.0废弃了了1.0的原生过滤器

```
{{ message | capitalize }}
new Vue({
  // ...
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
```
过滤器可以串联
```
{{ message | filterA | filterB }}
```
过滤器是 JavaScript 函数，因此可以接受参数：
```
{{ message | filterA('arg1', arg2) }}
```

##官方中文API
[Vue](https://cn.vuejs.org/v2/guide/syntax.html#修饰符)