---
title: 创建简单的Nodejs服务器
author: Robbin
banner_img: /images/common/default_index_img.jpg
index_img: /images/node.jpeg
wordcount: 300
date: 2022-03-04 11:16:39
updated: 2022-03-03 14:19:35
tags: ["#NodeJs", "#Express", "#后端"]
categories: nodejs
---

1.创建一个 package.json 文件
npm init

2.下载包，--sava 是用来记录并保存下载的包名称在 package.json 文件中
npm install 包名称 --save install 可简写成 i
如：npm install express --save #可简写形式 npm i express--save  
在别的地方用安装过得包，只要拷贝 package.json 文件放到要安装的路径下并运行第 1 步

3.1 express 必须 //--sav 是把下载的包名称保存到 package.json 文件中
npm install express --save

3.2 bodyParser 用于解析客户端请求的 body 中的内容,内部使用 JSON 编码处理,url 编码处理以及对于文件的上传处理.
npm i body-parser --save

3.3 mysql 用到数据库下载这个
npm install mysql --save

3.4 cors 跨域
npm i cors --save

下载完成后，在根目录下创建服务器

1. 创建 server.js 文件
   server.js 文件代码如下

```
// 引入结构件
const express = require("express");
const mysql = require("mysql");
const bodyParser = require("body-parser");
const cors = require("cors");

// 使用express构建web服务器
var app = express();
var server = app.listen(4000,()=>{
console.log("服务器启动成功,IP地址为:http://localhost:4000");
});

// 配置跨域模块，允许那个地址可以跨域访问
app.use(cors({
orign: ["http://127.0.0.1:8080",
"http://localhost:8080"],
credentials:true
}));

// 使用body-parser中间件
app.use(bodyParser.urlencoded({extended:false}));
app.get("/",(req,res)=>{
res.send("服务器启动成功");
})
```

然后在浏览器中访问自己设置的 IP 地址 localhost:4000

![node服务](./images/node-server.png)
