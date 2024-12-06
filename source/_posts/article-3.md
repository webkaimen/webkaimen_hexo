---
title: vue-cli脚手架中引入elementUI框架
author: Robbin
banner_img: /images/common/default_index_img.jpg
index_img: /images/common/vue.jpeg
date: 2022-03-08 16:29:25
updated: 2022-03-08 16:29:25
tags: ["#Vue.js", "#前端", "#ElementUI", "#Vue-cli"]

categories: Vue.js
---

本文章是在 vue-cli 脚手架中引入 elementUI 的,请先创建一个脚手架再实行以下步骤。

### 1.下载 elementUI 框架

```
npm i element-ui -S
```

### 2.引入 Element

你可以引入整个 Element，或是根据需要仅引入部分组件。我引入的是完整的 Element。按需引入请自行去官网查阅。
完整引入
在 `main.js` 中写入以下内容：

```javascript
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from "vue";
import App from "./App";
import router from "./router";

// 引入element框架
import ElementUI from "element-ui";
import "element-ui/lib/theme-chalk/index.css";
Vue.use(ElementUI);

Vue.config.productionTip = false;

/* eslint-disable no-new */
new Vue({
  el: "#app",
  router,
  components: { App },
  template: "<App/>",
});
```

以上代码便完成了 `Element` 的引入。需要注意的是，样式文件需要单独引入。

3.验证`elementUI`框架是否引入成功

在`components`文件夹下插入`elementUI`框架样式

```html
<el-button type="primary">主要按钮</el-button>
```

![vue引入elementUI框架](./images/1541474252210-a58efad5-19a5-474f-a442-d167129111d6.jpeg)

4.启动项目
`npm run dev`

浏览器打开网址`http://localhost:8080`,显示按钮如下

证明引入成功

![vue引入elementUI框架](./images/1541474476111-2890bba1-d879-4b12-93a1-1925d90c2dcb.jpeg)
