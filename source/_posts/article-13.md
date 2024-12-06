---
title: Express生成器快速搭建web服务器
author: Robbin
banner_img: /images/common/default_index_img.jpg
index_img: /images/common/node.jpeg
wordcount: 300
date: 2022-04-15 17:08:54
updated: 2022-04-15 17:08:54
tags: ["#Node.js", "#服务器", "#Express", "#生成器"]
categories: Node.js
---

<p class="note note-success"> 通过应用生成器工具 express-generator 可以快速创建一个应用的骨架。<br/>生成器创建的应用程序结构只是构建Express应用程序的众多方法之一，随意使用此结构或修改它以最好地满足你的需求。
</p>
<!-- more -->

[express 官网](https://www.expressjs.com.cn/starter/generator.html)

安装 express 生成器
`npm i -g express-generator`
使用 express 初始化项目结构
`express --no-view 项目名称`

![express生成器](./images/1650014324.jpg)

```

├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
│   ├── index.html
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.pug
    ├── index.pug
    └── layout.pug

7 directories, 10 files

```

进入项目目录
`cd server`
命令行中修改为新建项目下，并安装依赖
`npm i`
启动 express 服务
`npm run start`

然后在浏览器中打开`http://localhost:3000` 就可以打开这个项目了。

### 参考文档

[express 简介及使用 express-generator 快速搭建项目框架](https://blog.csdn.net/Charissa2017/article/details/105034452)
