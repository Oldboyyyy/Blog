---
title: 计算属性 computed
date: 2017-03-26 18:16:28
tags: [Vue]
categories: [笔记,Vue]
---
> 模板表达式是非常的便利，也可以进行简单的运算，但是面对较为复杂的运算，就会让模板变得那已维护。面对相对复杂的运算应该选用计算属性来处理

### 简单的例子
```
<div id="app">
    <p>{{msg}}</p>
    <p>computed:{{reverseMsg}}</p>
</div>

<script>
var vm = new Vue({
       el: '#app',
       data: {
           msg: 'hello Vue!'
       },
       computed: {
           reverseMsg: function(){
                return this.msg.split(' ').reverse().join(' ');
           }
       }
   });
</script>
```
<!--more-->
结果是：
![结果](http://upload-images.jianshu.io/upload_images/4760143-43fd328d84eaea99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们先声明一个计算属性`reverseMsg`。我们提供的函数将作为`vm.reverseMsg`的`getter`；
`vm.reverseMsg`的值是取决于`vm.message`的值。
`Vue`知道`vm.reverseMsg`的值是依赖于`vm.msg`，因此当`vm.msg`发生改变时，所有依赖于`vm.reverseMsg`的绑定也会更新。

### 计算缓存 VS Methods
我们也可以通过利用`Methods`来达到同样的效果
```
<p>methods:{{reverseMsgfn()}}</p>

... //其他的代码省略
methods: {
           reverseMsgfn: function(){
               return this.msg.split(' ').reverse().join(' ');
           }
       }
```
结果：
![Methods的结果](http://upload-images.jianshu.io/upload_images/4760143-91bcb9fd29d45fa7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
最终的结果都是一样的，不同的是`计算属性`是基于他们的依赖进行缓存的;`计算属性`只有在他的相关依赖发生改变时才会重新求值；这就意味着，在`msg`值没有发生改变，`reverseMsg`的值就不会发生改变，这期间多次访问`reverseMsg`属性都是之前的的计算结果，而是不值再次进行执行函数;如果我们不需要缓存的话，我们就可以用`method`代替。

### 计算属性 VS Watch属性
`Vue`提供了一种方式来观察和响应Vue实例上的数据变动：`watch`属性
```
<div id="app">
     <label for="">firstName:<input type="text" v-model='firstName'></label>
     <label for="">lastName:<input type="text" v-model='lastName'></label>
     <p>fullName:{{fullName}}</p>
</div>

...//其他代码省略
watch: {
       firstName: function(val){
             this.fullName = val + ' ' + this.lastName;
       },
       lastName: function(val){
             this.fullName = this.firstName + ' ' + val;
       }
 }
```
下面使用`computed`实现的
```
<div id="app">
     <label for="">firstName:<input type="text" v-model='firstName'></label>
     <label for="">lastName:<input type="text" v-model='lastName'></label>
     <p>fullName:{{fullName}}</p>
</div>

...//其他代码省略
computed: {
        fullName : {
                get: function(){
                    return this.firstName +' '+ this.lastName;
                 },
               set: function(newVal){
                    var names = newVal.split(' ');
                    this.firstName =  names[0];
                    this.lastName = names[names.length - 1];
                } 
        }
```
![watch和computed结果](http://upload-images.jianshu.io/upload_images/4760143-ce338fd65a9a3ac7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
`computed`也可以实现相同的效果，而且代码更加简洁易懂
计算属性，一般默认只有getter，不过我们也可以提供一个setter,我们就可以通改变`fullName`值，也可以动态改变`firstName`和`lastName`。

### 观察者 Watchers
虽然计算属性在大多数情况下是可以替代`watch`的，但是在执行异步操作或者更大开销操作的时候`watch`更适合;

`watch`选项允许我们执行异步操作，限制我们操作的频率并在得到结果前设置中间状态，这是计算属性无法做到的。
```
<div id="app3">
    你的问题是：<input type="text" v-model="question">
    答案是：<p v-text="answer"></p>
</div>

<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>
<script>
    new Vue({
        el: '#app3',
        data: {
            question: '',
            answer: '你不提问我没法回答'
        },
        watch: {
            question: function(){
                this.answer = '问题通常包含一个问号';
                this.getAnswer();
            }
        },
        methods: {
            getAnswer: _.debounce(function () {
                var _this = this;
                if(_this.question.indexOf('?') === -1){
                    return _this.answer = '问题通常包含一个问号';
                }
                _this.answer = '搜索中...';
                axios.get('https://yesno.wtf/api').then(function(response){
                    _this.answer = _.capitalize(response.data.answer);
                }).catch(function(err){
                    _this.answer = 'err'
                })
            },500)
        }

    });
    
</script>
```

![实例结果](http://upload-images.jianshu.io/upload_images/4760143-8ad85a5a26640e05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 官方API
[Vue](https://cn.vuejs.org/v2/guide/computed.html#计算缓存-vs-Methods)