---
title: webpack起步
date: 2017-03-18 20:46:51
tags: [webpack]
categories: [笔记]
---

### webpack介绍
> Webpack 是当下最热门的前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader 的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等。

### 安装
<!--more-->
##### 本地安装

```
npm install webpack --save-dev
npm install webpack@<version> --save-dev //安装指定版本的webpack
```

#### 全局安装

```
npm install webpack -g
```

### 小试牛刀
webpack打包javascript模块

1.新建一个文件夹，进入文件夹右键打开`git bush`
```
npm init -y // npm初始化文件会自动建一个 package.json文件
npm install --save-dev webpack //本地安装webpack
./node_modules/.bin/webpack --help // windows下这个查看有没有安装好webpack，中文文档这里错了
```

2.在文件夹中新建一个`app`文件夹，并新建一个`index.js`
```
function component () {
  var element = document.createElement('div');

  /* 需要引入 lodash，下一行才能正常工作 */
  element.innerHTML = _.join(['Hello','webpack'], ' ');

  return element;
}

document.body.appendChild(component());
```

3.在根目录下建一个`index.html`
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>webpack2</title>
</head>
<body>
    <--!这里引用的webpack打包以后的js文件-->
    <script src="dist/bundle.js"></script>

</body>
</html>
```

4.由于`index.js`里面引用了`lodash.js`所以本地必须安装`lodash.js`
```
npm install -save lodash
```

5.关键的一部，开始主角上场了
```
./node_modules/.bin/webpack app/index.js dist/bundle.js
webpack路径 要打包的文件 打包以后的路劲
```

![U`$PNW_R2KUC3`53_(XUS~8.png](http://upload-images.jianshu.io/upload_images/4760143-95769e86f298a9fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这样就打包成功了，我们会发现项目根目录下多了一个`dist`文件夹，里面有个`bundle.js`文件，这个就是我们打包后的文件
*webpack除了会修改`import/export`以外不会修改你的代码，如果想使用es6语法需要使用`Babel`来编译*

6.除了命令行的方式来完成打包，我们还可以通过配置文件的方式来实现，现在项目根目录新建一个`webpack.config.js`文件
```
var path = require('path');

module.exports = {
  entry: './app/index.js', //入口文件
  output: {
    filename: 'bundle.js', //完成打包以后的文件名称
    path: path.resolve(__dirname, 'dist') // 打包文件的目录位置
  }
};
```
这时候执行webpack命令
```
webpack --config webpack.config.js
```

![380)_C9K2$Z6BIMMI~MHX}I.png](http://upload-images.jianshu.io/upload_images/4760143-aa66766c44b544b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
配置文件可以是我们灵活的是用webpack。使用配置文件，我们可以对bundle添加加载器规则、插件、解析 选项，以及许多增强功能。

7.这种方式还是不是很方便，我们可以设置还可以在设置快捷方式，可以再`package.json`里面配置
```
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack" //配置在这里
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^2.2.1"
  },
  "dependencies": {
    "lodash": "^4.17.4"
  }
}
```
设置好了之后，通过`npm run build` 就可以启动了

8.文件目录如下


![}NQ31WDS$Y_W}{7@7L4JD3H.png](http://upload-images.jianshu.io/upload_images/4760143-be44b9a60adc5b11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


9.这就是webpack最基本的用法

###参考文档
[webpack2](http://www.css88.com/doc/webpack2/)