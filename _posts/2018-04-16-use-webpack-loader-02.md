---
layout: post
title:  "webpack加载器loader的使用（下）"
date:   2018-04-16 08:20:50
categories: webpack
tags:  打包 webpack
author: 王文章
---

* content
{:toc}

![webpack](https://i.loli.net/2018/04/21/5ada9267452a4.jpg)

在上一节的使用了 css-loader 和 style-loader 加载器来打包 css 文件并以嵌入式的方式加载到 html 文件中，两个 loader 加载器的安装命令如下。

```js

npm install css-loader --save-dev

```

```js

npm install style-loader --save-dev

```





## 过程


本节为 webpack 加载器 loader 的使用（下） 旨在展示如何将图片资源打包到js里并能够展示到html页面中。OK，接着上一节的内容，另复制一份 first-webpack 文件夹。防止与上一节代码混淆。

此时的*目录文件结构*如下：


> - app
>   - css
>     - mystyle.css
>   - images
>     - background-image1.jpg（4kb）
>     - background-image2.jpg（10mb）
>   - js
>     - index.js
> - build
>     - main.module.js
> - node_modules
> - index.html
> - main.js
> - package.json
> - webpack.config.js

1、现在我们试图给 index.html 页面一个背景图片，那么我们就要修改 css 文件

```css

*{
    margin: 0;
    padding: 0;
}

body{
    background-image: url("../images/background-image2.jpg");
    background-repeat: no-repeat;
    background-position: center;
    background-attachment: fixed;
    background-size: cover
}

.btn{
    width: 100px;
    height: 50px;
    background-color: #dd3915;
    border: 1px #dd3915 solid;
    text-align: center;
}

```

2、接着上一节的内容，此时的 *webpack.config.js* 配置文件如下所示

```js

var path = require("path");

module.exports = {
    // 入口文件
    entry: "./main.js",
    // 输出文件
    output: {
        // 输出的路径
        // path:__dirname+ '/build/',
        path: path.resolve(__dirname, "build"),

        // 输出的文件名
        filename: "[name].bundle.js"
    },
	
	// 加载模块
    module: {
    // 加载loader
        loaders: [
            {
                // .css 结尾的文件
                test: /\.css$/,
                // 加载器名称
                loader: "style-loader!css-loader"
            }
        ]
    }
}

```

3、现在我们依旧执行打包命令 *npm start* 发现会报以下错误信息，大致意思跟上小节的错误很相似，提示：模块解析失败，你可能需要一个合适的加载器来处理这种文件类型。

```js

ERROR in ./app/images/background-image2.jpg
Module parse failed: C:\***\first-webpack\app\images\background-image2
.jpg Unexpected character 'a (1:0)
You may need an appropriate loader to handle this file type.
(Source code omitted for this binary file)

```

4、既然我们知道错误，那就可以对症下药了。我们通过查找官网 API 发现有一个 file-loader 加载器，顾名思义，加载文件类型的加载器。所以我们执行其安装命令

```js

npm install file-loader --save-dev

```
5、安装完成之后就得修改配置文件 webpack.config.js 了。我们将里面的 module 模块修改如下：

```js

// 加载模块
module: {
    // 加载loader
    loaders: [
        {
            // .css 结尾的文件
            test: /\.css$/,
            loader: "style-loader!css-loader"
        }, {
            test: /\.(png|jpg|jpeg|gif|woff)$/,
            // 凡是小于8k的图片都将被转换为base64的格式
            loader: "file-loader?limit=8192&name=[path][name].[ext]",
        }
    ]
}

```
6、解释：limit 是限制图片大小，低于 8kb 的图片将以 base64 的形式直接插入到原引用处。name及其后面的参数表示输出的目录和图片名称。

7、好了，我们执行 npm start 试试看能不能成功。显而易见，结果是打包成功！此时我们在浏览器中打开 index.html 。可以发现 css 样式和背景图片均可以显示出来。棒棒哒！























