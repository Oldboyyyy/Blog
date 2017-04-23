---
title: nginx
date: 2017-04-23 20:51:33
tags: [Nginx]
categories: [笔记,Nginx]
---
这是豆瓣电影top250的api：`https://api.douban.com/v2/movie/top250`
直接在浏览器访问是这样的
![说明接口是没问题的](http://upload-images.jianshu.io/upload_images/4760143-209c9a2a4d156a84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
直接用ajax去请求接口看看
![js代码](http://upload-images.jianshu.io/upload_images/4760143-aeffc563a16b7cdb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
<!--more-->
结果如下
![不同域，存在跨域问题](http://upload-images.jianshu.io/upload_images/4760143-c8917e3cb3f1230b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
结果出错了，因为存在着跨域问题，跨域是不能直接调接口的，需要进行处理。
这里用的是nginx做的反向代理来实现的跨域
1.首先下载并安装的`nginx`服务器,下载地址：`http://nginx.org/en/download.html`,傻瓜式下一步下一步安装。
2.找到自己安装的目录位置，我是安装到d盘的`D:\nginx\nginx-1.12.0`,我的版本是`nginx-1.12.0`
![安装好了，目录文件如图！](http://upload-images.jianshu.io/upload_images/4760143-7dc8e2c3a6e36022.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.可以直接双击`nginx.exe`文件，也可以通过命令行工具启动，启动命令是`start nginx`
![可以在`nginx`根目录下，按住`shift键 `+鼠标右键，找到`在此处打开命令窗口`](http://upload-images.jianshu.io/upload_images/4760143-be270a1b71863323.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4.在浏览器里面输入`localhost`回车，出现如下页面即为成功安装并启动了`nginx`
![启动成功页面](http://upload-images.jianshu.io/upload_images/4760143-b3e24bd40f6e3d70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
5.现在打开配置文件，设置反向代理，配置文件在`D:\nginx\nginx-1.12.0\conf\nginx.conf`
![nginx.conf文件部分截图](http://upload-images.jianshu.io/upload_images/4760143-8ef362c444a52c9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
6.反向代理配置如下
![配置完毕](http://upload-images.jianshu.io/upload_images/4760143-f51283417bd69114.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
7.保存一下文件，在命令行里面输入`nginx -s reload`，重启下服务
![重启服务](http://upload-images.jianshu.io/upload_images/4760143-79228c0a2f6fb342.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
8.修改下求情地址，再试一下
![更改接口后的js代码](http://upload-images.jianshu.io/upload_images/4760143-12a3d12b29d8f7e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![已经可以正确返回结果了](http://upload-images.jianshu.io/upload_images/4760143-29ed29fd2c4169ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
9.我们还可以将刚刚在`nginx.conf`里面写的东西，放到另一个文件里面，我们只需要在使用的时候，将其引入就好了。`nginx.conf`里面的代码如下：
![nginx.conf配置](http://upload-images.jianshu.io/upload_images/4760143-dad019c9146b1e11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我是在`nginx-1.12.0\conf\douban\douban.conf`下新建的
![新建配置文件目录位置](http://upload-images.jianshu.io/upload_images/4760143-2d4d80cc9a44d2ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
`douban.conf`内容如下：

![就是之前的内容复制过去](http://upload-images.jianshu.io/upload_images/4760143-1ef41116b6ac82b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)