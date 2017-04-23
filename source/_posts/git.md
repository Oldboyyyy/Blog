---
title: Git常用命令记录
date: 2017-03-13 20:43:40
tags: [博客, git] 
categories: [笔记]
---
> 记录常用的git操作命令


### 创建提交版本库
1. 创建版本库
```
git init
```

2. 将文件添加到暂存区里面去
```
git add <文件名或文件夹>
git add -A  //提交所有变化
git add -u //提交被修改的和被删除的文件，不包括新文件
git add . //提交新文件和被修改的文件，不包括被删除的文件
```
<!--more-->
3. 把暂存区的文件提交到仓库
```
git commit -m '这次提交的备注'
```

4. 查看文件是否还有未提交
```
git status
```

5. 查看文件具体修改了哪些内容
```
git diff <要查看的文件名称>  
```

### 版本回退
1. 查看修改文件的修改记录
```
git log //内容信息比较丰富
git log --pretty=oneline //将信息集中到一行，去除了一些信息
```

2.  版本回退操作
```
git reset --hard HEAD^ //回退上一个版本
git reset --hard HEAD^^ //回退上上一个版本，以此类推，如果回退100个版本，就很麻烦
git reset --hard HEAD~100 // 也可以这样来操作
```

3. 通过命令查看文件内容
```
cat <文件名称>
```

4. 查看内容的版本号
```
git reflog
git rest --hard <你想回退的版本号>
```

### 撤销修改和删除文件

1. 未提交之前发现有错误，要恢复以前版本
   - 手动修改错误文件，重新add到暂存区
   - 使用`git reset --hard HEAD^`恢复上一个版本
   - 使用撤销命令操作，如下：
```
git checkout -- <文件名> 
```
 `git checkout`把文件在工作区的修改全部撤销，这里有两种情况，如下：
      1. 文件自动修改以后，还没放到暂存区，使用撤销修改就回到和版本库一模一样的状态。
      2. 另一种，文件修改以后并且也已经放入暂存区，接着又作了修改，撤销修改就回到了添加暂存区后的状态，只是撤销了最近的这次修改，如图示意：
![](http://upload-images.jianshu.io/upload_images/4760143-43418ba7fbca7042.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**注意：命令`git checkout -- aa.txt`中的 `--`很重要，没有的话，那么命令就变成了创建分支**

2. 删除文件
```
rm <你要删除的文件>
```
只要没有commit之前，都可以通过`git checkout -- a.txt` 来恢复

### github的操作

1. 注册github账号
2. 创建SSH KEY （位于户目录 /. ssh / id_rsa和id_rsa.pub这两个文件）没有可以通过以下命令创建
```
ssh-keygen -t rsa -C "你的邮箱地址"
```
3. 将本地仓库推上github
```
git remote add origin <你的github项目地址>
git push -u origin master //将本地仓库的master分支推送到远程仓库
```
第一次推送master分支，要加上`-u`，git不但是把master分支推送上了远程新的master上，还会把本的master分支和远程的master分支关联起来，以后推送拉去就可以简化命令

4. 从远程库克隆
```
git clone <要克隆项目的地址>
```

5. 查看远程库的信息
```
git remote //查看远程库信息
git remote -v  //详细信息
```

6. 把本地分支内容推送至远程库
```
git push origin <分支名称>
```

7. 创建远程的分支到本地来
```
git checkout -b dev origin/dev
```

8. 设置本地的dev分支与远程的origin/dev分支的链接
```
git branch --set-upstream dev origin/dev
```

### 创建与合并分支

1. 创建分支
```
git branch dev //创建dev分支
```

2. 查看分支
```
git branch  //会列出所有的分支，当前分支前面会家一颗星
```

3. 切换分支
```
git checkout dev //切换到dev分支
```

4. 创建分支并且切换分支
```
git checkout -b dev //创建并切换分支
```
相当于这两步操作`git branch dev``git checkout dev`

5. 合并分支
```
git merge dev //在非dev分支上合并dev分支
```

6. 删除分支
```
git branch -d dev
```

7. 解绝冲突问题

![1.png](http://upload-images.jianshu.io/upload_images/4760143-e205edb3e37fd393.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![2.png](http://upload-images.jianshu.io/upload_images/4760143-e5ec6f67acf569a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![3.png](http://upload-images.jianshu.io/upload_images/4760143-7dcc7349aa347772.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![4.png](http://upload-images.jianshu.io/upload_images/4760143-8133392071f7c92e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

8.分支管理策略
> 首先master主分支应该是非常稳定的，也就是用来发布新版本，一般情况下不允许在上面干活，干活一般情况下在新建的dev分支上干活，干完后，比如上要发布，或者说dev分支代码稳定后可以合并到主分支master上来。

### BUG分支

![01.png](http://upload-images.jianshu.io/upload_images/4760143-763a2b7b5ebbb05c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![02.png](http://upload-images.jianshu.io/upload_images/4760143-33297d1356862f42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![03.png](http://upload-images.jianshu.io/upload_images/4760143-f0a7e545ad1ede8c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![04.png](http://upload-images.jianshu.io/upload_images/4760143-b571b420f11deee4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![05.png](http://upload-images.jianshu.io/upload_images/4760143-a4dcf05afcd9bcd3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### Git其他命令

1. 创建空目录

```
mkdir <目录名>
```

2. 显示当前路径

```
pwd
````