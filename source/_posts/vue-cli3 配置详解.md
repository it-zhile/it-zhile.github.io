---
title: vue-cli3 配置详解
date: 2019-04-27 15:37:14
tags: [vue,vue-cli]
categories: 前端
description: vue-cli 3 就项目性能而言已经相当友好了，私有制定性也特别强，各种配置也特别贴心，可以根据项目大小和特性制定私有预设，对前期项目搭建而言效率极大提升了。
---

# vue-cli3.0 配置详解

> [官方介绍](https://cli.vuejs.org/zh/guide/)

vue-cli 3 就项目性能而言已经相当友好了，私有制定性也特别强，各种配置也特别贴心，可以根据项目大小和特性制定私有预设，对前期项目搭建而言效率极大提升了。



## 新建项目

```js
# 安装
npm install -g @vue/cli
# 新建项目
vue create my-project
# 项目启动
npm run serve
# 打包
npm run build
```



### 配置选择

3.0 版本包括  **默认预设配置** 和  **用户自定义配置**。

- 默认预设配置：`default (babel, eslint)`
- 用户自定义配置：`Manually select features`

```shell
C:\Users\Administrator\Desktop\vant-demo-master>vue create vue-cli3.0

Vue CLI v3.2.1
┌───────────────────────────┐
│  Update available: 3.3.0  │
└───────────────────────────┘
? Please pick a preset: (Use arrow keys)
> default (babel, eslint)
  Manually select features
```



### 自定义配置

> **用户自定义配置包括以下功能：**
>
> `Babel `、`TypeScript `、`Progressive Web App (PWA) Support` 、`Router `、`Vuex `、C`SS Pre-processors` 、`Linter / Formatter` 、`Unit Testing` 、`E2E Testing`

可以根据项目大小和功能体验配置不同的功能，`空格键 - 选中/反选` 、`按a键 - 全选/全不选`、`按i键 -反选已选择项` 、`上下键 - 上下移动选择`

```bash
Vue CLI v3.2.1
┌───────────────────────────┐
│  Update available: 3.3.0  │
└───────────────────────────┘
? Please pick a preset: Manually select features
? Check the features needed for your project: (Press <space> to select, <a> to toggle all, <i> to invert selection)
>(*) Babel
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support
 (*) Router
 (*) Vuex
 (*) CSS Pre-processors
 ( ) Linter / Formatter
 ( ) Unit Testing
 ( ) E2E Testing
```

在用户自定义配置之后，会询问是否保存当前的配置功能为将来的项目配置的预设值，这样下次可直接使用不需要再次配置。



### 自定义配置细节

在选择功能后按 `Enter 键` 会询问更细节的配置

#### TypeScript

```bash
Vue CLI v3.2.1
┌───────────────────────────┐
│  Update available: 3.3.0  │
└───────────────────────────┘
? Please pick a preset: Manually select features
? Check the features needed for your project: Babel, TS, PWA, Router, Vuex, CSS Pre-processors, Linter, Unit, E2E
# 是否使用class风格的组件语法
? Use class-style component syntax? (Y/n)
# 是否使用babel做转义
? Use Babel alongside TypeScript for auto-detected polyfills? (Y/n)
```

#### Router 

```bash
# 路由器使用 history 历史模式
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n)Y
```

#### CSS Pre-processors

```bash
# 选择CSS 预处理类型
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): (Use arrow keys)
> Sass/SCSS
  Less
  Stylus
```

#### Linter / Formatter

```bash
# 选择Linter / Formatter规范类型
? Pick a linter / formatter config: (Use arrow keys)
  TSLint
  ESLint with error prevention only
  ESLint + Airbnb config
  ESLint + Standard config
> ESLint + Prettier
  ? Pick additional lint features: 
  >(*) Lint on save
   ( ) Lint and fix on commit
```

#### Unit Testing

```bash
# 选择一个单元测试解决方案
? Pick a unit testing solution: (Use arrow keys)
> Mocha + Chai
  Jest
```

#### E2E Testing

```bash
# 选择一个E2E测试解决方案
? Pick a E2E testing solution: (Use arrow keys)
> Cypress (Chrome only)
  Nightwatch (Selenium-based)
```

#### 配置的存放类型

```bash
# 选择 Babel, PostCSS, ESLint 等自定义配置的存放位置 
? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? (Use arrow keys)
> In dedicated config files
  In package.json
```

#### 是否保存为配置预设值

```bash
# 在用户自定义配置之后，会询问是否保存当前的配置功能为将来的项目配置的预设值，下次可直接使用不需要再次配置。
Save this as a preset for future projects? (y/N)N
```

## 环境变量和模式

> vue-cli3.0 移除了配置文件目录： config和build文件夹。可以说是非常的精简了，那移除了配置文件目录后如何自定义配置环境变量和模式呢 ?



### 环境变量和模式的作用

所有方法都是来源于现实的需求。在一个产品的前端开发过程中，一般来说会经历本地开发、测试脚本、开发自测、测试环境、预上线环境，然后才能正式的发布。

对应每一个环境可能都会有所差异，比如说服务器地址、接口地址、websorket地址…… 等等。在各个环境切换的时候，就需要不同的配置参数，所以就可以用环境变量和模式，来方便我们管理。



### 环境变量

cli-3.0 总共提供了四种方式来制定环境变量都是在根目录下添加 `.env` 文件：

```bash
# 注意在根目录下创建的 .env 文件不需要加后缀
.env              # 在所有的环境中被载入(不知道这个存在的意义，所有的都需要的也就不需要配置了吧)
.env.local        # 在所有的环境中被载入，与 .env 的区别是只会在本地，该文件不会被git跟踪
.env.[mode]       # 只在指定的模式中被载入，比如 .env.development 来配置开发环境的配置
.env.[mode].local # 只在指定的模式中被载入，与.env.[mode]的区别也只是会在本地生效，该文件不会被git跟踪
```

**注意在根目录下创建的 `.env` 文件不需要加后缀**

环境文件里只能包含环境变量的 `键=值`对：

```bash
NODE_ENV = development
VUE_APP_PATTERN=aaa
```



### 环境变量的使用

设置完环境变量之后就可以在我们的项目中使用这两个变量了。

不过还需要注意的是在项目的不同地方使用，限制也不一样。

1. 在项目中，也就是 `src` 中使用环境变量的话，必须以 `process.en` 开头。

   例如我们可以在 `main.js` 中直接输出：`console.log(process.env.VUE_APP_PATTERN)`

2. 在 `webpack` 配置中使用，没什么限制，可以省略 `process.env` 直接通过 `NODE_ENV` 变量名来使用

3. 在 `public/index.html` 中的使用分三类：（没怎么用过）

   ```html
   <%= VAR %> 用于非转换插值  
   例如：`<link rel="shortcut icon" href="<%= BASE_URL %>favicon.ico">`
   <%- VAR %> 用于HTML转义插值
   <% expression %> 用于JavaScript控制流 
   ```



### 模式

模式是Vue CLI项目中的一个重要概念。默认情况下，Vue CLI项目中有三种模式：

1. development：在`vue-cli-service serve`下，即开发环境使用
2. production：在`vue-cli-service build` 和`vue-cli-service test:e2e`下，即正式环境使用
3. test： 在`vue-cli-service test:unit`下使用

另外，如果你想要修改模式下默认的环境变量的话可以通过--mode来实现，例如：

```
 "dev-build": "vue-cli-service build --mode development"
```

有环境变量就能完成我们的需求了，为什么需要有模式的存在，个人认为模式是为了提供给第三方的插件一个辨识。例如vuex可以根据模式的不同，在development自动注入`devtoolPlugin`插件，利于开发，而在production中检测到错误不会进行console。

### 举例

说完了概念，可能还是比较模糊，可以试着添加一个 `stage` 环境，用来模拟预上线。
首先在 `package.json` 添加一种类型，并修改默认环境变量为 `stage` 环境变量

```json
# package.json
"scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "stage": "vue-cli-service build --mode stage"
},
```

在根目录下创建 `.env.test` 文件，来声明变量：

```json
# .env.test文件
NODE_EVN = production
VUE_APP_CURRENTMODE = stage
outputDir = stage
```

这里声明的 `NODE_EVN = production ` 用来表示这是生产环境，`VUE_APP_CURRENTMODE`为项目变量，`outputDir` 为除数打包后文件的地址。
在 `vue.config.js` 中使用环境变量，制定输出文件为环境变量配置的文件：

```js
# vue.config.js
module.exports = {
    outputDir: process.env.outputDir,
    assetsDir: 'static'
}
```

最后执行命令 `npm run stage` 能看到根目录下生成了 `stage` 文件，这样我们就配置完了 `stage` 环境。

详细的项目地址可以参考：[以vue-cli3.0为基础搭建的一个工程化前端demo](https://github.com/Abiel1024/vue-project)



## vue.config.js 自定义配置

### 完整默认配置

```js
module.exports = {
 // 基本路径
 baseUrl: '/',
 // 输出文件目录
 outputDir: 'dist',
 // eslint-loader 是否在保存的时候检查
 lintOnSave: true,
 // use the full build with in-browser compiler?
 // https://vuejs.org/v2/guide/installation.html#Runtime-Compiler-vs-Runtime-only
 compiler: false,
 // webpack配置
 // see https://github.com/vuejs/vue-cli/blob/dev/docs/webpack.md
 chainWebpack: () => {},
 configureWebpack: () => {},
 // vue-loader 配置项
 // https://vue-loader.vuejs.org/en/options.html
 vueLoader: {},
 // 生产环境是否生成 sourceMap 文件
 productionSourceMap: true,
 // css相关配置
 css: {
  // 是否使用css分离插件 ExtractTextPlugin
  extract: true,
  // 开启 CSS source maps?
  sourceMap: false,
  // css预设器配置项
  loaderOptions: {},
  // 启用 CSS modules for all css / pre-processor files.
  modules: false
 },
 // use thread-loader for babel & TS in production build
 // enabled by default if the machine has more than 1 cores
 parallel: require('os').cpus().length > 1,
 // 是否启用dll
 // See https://github.com/vuejs/vue-cli/blob/dev/docs/cli-service.md#dll-mode
 dll: false,
 // PWA 插件相关配置
 // see https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-pwa
 pwa: {},
 // webpack-dev-server 相关配置
 devServer: {
  open: process.platform === 'darwin',
  host: '0.0.0.0',
  port: 8080,
  https: false,
  hotOnly: false,
  proxy: null, // 设置代理
  before: app => {}
 },
 // 第三方插件配置
 pluginOptions: {
  // ...
 }
}
```





## 样式全局变量与方法注入

> [向预处理器 Loader 传递选项](https://cli.vuejs.org/zh/guide/css.html#%E5%90%91%E9%A2%84%E5%A4%84%E7%90%86%E5%99%A8-loader-%E4%BC%A0%E9%80%92%E9%80%89%E9%A1%B9)

有的时候你想要向 webpack 的预处理器 loader 传递选项。你可以使用 `vue.config.js` 中的 `css.loaderOptions` 选项。比如你可以这样向所有 Sass 样式传入共享的全局变量：

```js
// vue.config.js
module.exports = {
  css: {
    loaderOptions: {
      // 给 sass-loader 传递选项
      sass: {
        // @/ 是 src/ 的别名
        // 所以这里假设你有 `src/variables.scss` 这个文件
        data: `@import "@/variables.scss";`
      }
    }
  }
}
```

Loader 可以通过 `loaderOptions` 配置，包括：

- [css-loader](https://github.com/webpack-contrib/css-loader)
- [postcss-loader](https://github.com/postcss/postcss-loader)
- [sass-loader](https://github.com/webpack-contrib/sass-loader)
- [less-loader](https://github.com/webpack-contrib/less-loader)
- [stylus-loader](https://github.com/shama/stylus-loader)





## 移动端单位适配

配置常用到的插件有：

- `amfe-flexible`：让网页根据设备dpi和宽度，利用viewport和html根元素的font-size配合rem来适配不同尺寸的移动端设备
- `postcss-pxtorem`：将项目中css的px转成rem单位，免去计算烦恼

### amfe-flexible 与 pxtorem

#### 安装

```js
# 安装 amfe-flexible
npm i amfe-flexible -S

# 安装 pxtorem
npm install postcss-pxtorem -D
```

#### **amfe-flexible 引入**

```js
# 入口文件 main.js
import 'amfe-flexible';
```

#### pxtorem配置

> rootValue ：  设计稿宽度的1/10。
>
> propList：需要做转化处理的属性，如`hight`、`width`、`margin`等，`*`表示全部。
>
> **注意 ：pxtorem 中，对于想忽略的 px 写成大写即可，如 `border:1PX solid #fff;`**

**pxtorem 配置有两种方式：**

1. 从根目录下的 postcss.config.js  引入：

   ```js
   # postcss.config.js
   module.exports = {
       plugins: {
           'autoprefixer': {
               browsers: ['Android >= 4.0', 'iOS >= 7']
           },
           'postcss-pxtorem': {
                 rootValue: 37.5,
                 propList: ['*']
           }
       }
   }
   ```

2. 从根目录下的 vue.config.js 引入：

   ```js
   # vue.config.js
   const autoprefixer = require('autoprefixer');
   const pxtorem = require('postcss-pxtorem');
   
   module.exports = {
       css: {
           loaderOptions: {
               postcss: {
                   plugins: [
                       autoprefixer(),
                       pxtorem({
                           rootValue: 37.5,
                           propList: ['*']
                       })
                   ]
               }
           }
       }
   }
   ```

#### 关于字号

配置完成后运行项目可以发现文字不给定字号是不会随屏幕缩放的，解决办法有两种：

1. 给每个文字明确的字号大小
2. 在 `app.vue` 里给根元素 `#app` 设置字体大小（推荐）