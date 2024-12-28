---
title: git 操作
date: 2022-04-10 16:02:19
tags: [git, gitee ]
categories: git
top_img: /img/3.jpg
cover: /img/3.jpg
---

git作为程序的代码版本控制库，初用时一些命名略显生疏，普及一下gitee仓库容量500M，github容量1G,单文件最大都是50M

```js
1.git clone ....url
//克隆代码
//本地克隆远程项目
2.git status 
//查看当前分支
3.git add *
    //通配符表示
    //提交新增文件|修改文件
   
4.git commit -m '这是我的第一次提交'
//提交并附带信息
//

5.git push 
//推送到云

6.git branch lanhan 
//创建分支

7.git checkout laohan
//切换分支

8.push之前执行更新操作
//新增文件，修改代码
9.git config --list 
//查看配置

```

具有一个远程仓库（多人访问，多人共同维护一份代码）

```js
//拉取仓库代码到本地
git pull
对应原生的
```

```js
本地仓库
git add 暂存区
//提交到暂存区
git commit -m '' 工作区
git push 到远程
```

合并分支

进入主分支 

```js
1.进入主分支

2.合并主分支
git merge hanmy

//查看所有分支
git branch

```

```js
git branch xxx 新建分支
git checkout xxx 切换 分支

git checkout -b xxx 新建分支并进入新建分支

git merge xxx 当前分支合并分支

git pull 拉取仓库
git push 上传仓库
```

插件 gitlens 



```js
解决 fatal: Not a git repository (or any of the parent directories): .git 问题

然后关联远程或push 又出现了错误，如下

 fatal: Not a git repository (or any of the parent directories): .git 


提示说没有.git这样一个目录

在命令行 输入 git init  然后回车就好了

再重新执行添加文件的命令即可。
原文链接：https://blog.csdn.net/wenb1bai/article/details/89363588
```





index 表示暂存区