---
title: 条件渲染与列表渲染
date: 2017-03-30 23:18:03
tags: [Vue]
categories: [笔记,Vue]
---
## 条件渲染
> 条件渲染，就满足一定的条件以后才会渲染。

### v-if
`v-if `指令类似于，`js`中的`if`语句，当条件满足时才会执行
```
<span v-if="ok">v-if</span> //ok的值为true，span标签才会被渲染
<template v-if="ok"> //同时渲染多个元素
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
var vm = new Vue({
       el: '#app',
       data: {
           ok: true,
           no: true,
           type: 'c',
           toggle: true
       }
   });
```
<!--more-->
### v-else
`v-else`指令，类似于`js`中的`else`语句，当`v-if`条件不成立是，`v-else`就会渲染。
```
<span v-if="ok">v-if</span>
<span v-else>v-else</span> //当ok 的值为false，是渲染
```
*`v-else`必须紧跟着在`v-if`或者`v-else-if`的到后面，否则不会被识别。*
### v-else-if
是`2.1.0`新增加的指令，类似于`js`中的`else if`，可以链式使用多次。
```
<p v-if="type === 'a'">a</p>
<p v-else-if="type === 'b'">b</p>
<p v-else="">not a and b</p>
```
*`v-else-if`必须紧跟着在`v-if`或者`v-else-if`的到后面，否则不会被识别。*
### 用key管理可复用的元素
`vue`会尽可能的高效的渲染元素，通常会复用已有元素而不会从头渲染
```
 <p v-if="toggle"><label>username </label> <input type="text" placeholder="username"></p>
 <p v-else=""><label>email</label> <input type="text" placeholder="email"></p>
 <button @click="toggle = !toggle">toggle</button>
```
上面例子因为两个`p标签`用用了相同的元素，`<input>`不会被替换掉，仅仅是替换了他的`placeholder`。

![username显示时，输入框里面输入的1](http://upload-images.jianshu.io/upload_images/4760143-9fb69e6edec0f69e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![切换到，email是1任然存在，说明input是复用之前的input](http://upload-images.jianshu.io/upload_images/4760143-f7fd73485e26452c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 当我们不想复用他们时，只要加上唯一的key属性

```
<p v-if="toggle"><label>username </label> <input type="text" placeholder="username" key="username"></p>
<p v-else=""><label>email</label> <input type="text" placeholder="email" key="email"></p>
<button @click="toggle = !toggle">toggle</button>
```

![加上可以以后，在username上输入了1](http://upload-images.jianshu.io/upload_images/4760143-b5aa38956e8f58a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![切换到email下，1不见了，说明两个input不是同一个，没有复用之前的了](http://upload-images.jianshu.io/upload_images/4760143-1675e9c1cf0a0cee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*注意, <label> 元素仍然会被高效地复用，因为它们没有添加 key 属性。*

### v-show

v-show与v-if的用法几乎一致
```
<span v-show="ok">v-show</span>
```
### v-show VS v-if
- `v-show`不支持 `<template>` 语法，也不支持` v-else`。
- `v-if` 是真正的条件渲染，因为他确保在切换过程中条件块内部的事件监听器和子组件适当的被销毁和重建
- `v-if`也是惰性的，如果在处事渲染时条件为假，则什么也不做，直到条件为真时，才开始渲染条件块
- `v-show `就简单的多，不管条件是啥总会被渲染，并且只是简单的基于`css`的切换
- 一般需要频繁的切换就是用`v-show`，运行条件不太会改变则使用`v-if`
- 当 `v-if `与 `v-for` 一起使用时，`v-for` 具有比 `v-if` 更高的优先级


## 列表渲染
### v-for
我们用 `v-for` 指令根据一组数组的选项列表进行渲染。基本用法如下：
```
<div id="app">
    <ul>
         <li v-for="item in items" v-text="item.text"></li>
    </ul>
</div>
var vm = new Vue({
        el: '#app',
        data: {
           items: [
               {text: 'item1'},
               {text: 'item2'},
               {text: 'item3'}
           ]
        }
});
```

![基本用法示例](http://upload-images.jianshu.io/upload_images/4760143-b2d53a0d6c5c137d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*v-for 还支持可选的第二个参数为当前项的索引*
```
<ul>
      <li v-for="(val,index) in items" v-text="(index+1) + '. ' + val.text"></li>
</ul>
```

![带了索引的示例](http://upload-images.jianshu.io/upload_images/4760143-8cea72e028339da5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*利用template标签同时渲染多个标签*
```
<ul>
      <template v-for="(val, index) in items">
          <li>{{index}}</li>
          <li>{{val.text}}</li>
       </template>
 </ul>
```

![同时渲染两个li的示例](http://upload-images.jianshu.io/upload_images/4760143-96efcac485438d58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*对象的迭代，是按 `Object.keys()` 的结果遍历，但是不能保证它的结果在不同的 `JavaScript` 引擎下是一致的*
```
<span v-for="key in obj">{{key}}</span>
<p v-for="(key, value) in obj">{{key}}: {{value}}</p>
<p v-for="(key, value, index) in obj">{{key}}: {{value}}: {{index}}</p>
```
![对象迭代示例](http://upload-images.jianshu.io/upload_images/4760143-f416fab25aeaff03.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<button v-for="i in 10">{{i}}</button>
```
![整数迭代示例](http://upload-images.jianshu.io/upload_images/4760143-c2bfff7d3d87e178.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当迭代渲染遇上组件
```
<div id="#app">
    <my-ul :items="items"></my-ul> //将数据注入子组件
</div>
<template id="myul">
    <ul>
        <li v-for="i in items">{{i.text}}</li>
    </ul>
</template>
<script>
    Vue.component('my-ul',{
        template: '#myul',
        props: ['items']  //接受父组件传进了的数据
    });
 var vm = new Vue({
        el: '#app',
        data: {
           items: [
               {text: 'item1'},
               {text: 'item2'},
               {text: 'item3'}
           ]
        }
    });
</script>
```
![组件循环示例](http://upload-images.jianshu.io/upload_images/4760143-6b2fb5017571f3e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### key
当 `Vue.js` 用 `v-for` 正在更新已渲染过的元素列表时，它默认用 “就地复用” 策略。
建议尽可能使用` v-for` 来提供 `key` ，除非迭代 `DOM` 内容足够简单，或者你是故意要依赖于默认行为来获得性能提升。用法跟前面一样。

### 数组的更新检查
`Vue `包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下
- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()
这些方法都会改变原数组，也有一些方法是返回一个新数组，不会改变原数组。例如： filter(), concat(), slice() 。当使用非变异方法时，可以用新数组替换旧数组。
***注意事项***
由于 JavaScript 的限制， Vue 不能检测以下变动的数组：
- 当你利用索引直接设置一个项时，例如： `vm.items[indexOfItem] = newValue`
- 当你修改数组的长度时，例如： `vm.items.length = newLength`

解决方法：
```
Vue.set(example1.items, indexOfItem, newValue)
example1.items.splice(indexOfItem, 1, newValue)
example1.items.splice(newLength)
```

### 数据过滤
我们想要显示一个数组的过滤或排序副本，而不实际改变或重置原始数据。
```
 奇数：<span v-for="i in odd">{{i}}</span>
 偶数：<span v-for="i in even(number)">{{i}}</span>

 var vm = new Vue({
        el: '#app',
        data: {                
            number: [1,2,3,4,5,6]
        },
        computed: {
            odd: function(){
                return this.number.filter(function(i){
                    return i%2 === 1;
                })
            }
        },
        methods: {
            even: function(arr){
                return arr.filter(function(i){
                    return i%2 === 0;
                })
            }
        }

    });
```
![过滤示例](http://upload-images.jianshu.io/upload_images/4760143-a9e5e7a9f3e28d7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 官方API
[Vue](https://cn.vuejs.org/v2/guide/list.html#显示过滤-排序结果)
