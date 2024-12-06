---
title: 【面试】说说typeof和instanceof的区别
author: Robbin
banner_img: /images/common/default_index_img.jpg
date: 2022-03-08 10:06:30
updated: 2022-03-08 10:06:30
tags: ["#面试", "#前端"]
categories: "面试"
index_img: /images/common/var.jpeg
comment: "valine"
---

## 背景

typeof 和 instanceof 操作符都是用来判断数据类型的，但是它们的使用场景却各不相同，其中一些细节也需要特别注意。接下来让我们一探究竟，彻底掌握该知识点，再也不惧面试官的提问。

<!-- more -->

### typeof

typeof 是一个一元运算符，放在一个运算数前面，这个运算数可以是任何类型。它返回一个字符串，说明运算数的类型。请看栗子：

```JavaScript
const type =  typeof '中国万岁'; // string
typeof 666; // number
typeof true; // boolean
typeof undefined; // undefined
typeof Symbol(); // symbol
typeof 1n; // bigint
typeof () => {}; // function

typeof []; // object
typeof {}; // object
typeof new String('xxx'); // object

typeof null; // object
```

通过以上例子可以看出，typeof 只能准确判断基本数据类型和函数（函数其实是对象，并不属于另一种数据类型，但也能够使用 typeof 进行区分），无法精确判断出引用数据类型（统统返回 object）。
有一点需要注意，调用 typeof null 返回的是 object，这是因为特殊值 null 被认为是一个对空对象的引用（也叫空对象指针）。
如果想准确判断引用数据类型，可以用 instanceof 运算符。

### instanceof

instanceof 运算符放在一个运算数的后面，对象的前面。它返回一个布尔值，说明运算数是否是某个对象的实例：

```JavaScript
const result = [] instanceof Array; // true

const Person = function() {};
const p = new Person();
p instanceof Person; // true

const message = new String('xxx');
message instanceof String; // true
```

### 区别

typeof 会返回一个运算数的基本类型，instanceof 返回的是布尔值

instanceof 可以准确判断引用数据类型，但是不能正确判断基本数据类型

typeof 虽然可以判断基本数据类型（null 除外），但是无法判断引用数据类型（function 除外）

### 扩展

Object.prototype.toString.call()
typeof 和 instanceof 都有一定的弊端，并不能满足所有场景的需求。如果需要通用检测数据类型，可以使用 Object.prototype.toString.call()方法：

```JavaScript
Object.prototype.toString.call({}); // "[object Object]"
Object.prototype.toString.call([]); // "[object Array]"
Object.prototype.toString.call(666); // "[object Number]"
Object.prototype.toString.call('xxx'); // "[object String]"
```

注意，该方法返回的是一个格式为"[object Object]"的字符串。
封装函数
为了更方便的使用，我们可以将这个方法进行封装：

```JavaScript
function getType(value) {
    let type = typeof value;
    if (type !== 'object') { // 如果是基本数据类型，直接返回
        return type;
    }
    // 如果是引用数据类型，再进一步判断，正则返回结果
    return Object.prototype.toString.call(value).replace(/^\[object (\S+)\]$/, '$1');
}

getType(123); // number
getType('xxx'); // string
getType(() => {}); // function
getType([]); // Array
getType({}); // Object
getType(null); // Null
```

### 最后

如果文中有错误或者不足之处，欢迎大家在评论区指正。
