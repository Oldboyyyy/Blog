---
title: Vue实例
date: 2017-03-21 22:53:16
tags: [Vue]
categories: [笔记,Vue]
---
**之前学过Vue的官方文档，因为项目中没有用过，很快就忘记得差不多了，所以这次决定重新学习一下，并且记录下自己的学习过程以及自己的想法**

### 构造器

每一个Vue.js应用都是通过构造函数Vue创建的一个Vue的根实例启动的。
```
  var data = {a: 1};
  var vm = new Vue({
      el: '#app', //挂载对象
      data: data //代理的数据
  })
```
<!--more-->
### 属性与方法

每一个Vue实例都会代理其`data`对象里面的所以属性，注意这时代理的`data`的属性是响应的。如果实例创建以后，添加新的属性到实例上，它不会触发视图更新（后面会详细讨论）。
```
    var data = {a: 1};
    var vm = new Vue({
        el: '#app',
        data: data
    });
    vm.a === data.a;//true
    //修改vm的a的值会影响到原始值
    vm.a = 2;
    console.log(data.a); //=> 2
    //反之亦然
    data.a = 3;
    console.log(vm.a);// => 3
```
除了data属性，Vue还暴露了一些有用的实例属性与方法。这些方法与属性都是以$开头，以便于代理的data属性区分
```
    console.log(vm.$data === data); //true
    console.log(vm.$el === document.getElementById('app')); //true

    vm.$watch('a', function(newVal, oldVal){
        console.log('a改变了，新值是'+ newVal +'，旧值是'+ oldVal);
    });
    vm.a = 'a'; //a的值变了，就会触发上面的回调函数
    //a改变了，新值是a，旧值是3
```

### 实例的生命周期
> 每一个Vue实例在被创建的之前都要经过一些列的初始化的过程;
例如实例需要配备数据观测，模板编译，挂载实例到DOM，然后数据变化到Dom,在这过程中，会调用一系列的生命周期的钩子函数，这就提供了给我们执行自定义逻辑的机会.

### 生命周期图示
盗用官方文档的图：
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4760143-c1d266fca1e6135d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### API地址
[Vue 中文API](https://cn.vuejs.org/v2/guide/instance.html)
