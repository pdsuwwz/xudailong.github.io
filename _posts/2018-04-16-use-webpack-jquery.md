---
layout: post
title:  "webpack加载器 jQuery的打包"
date:   2018-04-16 08:35:21
categories: webpack
tags:  打包 webpack
author: 王文章
---

* content
{:toc}

![webpack](https://i.loli.net/2018/04/21/5ada9267452a4.jpg)

在上一节中，我们知道了如何将图片资源打包到 js 文件中去，就是用到了 file-loader 加载器这个东西。

本节将展示如何将自己写过的 jQuery 方法打包到 js 文件中去。这次呢，为了复习一遍前面学过的东西，我们将重新新建一个项目。





## 过程

1、现在我们新建一个文件夹，命名：webpack-test
2、执行以下命令，一路回车，目的是生成一个 package.json 文件，并里面配置快速使用 webpack 命令。由于后面会讲到热加载，热加载需要用到 npm start 命令，所以将原来的打包命令换成 npm run build 。

```js

npm init

```

```json

"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
}

```

3、我们来安装 webpack 模块 ，执行以下命令，等待安装完毕。

```js

npm install webpck@2.3.2 --save-dev

```
## 安装jQuery模块
4、既然我们要打包 jQuery ，那么我们就得安装 jQuery 模块，还得有一个 expose-loader 模块，用法见官网 API，执行以下命令，注：本例中用到的 jquery 版本为 3.3.1 。[expose-loader](https://webpack.js.org/loaders/expose-loader/)

```
npm install jquery --save-dev 

```

```
npm install expose-loader --save-dev 

```

5、我们来新建一个 src 文件夹，用于存放 js 代码，在其中新建一个 test_jq.js 文件，为了便于测试，里面写如下方法。

```js

$(function(){
    $(".btn").on("click",function(){
        alert("成功将 jQuery 打包！");
    });
});

```

6、在根目录新建一个入口文件 main.js，写入如下代码。

```js

require("jquery");
require("./src/test_jq.js");

```


7、OK,还差一个配置文件对吧，我们来新建一个 webpack.config.js ，配置如下。

```js

var path = require("path");

module.exports = {
    entry: {
        main: "./main.js"
    },
    output: {
        path: path.resolve(__dirname, "dist"),
        filename: "[name].bundle.js"
	},
	module: {
    	rules: [{
            test: require.resolve("jquery"),
            use: [{
                loader: "expose-loader",
                options: "$"
            }]
        }]
    }
};


```

8、在根目录新建一个 index.html 文件 写入以下代码。

```html

<!DOCTYPE html>
<html lang="zh-Hans">
<head>
	<meta charset="UTF-8">
	<title> webpack 打包 jQuery </title>
</head>
<body>
	<button class="btn">点我测试 webpack 打包 jQuery 是否成功。</button>
	<script type="text/javascript" src="./dist/main.bundle.js"></script>
</body>
</html>

```

9、最后使用打包命令 npm run build ，打包完成后用浏览器打开此 html 文件，点击按钮测试下，弹出提示信息说明打包 jQuery 成功。

## 目录结构
此时的*目录文件结构*如下：

> - src
>     - test.jq.js
> - dist
>     - main.module.js
> - node_modules
> - index.html
> - main.js
> - package.json
> - webpack.config.js






















