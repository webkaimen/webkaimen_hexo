---
title: 18个杀手级JavaScript单行代码
author: Robbin
banner_img: /images/common/default_index_img.jpg
index_img: /images/common/JavaScript.jpeg
date: 2022-03-08 15:35:35
updated: 2022-03-08 15:35:35
tags: ["#前端", "#JavaScript"]
categories: JavaScript
---

### 1、复制到剪贴板

使用 navigator.clipboard.writeText 轻松将任何文本复制到剪贴板。

```javascript
const copyToClipboard = (text) => navigator.clipboard.writeText(text);
copyToClipboard("Hello World");
```

### 2、检查日期是否有效

使用以下代码段检查给定日期是否有效。

```javascript
const isDateValid = (...val) => !Number.isNaN(new Date(...val).valueOf());
isDateValid("December 17, 1995 03:24:00");
// Result: true
```

### 3、找出一年中的哪一天

查找给定日期的哪一天。

```javascript
const dayOfYear = (date) =>
  Math.floor((date - new Date(date.getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24);
dayOfYear(new Date());
// Result: 272
```

### 4、将首字符串大写

Javascript 没有内置的大写函数，因此我们可以使用以下代码。

```javascript
const capitalize = str => str.charAt(0).toUpperCase() + str.slice(1)capitalize("follow for more")// Result: Follow for more
```

### 5、找出两日期之间的天数

使用以下代码段查找给定 2 个日期之间的天数。

```javascript
const dayDif = (date1, date2) => Math.ceil(Math.abs(date1.getTime() - date2.getTime()) / 86400000)dayDif(new Date("2020-10-21"), new Date("2021-10-22"))// Result: 366
```

### 6、清除所有 Cookie

你可以通过使用 document.cookie 访问 cookie 并清除它来轻松清除存储在网页中的所有 cookie。

```javascript
const clearCookies = document.cookie
  .split(";")
  .forEach(
    (cookie) =>
      (document.cookie = cookie
        .replace(/^ +/, "")
        .replace(/=._/, `=;expires=${new Date(0).toUTCString()}; path=/`))
  );
```

### 7、生成随机十六进制

```javascript
你可以使用 Math.random 和 padEnd 属性生成随机十六进制颜色。
const randomHex = () => `#${Math.floor(Math.random() _ 0xffffff).toString(16).padEnd(6, "0")}`
console.log(randomHex());
//Result: #92b008
```

### 8、从数组中删除重复项

你可以使用 JavaScript 中的 Set 轻松删除重复项。

```javascript
const removeDuplicates = (arr) => [...new Set(arr)];
console.log(removeDuplicates([1, 2, 3, 3, 4, 4, 5, 5, 6]));
// Result: [ 1, 2, 3, 4, 5, 6 ]
```

### 9、从 URL 获取查询参数

你可以通过传递 window.location 或原始 URL goole.com?search=easy&page=3 从 url 轻松检索查询参数

```javascript
const getParameters = (URL) => {
  URL = JSON.parse(
    '{"' +
      decodeURI(URL.split("?")[1])
        .replace(/"/g, '"')
        .replace(/&/g, '","')
        .replace(/=/g, '":"') +
      '"}'
  );
  return JSON.stringify(URL);
};
getParameters(window.location); // Result: { search : "easy", page : 3 }
```

### 10、从日期记录时间

我们可以从给定日期以小时::分钟::秒的格式记录时间。

```javascript
const timeFromDate = (date) => date.toTimeString().slice(0, 8);
console.log(timeFromDate(new Date(2021, 0, 10, 17, 30, 0)));
// Result: "17:30:00"
```

### 11、检查数字是偶数还是奇数

```javascript
const isEven = (num) => num % 2 === 0;
console.log(isEven(2));
// Result: True
```

### 12、求数字的平均值

使用 reduce 方法找到多个数字之间的平均值。

```javascript
const average = (...args) => args.reduce((a, b) => a + b) / args.length;
average(1, 2, 3, 4);
// Result: 2.5
```

### 13、反转字符串

你可以使用 split、reverse 和 join 方法轻松反转字符串。

```javascript
const reverse = (str) => str.split("").reverse().join("");
reverse("hello world");
// Result: 'dlrow olleh'
```

### 14、检查数组是否为空

检查数组是否为空的简单单行程序将返回 true 或 false。

```javascript
const isNotEmpty = (arr) => Array.isArray(arr) && arr.length > 0;
isNotEmpty([1, 2, 3]);
// Result: true
```

### 15、获取选定的文本

使用内置的 getSelectionproperty 获取用户选择的文本。

```javascript
const getSelectedText = () => window.getSelection().toString();
getSelectedText();
```

### 16、打乱数组

使用 sort 和 random 方法打乱数组非常容易。

```javascript
const shuffleArray = (arr) => arr.sort(() => 0.5 - Math.random());
console.log(shuffleArray([1, 2, 3, 4])); // Result: [ 1, 4, 3, 2 ]
```

### 17、检测暗模式

使用以下代码检查用户的设备是否处于暗模式。

```javascript
const isDarkMode =
  window.matchMedia &&
  window
    .matchMedia("(prefers-color-scheme: dark)")
    .matchesconsole.log(isDarkMode); // Result: True or False
```

### 18、将 RGB 转换为十六进制

```JavaScript
const rgbToHex = (r, g, b) =>   "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1);rgbToHex(0, 51, 255); // Result: #0033ff
```
