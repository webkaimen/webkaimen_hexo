---
title: element-ui表单验证遇到v-if时不生效
author: Robbin
banner_img: /images/common/default_index_img.jpg
index_img: /images/common/vue.jpeg
wordcount: 300
date: 2022-03-28 11:53:46
updated: 2022-03-28 11:53:46
tags: ["#Vue.js", "#v-if", "#ElementUI", "#el-form-item"]
categories: Vue.js
---

![注册功能](./images/90711640-23CD-478f-B5C9-2089D716075A.png)
![注册功能](./images/90711640-23CD-478f-B5C9-2089D716075B.png)

<p class="note note-success"> 今天在项目中遇到一个问题，需要 v-if 渲染的 el-form-item 组件使用 prop 设置规则校验时不生效：
</p>

### 发现问题：校验规则不生效

`el-form-item` 组件使用 `prop` 设置规则校验时不生效

贴个没解决前的 gif
![注册功能](./images/90711640-23CD-478f-B5C9-2089D716075C.gif)

然后我尝试把注册页面改成默认展示,发现该问题不存在
![注册功能](./images/90711640-23CD-478f-B5C9-2089D716075D.gif)

### 解决办法

给该 `el-form-item` 设置一个 `key` 即可

```html
<template>
  <div class="login-register">
    <div class="contain">
      <div class="login-box" :class="{ active: isLogin }">
        <div class="login-contain" v-if="isLogin">
          <h1>登录</h1>
          <el-form
            :model="loginForm"
            status-icon
            :rules="rules"
            ref="loginForm"
            label-width="100px"
            class="login-form"
          >
            <el-form-item label="账号" prop="username">
              <el-input v-model.number="loginForm.username"></el-input>
            </el-form-item>
            <el-form-item label="密码" prop="password">
              <el-input
                type="password"
                v-model="loginForm.password"
                autocomplete="off"
              ></el-input>
            </el-form-item>
            <el-form-item
              ><el-button type="text">忘记密码?</el-button></el-form-item
            >

            <el-form-item style="text-align: center">
              <el-button
                style="width: 40%"
                type="primary"
                @click="submitForm('loginForm')"
                >登录</el-button
              >
            </el-form-item>
            <el-form-item>
              <el-divider>社交账号直接登录</el-divider>
              <el-button>微信</el-button>
              <el-button>QQ</el-button>
              <el-button>GitHub</el-button>
              <el-button>Gitee</el-button>
            </el-form-item>
          </el-form>
        </div>
        <div class="login-contain" v-else>
          <h1>注册</h1>
          <el-form
            :model="loginForm"
            status-icon
            :rules="rules"
            ref="loginForm"
            label-width="100px"
            class="login-form"
            :selfUpdate="true"
          >
            <el-form-item label="账号" prop="username">
              <el-input v-model.number="loginForm.username"></el-input>
            </el-form-item>
            <el-form-item label="密码" prop="password">
              <el-input
                type="password"
                v-model="loginForm.password"
                autocomplete="off"
              ></el-input>
            </el-form-item>
            <el-form-item label="确认密码" key="checkPass" prop="checkPass">
              <el-input
                type="password"
                v-model="loginForm.checkPass"
                autocomplete="off"
              ></el-input>
            </el-form-item>
            <el-form-item style="text-align: center">
              <el-button
                style="width: 40%"
                type="primary"
                @click="registerSubmitForm('loginForm')"
                >提交</el-button
              >
            </el-form-item>
          </el-form>
        </div>
      </div>
      <div class="small-box" :class="{ active: isLogin }">
        <div class="small-contain" v-if="isLogin">
          <div class="stitle">你好，朋友!</div>
          <p class="scontent">开始注册，和我们一起旅行</p>
          <el-button style="width: 60%" @click="changeType" round
            >注册</el-button
          >
        </div>
        <div class="small-contain" v-else>
          <div class="stitle">欢迎回来!</div>
          <p class="scontent">与我们保持联系，请登录你的账户</p>
          <el-button style="width: 60%" @click="changeType" round
            >登录</el-button
          >
        </div>
      </div>
    </div>
  </div>
</template>
```

![注册功能](./images/90711640-23CD-478f-B5C9-2089D716075E.gif)

完美解决

### 总结

因为 v-if 在两个相同表单或组件且没有 `key` 标识唯一性的情况下，会复用已有元素而不是从头开始渲染

### demo 地址

<!-- [login-register-demo](https://gitee.com/Robbinweb/login-register-demo) -->

<a href="https://gitee.com/Robbinweb/login-register-demo" target="_blank">login-register-demo</a>
