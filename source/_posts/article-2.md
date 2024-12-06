---
title: 7个常见的前端手写功能
author: Robbin
banner_img: /images/common/default_index_img.jpg
index_img: /images/common/JavaScript.jpeg
date: 2022-03-07 17:25:26
updated: 2022-03-07 17:25:26
tags: ["#JavaScript", "#前端", "#基础"]
categories: JavaScript
wordcount: 500
---

今天给大家带来的是 7 个常见的 JavaScript 手写功能，重要的地方已添加注释。有的是借鉴别人的，有的是自己写的，如有不正确的地方，欢迎多多指正。

<!-- more -->

### 1.防抖

```
function debounce(fn, delay) {
  let timer
  return function (...args) {
    if (timer) {
      clearTimeout(timer)
    }
    timer = setTimeout(() => {
      fn.apply(this, args)
    }, delay)
  }
}

// 测试
function task() {
console.log('run task')
}
const debounceTask = debounce(task, 1000)
window.addEventListener('scroll', debounceTask)

```

### 2.节流

```
function throttle(fn, delay) {
  let last = 0 // 上次触发时间
  return (...args) => {
    const now = Date.now()
    if (now - last > delay) {
      last = now
      fn.apply(this, args)
    }
  }
}

// 测试
function task() {
  console.log('run task')
}
const throttleTask = throttle(task, 1000)
window.addEventListener('scroll', throttleTask)

```

### 3.深拷贝

```
function deepClone(obj, cache = new WeakMap()) {
  if (obj === null || typeof obj !== 'object') return obj
  if (obj instanceof Date) return new Date(obj)
  if (obj instanceof RegExp) return new RegExp(obj)

  if (cache.get(obj)) return cache.get(obj) // 如果出现循环引用，则返回缓存的对象，防止递归进入死循环
  let cloneObj = new obj.constructor() // 使用对象所属的构造函数创建一个新对象
  cache.set(obj, cloneObj) // 缓存对象，用于循环引用的情况

  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      cloneObj[key] = deepClone(obj[key], cache) // 递归拷贝
    }
  }
  return cloneObj
}

// 测试
const obj = { name: 'Jack', address: { x: 100, y: 200 } }
obj.a = obj // 循环引用
const newObj = deepClone(obj)
console.log(newObj.address === obj.address) // false

```

### 4.异步控制并发数

```
function limitRequest(urls = [], limit = 3) {
  return new Promise((resolve, reject) => {
    const len = urls.length
    let count = 0

    // 同时启动limit个任务
    while (limit > 0) {
      start()
      limit -= 1
    }

    function start() {
      const url = urls.shift() // 从数组中拿取第一个任务
      if (url) {
        axios.post(url).then(res => {
          // todo
        }).catch(err => {
          // todo
        }).finally(() => {
          if (count == len - 1) {
            // 最后一个任务完成
            resolve()
          } else {
            // 完成之后，启动下一个任务
            count++
            start()
          }
        })
      }
    }

  })
}
```

### 5.数组排序

<p class="note note-success">sort 排序</p>

```
// 对数字进行排序，简写
const arr = [3, 2, 4, 1, 5]
arr.sort((a, b) => a - b)
console.log(arr) // [1, 2, 3, 4, 5]

// 对字母进行排序，简写
const arr = ['b', 'c', 'a', 'e', 'd']
arr.sort()
console.log(arr) // ['a', 'b', 'c', 'd', 'e']

```

<p class="note note-success">冒泡排序</p>

```
function bubbleSort(arr) {
  let len = arr.length
  for (let i = 0; i < len - 1; i++) {
    // 从第一个元素开始，比较相邻的两个元素，前者大就交换位置
    for (let j = 0; j < len - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        let num = arr[j]
        arr[j] = arr[j + 1]
        arr[j + 1] = num
      }
    }
    // 每次遍历结束，都能找到一个最大值，放在数组最后
  }
  return arr
}

//测试
console.log(bubbleSort([2, 3, 1, 5, 4])) // [1, 2, 3, 4, 5]

```

### 6.数组去重

<p class="note note-success">Set 去重</p>

```
const newArr = [...new Set(arr)]
// 或
const newArr = Array.from(new Set(arr))
```

<p class="note note-success">indexOf 去重</p>

```
function resetArr(arr) {
  let res = []
  arr.forEach(item => {
    if (res.indexOf(item) === -1) {
      res.push(item)
    }
  })
  return res
}

// 测试
const arr = [1, 1, 2, 3, 3]
console.log(resetArr(arr)) // [1, 2, 3]
```

### 7.获取 url 参数

<p class="note note-success">URLSearchParams 方法</p>
```
// 创建一个URLSearchParams实例
const urlSearchParams = new URLSearchParams(window.location.search);
// 把键值对列表转换为一个对象
const params = Object.fromEntries(urlSearchParams.entries());
```
<p class="note note-success">split 方法</p>

```
function getParams(url) {
  const res = {}
  if (url.includes('?')) {
    const str = url.split('?')[1]
    const arr = str.split('&')
    arr.forEach(item => {
      const key = item.split('=')[0]
      const val = item.split('=')[1]
      res[key] = decodeURIComponent(val) // 解码
    })
  }
  return res
}

// 测试
const user = getParams('https://www.baidu.com/?user=robbin&age=18')
console.log(user) // { user: 'robbin', age: '18' }

```

以上就是工作或求职中最常见的手写功能，你是不是全都掌握了呢!!!
