---
title: Vue项目优化打包——前端加分项
author: Robbin
banner_img: /images/common/default_index_img.jpg
index_img: /images/common/vue.jpeg
date: 2022-03-08 13:38:47
updated: 2022-03-08 13:38:47
tags: ["#前端", "#Vue.js"]
categories: Vue.js
---

## 前言

<p class="note note-success">Vue项目开发完毕后，对项目进行打包发布之前，必不可少的操作就是项目优化，这也是程序猿的加分项。跟随本文的脚步来看看如何对项目进行优化吧~</p>

<!-- more -->

## 一、路由懒加载

### 1. 为什么需要路由懒加载

当刚运行项目的时候，发现刚进入页面，就将所有的 js 文件和 css 文件加载了进来，这一进程十分的消耗时间。
如果打开哪个页面就对应的加载响应页面的 js 文件和 css 文件，那么页面加载速度会大大提升。

### 2. 如何实现路由懒加载

vue 官方文档：[路由懒加载](https://router.vuejs.org/zh/guide/advanced/lazy-loading.html)代码如下（示例）:

```JavaScript
{
  path: '/login',
  component: () => import('@/views/login/index'),
  hidden: true
},
```

### 3. 路由懒加载中的魔法注释

通过在注释中指定 `webpackChunkName`，可以自定义这个文件的名字。
代码如下（示例）:

```JavaScript
components = () => import(/_ webpackChunkName:"login"_/ "../component/Login.vue")
```

## 二、分析包大小

### 1. 需求

想要知道打包生成的文件中，每个文件所占的空间大小。以便于我们分析，对代码进行优化处理。

### 2. 如何生成打包分析文件

终端中运行 `npm run preview -- --report`, 这个命令会从我们的入口 main.js 进行依赖分析，分析出各个包的大小。最终会在生成的 dist 文件夹下生成一个 report.html 的文件，打开后就可以看到我们在项目使用文件占据的空间大小啦~
(效果图如下：)

![report.html](./images/a8edced7345a44a1b732df1bb2596db1.jpg)

## 三、webpack 配置排除打包

### 1. 需求

将一些不常用的包，排除在生成的打包文件以外。例如：上图所示的 xsxl.js 、 element.js，可以将其排除在打包生成的文件以外

### 2. 排除打包

找到 `vue.config.js`， 添加 `externals` 项，具体如下：
代码如下（示例）：

```JavaScript
configureWebpack: {
  // 配置单页应用程序的页面的标题
  name: name,
  externals: {
    /\*\*
    _ externals 对象属性解析。
    _ 基本格式：
    _ '包名' : '在项目中引入的名字'
    _
    \*/
    'vue': 'Vue',
    'element-ui': 'ElementUI',
    'xlsx': 'XLSX'
    },
    resolve: {
    alias: {
    '@': resolve('src')
    }
  }
}
```

## 四、 引用网络资源

### 1. 需求

我们进行了上一步操作以后，打包生成后的包小了很多。但是呢，没有这些依赖包的话，项目上线是没有办法运行的。那么就需要引用网络中的资源，来支持我们代码的运行。

### 2. CDN

`CDN` 全称叫做“Content Delivery Network”，中文叫内容分发网络。我们用它来提高访问速度
把一些静态资源：css， .js，图片，视频放在第三方的 CDN 服务器上，可以加速访问速度。

#### 好处：

减少应用打包出来的包体积
加快静态资源的访问
利用浏览器缓存，不会变动的文件长期缓存

### 3. 实现步骤

注意:在开发环境时，文件资源还是可以从本地 `node_modules` 中取出，而只有项目上线了，才需要去使用外部资源。此时我们可以使用环境变量来进行区分。具体如下：
代码如下（示例）：
在 `vue.config.js` 文件中:

```JavaScript
let externals = {}
let cdn = { css: [], js: [] }
const isProduction = process.env.NODE_ENV === 'production' // 判断是否是生产环境
if (isProduction) {
  externals = {
      /**
      * externals 对象属性解析：
      * '包名' : '在项目中引入的名字'
    */
      'vue': 'Vue',
      'element-ui': 'ELEMENT',
      'xlsx': 'XLSX'
  }
  cdn = {
    css: [
      'https://unpkg.com/element-ui/lib/theme-chalk/index.css' // element-ui css 样式表
    ],
    js: [
      // vue must at first!
      'https://unpkg.com/vue@2.6.12/dist/vue.js', // vuejs
      'https://unpkg.com/element-ui/lib/index.js', // element-ui js
      'https://cdn.jsdelivr.net/npm/xlsx@0.16.6/dist/xlsx.full.min.js', // xlsx
    ]
  }
}
```

webpack 配置 `externals` 配置项

```JavaScript
configureWebpack: {
  // 配置单页应用程序的页面的标题
  name: name,
  externals: externals,
  resolve: {
    alias: {
      '@': resolve('src')
    }
  }
}
```

通过 `html-webpack-plugin` 注入到 `index.html` 之中:
在`vue.config.js`文件中配置:

```JavaScript
chainWebpack(config) {
  config.plugin('preload').tap(() => [
    {
      rel: 'preload',
      fileBlacklist: [/\.map$/, /hot-update\.js$/, /runtime\..*\.js$/],
      include: 'initial'
    }
  ])
  // 注入cdn变量 (打包时会执行)
  config.plugin('html').tap(args => {
    args[0].cdn = cdn // 配置cdn给插件
    return args
  })
  // 省略其他...
}
```

找到 `public/index.html` 通过配置 CDN Config 依次注入 css 和 js。修改 head 的内容如下:

```html
<head>
  <meta charset="utf-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta
    name="viewport"
    content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no"
  />
  <link rel="icon" href="<%= BASE_URL %>favicon.ico" />
  <title><%= webpackConfig.name %></title>

  <!-- 引入样式 -->
  <% for(var css of htmlWebpackPlugin.options.cdn.css) { %>
  <link rel="stylesheet" href="<%=css%>" />
  <% } %>

  <!-- 引入JS -->
  <% for(var js of htmlWebpackPlugin.options.cdn.js) { %>
  <script src="<%=js%>"></script>
  <% } %>
</head>
```

## 五、 打包去除 `console.log`

### 1. 需求

在项目打包上线后，去除掉代码中所有的 `console.log`

### 2. 代码演示

在 `vue.config.js` 文件中配置：
代码如下（示例）：

```JavaScript
chainWebpack(config) {
  config.optimization.minimizer('terser').tap((args) => {
    args[0].terserOptions.compress.drop_console = true
    return args
  })
}
```
