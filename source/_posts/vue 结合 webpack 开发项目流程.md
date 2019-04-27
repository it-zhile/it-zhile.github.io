---
title: vue 结合 webpack 开发项目流程   #文章标签
date: 2018-11-17 18:06:22             #文章创建时间
categories: 前端                      #分类
tags: [前端,vue,webpack]              #文章标签，可空，多标签请用格式，注意:后面有个空格
# description: 附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面
# photos:                             #文章标题图片
#     - "http://oz2tkq0zj.bkt.clouddn.com/17-11-9/52323298.jpg"
---
# vue结合webpack开发项目流程

> 案例文件夹说明：
>
> webpack为隔行变色案例
>
> vue_webpack_remould为使用隔行变色改造好的vue开发环境
>
> vue_webpack_demo为回顾与梳理配置的vue开发环境
>
> vue_webpack为使用源文件快速搭建的vue开发环境



## webpack简介

> 官网：https://doc.webpack-china.org/



### 什么是webpack?

WebPack 可以看做是 **模块打包机**：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其转换和打包为合适的格式供浏览器使用。



### webpack能做什么？

#### 网页中有哪些常见的静态资源

- CSS：.css、.less、.sass、.scss
- 图片：.png、.jpg、.gif、.bmp
- JS：.js、.coffee、.js
- 字体：.ttf、.woff、.woff2、.svg、.eot
- 模板文件：.jade、.vue



#### 页面引入的静态资源多了以后有什么问题

加载慢 ：请求次数多（因为浏览从上到下解析HTML文件，当遇到 script、link、img 会立即发起很多的二次请求；）

依赖复杂：当项目大了以后，各种文件之间有很复杂的依赖关系，如果处理不好，则项目运行会经常报错！

#### 如何解决上述两个问题

加载慢怎么解决：图片合并成精灵图、CSS和JS进行压缩混淆合并、图片转成base64

依赖问题：基于模块化，让某些模块化工具帮我们解决复杂的依赖关系

#### 如何完美实现上述的2种解决方案

使用Webpack或者Gulp

- 借助于webpack这个前端自动化构建工具，可以完美实现资源的合并、打包、压缩、混淆等诸多功能。






### WebPack与Grunt/Gulp的区别

其实Webpack和另外两个并没有太多的可比性，Gulp/Grunt是一种能够优化前端的开发流程的工具，而WebPack是一种模块化的解决方案，不过Webpack的优点使得Webpack在很多场景下可以替代Gulp/Grunt类的工具。

**Grunt和Gulp的工作方式是：**在一个配置文件中，指明对某些文件进行类似编译，组合，压缩等任务的具体步骤，工具之后可以自动替你完成这些任务。

![](https://note.youdao.com/yws/api/personal/file/F31F51715BC34A678F1F24BEAFB1BADE?method=download&shareKey=bfe39e8162a3dacea82225140fc27a2f)

**Webpack的工作方式是：**把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个（或多个）浏览器可识别的JavaScript文件。

![](https://note.youdao.com/yws/api/personal/file/1A328F1FAEAE4F309835016629BB98F3?method=download&shareKey=fa4b41058a6d4973ff3090c58b79b79c)

如果实在要把二者进行比较，Webpack的处理速度更快更直接，能打包更多不同类型的文件。



### 安装webpack的两种方式

1. 运行 `npm i webpack -g` 全局安装webpack，这样就能在全局使用webpack的命令
2. 在项目根目录中运行 `npm i webpack -D` 安装到项目依赖中




## vue结合webpack搭建开发环境

> webpack 是基于 node 环境开发的，所以开发之前电脑需安装 node 
>
> node 官网：http://nodejs.cn/



### 完整项目目录结构预览

> 这是最终的 vue 开发环境目录，我们先从一个简单的隔行变色案例来一步一步改造成为我们需要的最终开发环境

```json
|-- src                // 项目的原文件目录 
|   |-- components     // .vue组件存放目录
|   |-- |-- App.vue    // 根组件
|   |-- images		   // 图片目录
|   |-- js             // 项目的js文件存放目录
|   |-- |-- main.js    // 项目的js打包入口文件
|   |-- style		   // 样式目录
|   |-- util		   // 项目插件工具类存放目录
|   |-- index.html     // 文件的主页面
|-- package.json       // 项目所需要的各种模块和配置信息
|-- .babelrc		   // 处理高级JS语法文件
|-- webpack.config.js  // webpack的配置文件
```



### 初始化项目

新建项目目录在项目目录运行命令 `npm init -y ` 初始化项目，使用npm管理项目中的依赖包，初始化完成后根目录会自动生成一个 `package.json` 该文件定义了这个项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。详情参考：http://javascript.ruanyifeng.com/nodejs/packagejson.html

``` bash
$ npm init -y
```



### 在index.html书写页面结构

> 这里只是为了方便后面的讲解，最终此页面会改造为一个容器，将来Vue渲染的组件，会填充到这个容器里显示

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
    <li>8</li>
    <li>9</li>
    <li>10</li>
  </ul>
</body>
</html>
```



### 安装webpack到项目开发依赖

在项目根目录中运行命令 `cnpm i webpack -D` 安装 webpack 到项目开发依赖中，安装完成后会自动生成一个 node_modules 文件夹此文件夹为 npm 安装该项目的依赖库 

```bash
npm i webpack --save-dev
```



### 安装jquery到项目运行依赖

运行命令安装 jquery.js 到项目运行依赖

```bash
npm i jquery --save
```



### 在main.js中书写代码逻辑

> 先来实现一个简单的隔行变色

```javascript
/* 这是项目的 JS 入口文件 */
// 使用 node 的方式导入 Jquery 包
// var $ = require('jquery');

// 1.0 (推荐)使用 ES6 导入模块的方式   import *** from '模块标识符'
import $ from 'jquery'

/* 2.0 使用 :odd 选取奇数行,使用 :even 选取偶数行 */
$(function () {
  $('li:odd').css('backgroundColor', 'blue');
  $('li:even').css('backgroundColor', 'red');
});
```



### 使用webpack处理main.js

直接在 `index.html` 页面上引用 `main.js` 会报错，因为浏览器不认识 `import` 这种高级的 JS 语法，此时我们需要使用 webpack 进行处理，webpack默认会把这种高级的语法转换为低级的浏览器能识别的语法；

执行打包编译命令 `webpack 要打包编译的文件路径 打包编译好后输出的文件路径`

```
webpack src/js/main.js dist/bundle.js
```

结果如下：

![webpack src/js/main.js dist/bundle.js的执行结果](https://note.youdao.com/yws/api/personal/file/17869CD9722B4505B59D1E5E695838AA?method=download&shareKey=b79bc9b1c1be9a38ec34adafc8f925f0)

从上图我们可以看出 `webpack` 编译了 `main.js`  

执行完毕后会在根目录下生成一个 dist 文件夹此文件夹下会有一个 bundle.js 文件，这个文件就是编译好后的 main.js ，此时再在 imdex.html 页面引入 bundle.js 文件就可以看到结果了。



### 创建webpack配置文件处理main.js并简化打包命令

> 每次页面代码改变都要在命令行执行 `webpack src/js/main.js dist/bundle.js` 这么一大坨相当恶心，所以在项目根目录中创建 `webpack.config.js` 配置文件来简化打包命令

由于运行 webpack 命令的时候，webpack 需要指定入口文件和输出文件的路径，所以，我们需要在 `webpack.config.js` 中配置这两个路径即 entry(入口文件) 和 output(输出文件的路径)

```javascript
/* 
 *  此文件为 webpackd 打包时候，默认要在项目根目录中查找的配置文件
 *  配置文件的默认名称为 webpack.config.js 
 */

/*  2.0
 *  导入 Node 中的 Path(路径) 模块
 *    问题1：为什么这里可以使用 Node 语法或者 引用Node模块？因为 webpack 是基于 Node 构建的
 *    问题2：为什么Node不识别 import？？？？   
 *           因为Node中的解析引擎是从 chrome中的V8移植过去的
 *           由于 V8 专门是为浏览器开发的引擎所以暂时不支持
 *           因此 Node 也被迫不支持 import
 */
var path = require('path');
// import path from 'path'


/*  1.0
 *  module.exports 这是一个典型的 Node 模块向外暴露成员的方式
 *  向外界暴露一个配置对象，将来 webpack 在启动的时候会默认来查找 webpack.config.js
 *  并读取这个文件中导出的配置对象，来进行打包处理 
 */
module.exports = {
  /* 2.1
   * entry 属性，表示要打包的文件的路径。
   *   path.join(__dirname,'') 是node.js中的一个全局变量
   *   它指向当前执行脚本所在的目录。主要是为了容错确保指向的目录正确性。
   */
  entry: path.join(__dirname, 'src/js/main.js'),
  /* 2.2
   * output属性：配置输出选项，配置打包好的文件的数据路径和文件名信息 
   *   path：指定输出文件的路径
   *   filename：指定输出文件的名称
   */
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  }
}
```

配置好后直接在命令行执行 `webpack` 既可，执行 `webpack` 就相当于执行之前的 `webpack src/js/main.js dist/bundle.js`  命令。



### 实现webpack的实时打包构建

> 由于每次重新修改代码之后，都需要手动运行 webpack 打包的命令，比较麻烦，所以使用 `webpack-dev-server` 来实现代码实时打包编译，当修改代码之后，会自动进行打包构建。

安装 webpack-dev-server 到开发依赖

```
cnpm i webpack-dev-server --save-dev
```

安装完成之后，在命令行直接运行 `webpack-dev-server` 来进行打包，发现报错，此时需要借助于 `package.json` 文件中的指令，来运行 `webpack-dev-server` 命令。

在 `package.json` 文件的 `scripts` 节点下新增 `"dev": "webpack-dev-server"`  指令

执行命令 `npm run dev` 

```
npm run dev
```

结果如下：

![npm run dev 执行结果](https://note.youdao.com/yws/api/personal/file/E4FB23B7E92740BFA4AC48D5239B8056?method=download&shareKey=3b0496f10c8f22590f8e35265681697f)

发现可以进行实时打包，但是并没有生成 dist 目录和 bundle.js 文件，这是因为 `webpack-dev-server` 将打包好的文件放在了内存中，把 `bundle.js` 放在内存中的好处是由于需要实时打包编译，所以放在内存中速度会非常快。

这个时候访问启动的 `http://localhost:8080/` 网站，发现是一个文件夹的面板，需要点击到src目录下，才能打开我们的 index 首页。

![访问 http://localhost:8080/](https://note.youdao.com/yws/api/personal/file/B9809B7C457A44E9B958434B9D8C9A35?method=download&shareKey=c087b7991223efd5188cc7fdd121f34f)

点击进入首页后发现引用不到 `bundle.js` 文件，因为 `bundle.js` 此时在内存中放着，所以需要修改 `index.html` 中 `script` 的 `src` 属性为 `<script src="../bundle.js"></script>` 

为了能在访问 `http://localhost:8080/` 的时候直接访问到index首页，可以使用 `--contentBase src` 指令来修改package.json 的 dev 指令来指定启动的根目录

```json
 "dev": "webpack-dev-server --contentBase src"
```

同时修改 `index` 页面中 `script` 的 `src` 属性为 `<script src="bundle.js"></script>`

注意：修改 src 文件夹以外的文件后都需要重新启动终端



### 实现自动打开浏览器、热更新和配置浏览器的默认端口号

**(推荐)方式1：**

修改 `package.json` 的script节点如下

```json
"dev": "webpack-dev-server --contentBase src --open --port 3000 --hot"
```

| 属性                | 描述                 |
| :---------------- | :----------------- |
| --contentBase src | 表示指定默认打开目录为 src 目录 |
| --open            | 表示自动打开浏览器          |
| --port 3000       | 表示打开的端口号为3000      |
| --hot             | 表示启用浏览器热更新         |

**方式2：**

1. 修改  `webpack.config.js`  文件，新增  `devServer`  节点此节点为一个对象如下：

```javascript
devServer:{    // 这个节点配置了 webpack-dev-server 的相关指令
    hot:true,  //启用浏览器热更新
    open:true, //自动打开浏览器
    port:4321，//指定端口号为4321
    contentBase:'src'//指定打开目录为src
}
```

2. 在头部引入`webpack`模块：

```javascript
var webpack = require('webpack');
```

3. 新增 `plugins` 节点此节点为一个数组，在 `plugins` 节点下新增：

```javascript
plugins: [
  new webpack.HotModuleReplacementPlugin()
]
```

注意：`package.json`  文件里的 `script` 节点中一定要添加 `“dev”:"webpack-dev-server"` 才能使用 `npm run dev` 命令进行实时打包



### 使用html-webpack-plugin插件配置启动页面

由于使用 `--contentBase` 指令的过程比较繁琐，需要指定启动的目录，同时还需要修改 `index.html` 中 `script` 标签的 `src` 属性，所以推荐大家使用 `html-webpack-plugin` 插件配置启动页面，该插件会自动生成 `index.html` 文件并把 `bundle.js` 注入到页面中！

运行命令安装 html-webpack-plugin 插件到开发依赖

```
cnpm i html-webpack-plugin --save-dev
```

修改 `webpack.config.js` 配置文件如下：

```javascript
var path = require('path');
/* 3.0 导入html-webpack-plugin 自动生成 HTMl 文件插件 */
var htmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
  entry: path.join(__dirname, 'src/js/main.js'),
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  /* 3.1 添加 plugins 节点配置插件 */
  plugins: [
    /* 3.2 
     * new htmlWebpackPlugin({})：实例化 html-webpack-plugin 插件
     *   template: 指定要编译的模板文件路径
     *   filename: 指定自动生成的HTML文件的名称
     */
    new htmlWebpackPlugin({
      template: path.join(__dirname, 'src/index.html'),
      filename: 'index.html'
    })
  ]
}
```

将 `index.html` 中 `script` 标签注释掉，因为 `html-webpack-plugin` 插件会自动把 `bundle.js` 注入到 `index.html` 页面中！



### loaders简介

通过使用不同的 `loader`，`webpack` 有能力调用外部的脚本或工具，实现对不同格式的文件的处理，比如说分析转换 `scss` 为 `css`，或者把下一代的 JS 文件`（ES6，ES7)` 转换为现代浏览器兼容的 `JS` 文件，对 `React` 的开发而言，合适的 `Loaders` 可以把 `React` 的中用到的 `JSX` 文件转换为 `JS` 文件。

`Loaders` 需要单独安装并且需要在 `webpack.config.js` 中的 `modules` 关键字下进行配置

| 属性         | 描述                                   |
| ---------- | ------------------------------------ |
| test       | 用以匹配 loaders 所处理文件的拓展名的正则表达式（必须）     |
| loader/use | loader 的名称，use 为 2.x 的写法推荐使用 use（必须） |
| include    | 添加必须处理的文件（文件夹）（可选）                   |
| exclude    | 屏蔽不需要处理的文件（文件夹）（可选）                  |
| query      | 为 loaders 提供额外的设置选项（可选）              |



### 使用webpack打包css文件

在页面中我们不可避免的要引入一些 CSS 文件比如说 base.css(页面样式的初始化)，我们可以在 `main.js` 里引入以作用全局。

在页面中引入 CSS 可通过 main.js 里引入

```javascript
/* 3.0 使用 import '直接通过路径标识符' 引入base.css样式 */
import '../style/base.css'
```

引入 css 文件后发现报错：You may need an appropriate loader to handle this file type.

```json
bundle.js:sourcemap:9947 Uncaught Error: Module parse failed: Unexpected token (10:12)
You may need an appropriate loader to handle this file type.
| 
| // 导入 css 文件
| import from '../style/index.css'
| 
| /* 使用 :odd 选取奇数行,使用 :even 选取偶数行 */

    at Object.<anonymous> (bundle.js:sourcemap:9947)
    at __webpack_require__ (bundle.js:sourcemap:679)
    at fn (bundle.js:sourcemap:89)
    at Object.<anonymous> (bundle.js:sourcemap:1015)
    at __webpack_require__ (bundle.js:sourcemap:679)
    at logLevel (bundle.js:sourcemap:725)
    at bundle.js:sourcemap:728
```

You may need an appropriate loader to handle this file type.（您可能需要一个合适的加载器来处理这个文件类型。）因为我们项目中没有配置处理 css 的 loader 所以才会报这个错误 

安装处理 CSS文 件的 loader 到开发依赖

```
cnpm i style-loader css-loader -D
```

在 `webpack.config.js` 配置文件里添加 module 配置节点：

```javascript
var path = require('path');
var htmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
  entry: path.join(__dirname, 'src/js/main.js'),
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  plugins: [
    new htmlWebpackPlugin({
      template: path.join(__dirname, 'src/index.html'),
      filename: 'index.html'
    })
  ],
  /* 4.0 添加 module 节点用来配置第三方 loader 模块
   *     module：用来配置第三方 loader 模块
   *       rules：配置文件的匹配规则
   *         test: 用以匹配 loaders 所处理文件的拓展名的正则表达式
   *         use : 表示使用哪些模块来处理
   *               use 中相关 loader 模块的调用顺序是从后向前调用的
   */
  module: {
    rules: [
      /* 5.0 添加处理 css 文件的匹配规则 */
      { test: /\.css$/, use: ['style-loader', 'css-loader'] }
    ]
  }
}
```

**注意**：`use` 表示使用哪些模块来处理 `test` 所匹配到的文件；`use` 中相关 loader 模块的调用顺序是从后向前调用的；



### 使用webpack打包sass文件

页面中除了会引用 CSS 文件外有时还会用到 SASS 文件，需要用到什么文件直接在 main.js 里导入就会作用到全局整个项目，在 main.js 中通过 import '路径标识符' 导入 index.scss 会发现报错，原因同上也是缺少相关的 loader 来处理 scss 文件，后续的 less、image 和一些高级 js语法 解决方案都一样，报错缺少什么 loader 就安装相关的 loader 即可。

安装处理 sass 文件的 loader 到开发依赖

```
cnpm i sass-loader node-sass --save-dev
```

在 `webpack.config.js` 的 module 节点下添加处理 sass 文件的 loader 模块匹配规则：

```javascript
/* 6.0 添加处理 sass 文件的匹配规则 */
{ test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] }
```



### 使用webpack打包less文件

安装处理 less 文件的 loader 到开发依赖

```
cnpm i less-loader less --save-dev
```

在 `webpack.config.js` 的 module 节点下添加处理 less文件的 loader 模块匹配规则：

```javascript
/* 7.0 添加处理 less 文件的匹配规则 */
{ test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] }
```



### 使用webpack处理css中的图片路径

当我们在页面中用 \<img> 标签来引入图片的时候没问题，但是使用 css 样式给某个标签设置背景图片就会报错，也是因为缺少相当的 loader 来处理我们 css 中的图片的路径。

安装处理 url路径 的 loader 到开发依赖

```
cnpm i url-loader file-loader --save-dev
```

在 `webpack.config.js` 的 module 节点下添加处理 url路径 的 loader 模块匹配规则：

```javascript
/* 8.0 添加处理 url路径 的匹配规则 */
{ test: /\.(png|jpg|gif)$/, use: 'url-loader' }
```

通过查看元素我们可以看到默认将背景图片转为了 base64 编码格式的图片

![背景图片被转为了 base64 编码格式](https://note.youdao.com/yws/api/personal/file/9ADC8742826D45809DB61B287F3B295B?method=download&shareKey=1b23f778252e3f2ec3146a6110c77f92)

可以通过`limit`指定进行base64编码的图片大小；只有小于等于指定字节（byte）的图片才会进行base64编码：

```javascript
/* 8.0 添加处理 url路径 的匹配规则 */
{ test: /\.(png|jpg|gif)$/, use: 'url-loader?limit=2000' },
```

![指定小于2000字节的图片才会进行base64转码](https://note.youdao.com/yws/api/personal/file/F4A9E47CC2EE4406B771109A9BF11D64?method=download&shareKey=0fb8bee651e9c54ed74a7cd60f861ed9)



### 使用babel处理高级JS语法

现在的浏览器中只能识别部分的 ES6 语法，某些 js 的高级语法不能识别，比如 class类，我们是 main.js 里写入

```javascript
/* 6.0 在 main.js 中写入 js 高级语法 
 * 在 class 内部，不能直接写语句   
 * 在类的作用内部，只能定义 方法 和 属性
 * class 只是一个 语法糖
 */
class Person {
  // 静态属性
  static info = { adderss:"北京", name: 'jack', age: 16 }
};
console.log(Person.info)
```

浏览器会报错：

![](https://note.youdao.com/yws/api/personal/file/EA3FBA751DF6436A91E6032BC1D1384E?method=download&shareKey=3adae98e685c9980dd9ba0de69aa0a20)

所以我们需要安装 babel 来处理这些高级语法将其转换为浏览器兼容的 `JS` 文件

安装处理 babel 的 loader 到开发依赖

```
cnpm i babel-core babel-loader babel-plugin-transform-runtime --save-dev
```

安装 babel 的转换语法到开发依赖

```javascript
/* 包含了所有的ES相关的语法（两者二选一，推荐）  */
cnpm i babel-preset-env babel-preset-stage-0 --save-dev
/* 支持ES语法到2015（两者二选一） */
cnpm i babel-preset-es2015 babel-preset-stage-0 --save-dev
```

在 `webpack.config.js` 的 module 节点下添加相关 loader 模块的匹配规则

**注意**：一定要把 `node_modules` 文件夹添加到排除项

```javascript
/* 9.0  添加处理 bable处理js高级语法 的匹配规则，并排除掉 node_modules 文件夹 */
{ test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ },
```

在项目根目录中添加 `.babelrc` 文件，并修改这个配置文件如下：

```javascript
{
	// 注意：如果安装的ES语法支持到2015侧这里为
    // "presets":["es2015", "stage-0"],
    "presets":["env", "stage-0"],
    "plugins":["transform-runtime"]
}
```

**注意：语法插件 `babel-preset-es2015` 可以更新为 `babel-preset-env` ，它包含了所有的ES相关的语法；**

以上是 webpack 的一些基本的配置，下面我们将介绍如何将此案例改造成我们需要的 vue 开发环境



### .vue 单文件组件

> 官网：https://cn.vuejs.org/v2/guide/single-file-components.html

在学习的时候我们使用  `Vue.component( 'login',  { template: '<h1>登陆组件</h1>' })` 来定义全局组件，紧接着在每个页面内指定一个容器元素。

这种方式在很多中小规模的项目中运作的很好，在这些项目里 JavaScript 只被用来加强特定的视图。但当在更复杂的项目中，或者你的前端完全由 JavaScript 驱动的时候，下面这些缺点将变得非常明显：

- **全局定义 (Global definitions)** 强制要求每个 component 中的命名不得重复
- **字符串模板 (String templates)** 缺乏语法高亮，在 HTML 有多行的时候，需要用到丑陋的 `\`
- **不支持 CSS (No CSS support)** 意味着当 HTML 和 JavaScript 组件化时，CSS 明显被遗漏
- **没有构建步骤 (No build step)** 限制只能使用 HTML 和 ES5 JavaScript, 而不能使用预处理器，如 Pug (formerly Jade) 和 Babel

文件扩展名为 `.vue` 的 **single-file components(单文件组件)** 为以上所有问题提供了解决方法，并且还可以使用 webpack 或 Browserify 等构建工具。



### 创建 Hello.vue 组件

在网页中学习 `Vue` 的时候，直接把组件和 `script` 代码写到一起了，但是在使用`webpack` 结合 `vue` 开发的时候，推荐将所有的组件(页面)，单独定义为一个 `.vue` 文件，这每一个 `.vue` 文件都是一个单独的 `vue 组件` ，在每个 `.vue` 组件内部，分为 三部分： `template`  `script`  `style`

```vue
<template>
<!-- 
	注意：在 .vue 的组件中，template 中必须有且只有唯一的根元素进行包裹
    一般都用 div 当作唯一的根元素 
-->
  <div>
    <p> {{Hello}} World! </p>
  </div>
</template>
<!--
	注意：在.vue的组件中，通过script标签来定义组件的行为
	需要使用ES6中提供的export default方式，导出一个vue实例对象
-->
<script>
export default {
  data:function(){
    return { msg: 'Hello'}
  }
}
</script>
<!-- 
注意：style标签要加上scoped属性避免后续多个组件样式覆盖 
-->
<style scoped>
p {
  font-size: 20px;
  text-align: center;
}
</style>
```

有了 `.vue` 组件，我们就进入了高级 JavaScript 应用领域。接下来就可以改造我们的 vue 的开发环境了。



### 在webpack中配置.vue组件页面的解析

改造 index.html 页面，改造好的 index.html 页面当做一个容器用来展示 vue 渲染的组件，至此这个页面后续基本不会再动了。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <!-- 这是容器，将来Vue渲染的组件，会填充到这个位置显示 -->
  <div id="app"></div>
</body>
</html>
```

在 components 文件夹里创建 `App.vue` 页面根组件，在 vue 项目中所有的页面都被当作一个组件

```vue
<template>
<!-- 
	注意：在 .vue 的组件中，template 中必须有且只有唯一的根元素进行包裹
	一般都用 div 当作唯一的根元素 
-->
  <div>
    <h1>这是APP组件 - {{msg}}</h1>
    <h3>我是h3</h3>
  </div>
</template>
<!--
	注意：在.vue的组件中，通过script标签来定义组件的行为，
	需要使用ES6中提供的export default方式，导出一个vue实例对象
-->
<script>
export default {
  data() {
    return {
      msg: 'OK'
    }
  }
}
</script>
<!-- 注意：style标签要加上 scoped 属性避免后续多个组件样式覆盖 -->
<style scoped>
h1 {
  color: red;
}
</style>
```

执行命令安装 vue.js 到项目运行依赖：

```
cnpm i vue -S
```

改造 `main.js` 入口文件：

```javascript
/* 1.0 导入 Vue 组件 */
import Vue from 'vue'
/* 2.0 导入 App.vue 页面根组件 */
import App from '../components/App.vue'
/* 3.0 创建一个 Vue 实例并使用 render 函数，渲染指定的组件 */
var vm = new Vue({
  el: '#app',
  render: c => c(App)
});
```

现在直接运行项目 npm run dev 会报错提示缺少相当的 loader 来处理我们的 .vue 文件

运行命令安装 解析转换 .vue 的 loader 为开发依赖：

```
cnpm i vue-loader vue-template-compiler -D
```

在 `webpack.config.js` 的 module 节点下添加处理 .vue 文件的 loader 模块匹配规则，并排除掉 node_modules文件夹

```javascript
/* 10.0 添加处理 .vue 文件的匹配规则，并排除掉 node_modules文件夹 */
{ test: /\.vue$/, use: 'vue-loader', exclude: /node_modules/ }
```

至此 vue 项目的基本开发环境已经搭建完成。



# vue 开发环境的搭建回顾与梳理

## 1.0 初始化项目

新建项目文件夹，在文件夹的根目录执行 npm init -y 初始化项目，会得到 package.json 文件

```
npm init -y
```

## 2.0 安装webpack到项目开发依赖

```
cnpm i webpack -D
```

## 3.0 创建项目结构目录

```json
|-- src                // 项目的原文件目录 
|   |-- components     // .vue组件存放目录
|   |-- |-- App.vue    // 根组件
|   |-- images		   // 图片目录
|   |-- js             // 项目的js文件存放目录
|   |-- |-- main.js    // 项目的js打包入口文件
|   |-- style		   // 样式目录
|   |-- util		   // 项目插件工具类存放目录
|   |-- index.html     // 文件的主页面
|-- package.json       // 项目所需要的各种模块和配置信息
|-- .babelrc		   // 处理高级JS语法文件
|-- webpack.config.js  // webpack的配置文件
```

## 4.0 编辑 index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <!-- 这是容器，将来Vue渲染的组件，会填充到这个位置显示 -->
  <div id="app"></div>
</body>
</html>
```

## 5.0 创建webpack配置文件处理main.js并简化打包命令

```javascript
var path = require('path');
module.exports = {
  entry: path.join(__dirname, 'src/js/main.js'),
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  }
}
```

## 6.0实现webpack的实时打包构建

安装 webpack-dev-server 到开发依赖

```
cnpm i webpack-dev-server --save-dev
```

安装完成之后，在 `package.json` 文件的 `scripts` 节点下新增 `"dev": "webpack-dev-server --contentBase src"`  指令

## 7.0 实现自动打开浏览器、热更新和配置浏览器的默认端口号

修改 `package.json` 的script节点如下

```json
"dev": "webpack-dev-server --contentBase src --open --port 3000 --hot"
```

## 8.0 使用html-webpack-plugin插件配置启动页面

使用 `html-webpack-plugin` 插件配置启动页面，该插件会自动生成 `index.html` 文件并把 `bundle.js` 注入到页面中！运行命令安装 html-webpack-plugin 插件到开发依赖

```
cnpm i html-webpack-plugin --save-dev
```

修改 `webpack.config.js` 配置文件，导入html-webpack-plugin 自动生成 HTMl 文件插件并添加 plugins 节点配置插件：

```javascript
var htmlWebpackPlugin = require('html-webpack-plugin');
```

```javascript
plugins: [
  new htmlWebpackPlugin({
    template: path.join(__dirname, 'src/index.html'),
    filename: 'index.html'
  })
]
```

## 9.0 使用webpack打包css文件

安装处理 CSS文 件的 loader 到开发依赖

```
cnpm i style-loader css-loader -D
```

在 `webpack.config.js` 配置文件里添加 module 配置节点并添加处理 css 文件的匹配规则：

```javascript
module: {
  rules: [
    { test: /\.css$/, use: ['style-loader', 'css-loader'] },
  ]
}
```

## 10. 使用webpack打包sass文件

安装处理 sass 文件的 loader 到开发依赖

```
cnpm i sass-loader node-sass --save-dev
```

在 `webpack.config.js` 的 module 节点下添加处理 sass 文件的 loader 模块匹配规则：

```javascript
{ test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] },
```

## 11. 使用webpack打包less文件

安装处理 less 文件的 loader 到开发依赖

```
cnpm i less-loader less --save-dev
```

在 `webpack.config.js` 的 module 节点下添加处理 less文件的 loader 模块匹配规则：

```javascript
{ test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
```

## 12. 使用webpack处理css中的图片路径

安装处理 url路径 的 loader 到开发依赖

```
cnpm i url-loader file-loader --save-dev
```

在 `webpack.config.js` 的 module 节点下添加处理 url路径 的 loader 模块匹配规则，通过`limit`指定进行base64编码的图片大小；只有小于等于指定字节（byte）的图片才会进行base64编码：

```javascript
{ test: /\.(png|jpg|gif)$/, use: 'url-loader?limit=2000' },
```

### 13. 使用babel处理高级JS语法

安装处理 babel 的 loader 到开发依赖

```
cnpm i babel-core babel-loader babel-plugin-transform-runtime --save-dev
```

安装 babel 的转换语法到开发依赖

```javascript
/* 包含了所有的ES相关的语法（两者二选一，推荐）  */
cnpm i babel-preset-env babel-preset-stage-0 --save-dev
/* 支持ES语法到2015（两者二选一） */
cnpm i babel-preset-es2015 babel-preset-stage-0 --save-dev
```

在 `webpack.config.js` 的 module 节点下添加相关 loader 模块的匹配规则

**注意**：一定要把 `node_modules` 文件夹添加到排除项

```javascript
{ test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ },
```

在项目根目录中添加 `.babelrc` 文件，并修改这个配置文件如下：

```javascript
{
    "presets":["env", "stage-0"],
    "plugins":["transform-runtime"]
}
```

## 14. 在webpack中配置.vue组件页面的解析

在 components 文件夹下新建 App.vue 根组件

```vue
<template>
  <div id="app">{{msg}}</div>
</template>

<script>
export default {
  data(){
    return {
      msg: 'Hello World!'
    }
  }
}
</script>

<style scoped>
div {
  text-align: center;
  font-size: 20px;
}
</style>
```

执行命令安装 vue.js 到项目运行依赖：

```
cnpm i vue -S
```

改造 `main.js` 入口文件：

```javascript
import Vue from 'vue'
import App from '../components/App.vue'
const vm = new Vue({
  el:'#app',
  render: c => c(App)
})
```

运行命令安装 解析转换 .vue 的 loader 为开发依赖：

```
cnpm i vue-loader vue-template-compiler -D
```

在 `webpack.config.js` 的 module 节点下添加处理 .vue 文件的 loader 模块匹配规则，并排除掉 node_modules文件夹

```javascript
{ test: /\.vue$/, use: 'vue-loader', exclude: /node_modules/ }
```

**vue 项目的基本开发环境搭建完成。**



# 快速搭建vue项目源文件

每次开发项目都要一步一步的配环境相当的麻烦，拿到三个配置文件，创建好项目结构，打开终端运行 `npm install`  或  `cnpm install`  既可自动为我们安装 package.json 里面的所以的插件依赖和 loader

### package.json文件

```json
{
  "name": "vue_webpack_demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --contentBase src --open --port 3000 --hot"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.2",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-preset-env": "^1.6.1",
    "babel-preset-stage-0": "^6.24.1",
    "css-loader": "^0.28.7",
    "file-loader": "^1.1.5",
    "html-webpack-plugin": "^2.30.1",
    "less": "^2.7.3",
    "less-loader": "^4.0.5",
    "node-sass": "^4.7.2",
    "sass-loader": "^6.0.6",
    "style-loader": "^0.19.1",
    "url-loader": "^0.6.2",
    "vue-loader": "^13.5.0",
    "vue-template-compiler": "^2.5.11",
    "webpack": "^3.10.0",
    "webpack-dev-server": "^2.9.7"
  },
  "dependencies": {
    "vue": "^2.5.11"
  }
}
```

### webpack.config.js文件

```javascript
/* 
 *  此文件为 webpackd 打包时候，默认要在项目根目录中查找的配置文件
 *  配置文件的默认名称为 webpack.config.js 
 */

/*  2.0
 *  导入 Node 中的 Path(路径) 模块
 *    问题1：为什么这里可以使用 Node 语法或者 引用Node模块？因为 webpack 是基于 Node 构建的
 *    问题2：为什么Node不识别 import？？？？   
 *           因为Node中的解析引擎是从 chrome中的V8移植过去的
 *           由于 V8 专门是为浏览器开发的引擎所以暂时不支持
 *           因此 Node 也被迫不支持 import
 */
var path = require('path');
// import path from 'path'

/* 3.0 导入html-webpack-plugin 自动生成 HTMl 文件插件 */
var htmlWebpackPlugin = require('html-webpack-plugin');

/*  1.0
 *  module.exports 这是一个典型的 Node 模块向外暴露成员的方式
 *  向外界暴露一个配置对象，将来 webpack 在启动的时候会默认来查找 webpack.config.js
 *  并读取这个文件中导出的配置对象，来进行打包处理 
 */
module.exports = {
  /* 2.1
   * entry 属性，表示要打包的文件的路径。
   *   path.join(__dirname,'') 是node.js中的一个全局变量
   *   它指向当前执行脚本所在的目录。主要是为了容错确保指向的目录正确性。
   */
  entry: path.join(__dirname, 'src/js/main.js'),
  /* 2.2
   * output属性：配置输出选项，配置打包好的文件的数据路径和文件名信息 
   *   path：指定输出文件的路径
   *   filename：指定输出文件的名称
   */
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  /* 3.1 添加 plugins 节点配置插件 */
  plugins: [
    /* 3.2 
     * new htmlWebpackPlugin({})：实例化 html-webpack-plugin 插件
     *   template: 指定要编译的模板文件路径
     *   filename: 指定自动生成的HTML文件的名称
     */
    new htmlWebpackPlugin({
      template: path.join(__dirname, 'src/index.html'),
      filename: 'index.html'
    })
  ],
  /* 4.0 添加 module 节点用来配置第三方 loader 模块
   *     module：用来配置第三方 loader 模块
   *       rules：配置文件的匹配规则
   *         test: 用以匹配 loaders 所处理文件的拓展名的正则表达式
   *         use : 表示使用哪些模块来处理
   *               use 中相关 loader 模块的调用顺序是从后向前调用的
   */
  module: {
    rules: [
      /* 5.0  添加处理 css 文件的匹配规则 */
      { test: /\.css$/, use: ['style-loader', 'css-loader'] },
      /* 6.0  添加处理 sass 文件的匹配规则 */
      { test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] },
      /* 7.0  添加处理 less 文件的匹配规则 */
      { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
      /* 8.0  添加处理 url路径 的匹配规则 */
      { test: /\.(png|jpg|gif)$/, use: 'url-loader?limit=200' },
      /* 9.0  添加处理 bable处理js高级语法 的匹配规则，并排除掉 node_modules文件夹 */
      { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ },
      /* 10.0 添加处理 .vue 文件的匹配规则，并排除掉 node_modules文件夹 */
      { test: /\.vue$/, use: 'vue-loader', exclude: /node_modules/ }
    ]
  }
}
```

### .babelrc文件

```json
{
	// 注意：如果安装的ES语法支持到2015侧这里为
    // "presets":["es2015", "stage-0"],
    "presets":["env", "stage-0"],
    "plugins":["transform-runtime"]
}
```

## vue结合webpack项目流程-源码下载
[vue结合webpack项目流程-源码](https://note.youdao.com/share/?id=ab5df8e2ea58ab6e9a3b1679488b610c&type=note#/)