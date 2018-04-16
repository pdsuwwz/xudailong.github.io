---
layout: post
title:  "如何使用 git 版本控制工具"
date:   2018-04-14 14:45:43
categories: github
tags:  git
author: 王文章
---

* content
{:toc}

## 任务

使用 git 来实现博客的版本控制功能。

## 过程

到官网下载 git ，可能网速有点慢，在这里提供网盘下载链接：

[git 2.15.0-64](https://pan.baidu.com/s/12GTlD5Sd3rmDx_PSQb-7-w)

密码: *n32v*

下载之后，常规的傻瓜式安装，安装过程在此就不再此赘述了，如有需要请百度，不在天朝请 google。

**一 .Git初始化及仓库创建和操作**

1. Git安装之后需要进行的一些基本信息配置

  （1）设置用户名：

```js

  git config --global user.name "你的github用户名";

```

  （2）设置用户邮箱：

```js

  git config --global user.email "你的github的默认邮箱";

```




  （3）配置成功后，我们用如下命令来看看是否配置成功，若里面的user.name和user.email显示的是你刚才输入的信息，那么就代表配置成功。

```js

git config --list

```

  注意：git config --global 参数，有了这个参数表示你这台机器上所有的git仓库都会使用这个配置，当然你也可以对某个仓库指定不同的用户名和邮箱

在配置的时候可能会报出以下异常信息，

```js

Can’t finish GitHub sharing process

```

这个时候用命令的方式配置用户信息就不太管用了，我们可以这样做。找到项目的文件夹，里面有个隐藏文件夹： `.git` ，打开之后找到 `config` 文件，用记事本打开，添加如下代码。

```js

[user]
    name = 你的github用户名
    email = 你的github的默认邮箱

```

这样子输入 `git config --list` 就会正确显示你的账户信息了。





