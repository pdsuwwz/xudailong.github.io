---
layout: post
title:  "webpack加载器loader的使用（上）"
date:   2018-04-16 07:58:22
categories: webpack
tags:  打包 webpack
author: 王文章
---

* content
{:toc}

![webpack](https://i.loli.net/2018/04/21/5ada9267452a4.jpg)

在上一节的使用配置文件 webpack.config.js 和修改 package.json 文件使得可以快速启动 webpack 的方式来对项目进行了打包。即使用了以下命令：

```js

npm start

```





本节将分成两小节来展示如何将 css 和图片资源打包到 js 里并能够展示到 html 页面中
> webpack 加载器 loader 的使用（上）：css 文件的打包
> 
> webpack 加载器 loader 的使用（下）：图片资源的打包

## 过程

1、OK，接着上一节的内容，另复制一份 first-webpack 文件夹。防止与上一节代码混淆。

2、为了更好的对js文件进行模块化地划分，我们在根目录中新建一个*main.js*文件
3、main.js 文件里使用以下代码来引入其他文件夹里的js，OK，再添加一个 js 文件夹，用来放前两小节用到的 index.js。

```js

require("./app/js/index.js");

```

4、在 app 文件夹里新建两个文件夹，分别命名为：css 和 images
5、在 css文件夹新建一个css文件：mystyle.css，images加入两个不同大小的图片（后面会用到）。
6、mystyle.css中这样写

```css

*{
    margin: 0;
    padding: 0;
}


.btn{
    width: 100px;
    height: 50px;
    background-color: #dd3915;
    border: 1px #dd3915 solid;
    text-align: center;
}

```


7、目录文件结构如下：


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

8、目前 main.js 文件引入文件如下

```js

require("./app/js/index.js");
require("./app/css/mystyle.css");

```

9、修改我们的 webpack.config.js 文件，将入口文件改为刚刚建立的 main.js 文件的路径。

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
    }
}

```


10、在根目录 first-webpack 中新建一个 index.html，注意 里面引入打包好的js文件：*../build/main.bundle.js*

```html

<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>webpack打包测试</title>
</head>
<body>
    <button class="btn">webpack引入样式</button>
    <script src="../build/main.bundle.js"></script>
</body>
</html>

```


11、开始打包，直接使用以下命令进行打包

```js

npm start

```

12、结果提示打包出现错误，失败了。。错误提示如下，大致意思是需要一个 loader 加载器来加载 css 文件，这里我们就需要一个 *css-loader*，通过 css-loader，就可以实现在js文件中通过 require 的方式来引入css。

```js

ERROR in ./app/css/mystyle.css
Module parse failed: C:\***\first-webpack\app\css\mystyle.css Unexpect
ed token (1:0)
You may need an appropriate loader to handle this file type.

```

13、打开cmd，运行如下命令安装 *css-loader*  

```js

npm install css-loader --save-dev

```

安装完成后就得修改 webpack.config.js 文件了，在其中加入 module 模块，具体接着见如下代码

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
                loader: "css-loader"
            }
        ]
    }
}

```


14、执行我们的 npm start 命令，诶，竟然打包成功，不报错了！
15、打开 index.html 文件赶紧看看呗。然而打开后发现页面还是那样，没有样式。。这是怎么回事呢？原来 webpack 只是把 css 文件引入到 js 里了，但没有在html中以 style 的方式嵌入 css，因此我们还需要安装一个模块 *style-loader* 执行以下命令。


```js

npm install style-loader --save-dev

```

16、安装完成后继续修改配置文件呗，，将 *webpack.config.js* 中 module 模块里的 loader 修改为如下代码，注意这里 模块 style 和 css 的位置固定，不可互换（loader加载方式为从上到下，从右往左），这里用了感叹号 " ! " 来分隔开。

```js

loader: "style-loader!css-loader"

```
17、重新执行 npm start 命令，打包成功!!! 之后在浏览器中将 index.html 页面重新刷新，发现 button 有样式了，说明 css 被加载了进来，右键审查元素也可以看到嵌入式的 css 样式。





















