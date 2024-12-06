---
title: 优雅的 JavaScript 个技巧
author: Robbin
banner_img: /images/common/default_index_img.jpg
index_img: /images/common/JavaScript.jpeg
date: 2022-03-08 10:52:13
updated: 2022-03-08 10:52:13
tags: ["#JavaScript", "#前端"]
categories: JavaScript
---

JavaScript 有很多很酷的特性，大多数初学者和中级开发人员都不知道。今天分享一些，我经常在项目中使用一些技巧。

<!-- more -->

### 1. 有条件地向对象添加属性

我们可以使用展开运算符号(`...`)来有条件地向 JS 对象快速添加属性。

```JavaScript
const condition = true;
const person = {
id: 1,
name: 'John Doe',
...(condition && { age: 16 }),
};
```

如果每个操作数的值都为 `true`，则 `&&` 操作符返回最后一个求值表达式。因此返回一个对象`{age: 16}`，然后将其扩展为 `person` 对象的一部分。
如果 `condition` 为 `false`，JavaScript 会做这样的事情:

```JavaScript
const person = {
id: 1,
name: 'Robbin',
...(false),
};
// 展开 `false` 对对象没有影响
console.log(person); // { id: 1, name: 'John Doe' }
```

### 2.检查属性是否存在对象中

可以使用 `in` 关键字来检查 JavaScript 对象中是否存在某个属性。

```JavaScript
const person = { name: 'Robbin', salary: 1000 };
console.log('salary' in person); // true
console.log('age' in person); // false

```

### 3.对象中的动态属性名称

使用动态键设置对象属性很简单。只需使用`['key name']`来添加属性:

```JavaScript
const dynamic = 'flavour';
var item = {
name: 'Robbin',
[dynamic]: '巧克力'
}
console.log(item); // { name: 'Robbin', flavour: '巧克力' }
```

同样的技巧也可用于使用动态键引用对象属性：

```JavaScript
const keyName = 'name';
console.log(item[keyName]); // returns 'Robbin'
```

### 4. 使用动态键进行对象解构

我们知道在对象解构时，可以使用 `:` 来对解构的属性进行重命名。但，你是否知道键名是动态的时，也可以解构对象的属性？

```JavaScript
const person = { id: 1, name: 'Robbin' };
const { name: personName } = person;
console.log(personName); // 'Robbin'
```

现在，我们用动态键来解构属性：

```JavaScript
const templates = {
'hello': 'Hello there',
'bye': 'Good bye'
};
const templateName = 'bye';

const { [templateName]: template } = templates;

console.log(template); // Good bye
```

### 5. 空值合并 `??` 操作符

当我们想检查一个变量是否为 `null` 或 `undefined` 时，??操作符很有用。当它的左侧操作数为 `null` 或 `undefined` 时，它返回右侧的操作数，否则返回其左侧的操作数。

```JavaScript
const foo = null ?? 'Hello';
console.log(foo); // 'Hello'

const bar = 'Not null' ?? 'Hello';
console.log(bar); // 'Not null'

const baz = 0 ?? 'Hello';
console.log(baz); // 0
```

在第三个示例中，返回 `0`，因为即使 `0` 在 JS 中被认为是假的，但它不是 `null` 的或 `undefined` 的。你可能认为我们可以用||算子但这两者之间是有区别的

你可能认为我们可以在这里使用 `||` 操作符，但这两者之间是有区别的。

```JavaScript
const cannotBeZero = 0 || 5;
console.log(cannotBeZero); // 5

const canBeZero = 0 ?? 5;
console.log(canBeZero); // 0
```

### 6.可选链 `?.`

我们是不是经常遇到这样的错误： `TypeError: Cannot read property 'foo' of null`。这对每一个毅开发人员来说都是一个烦人的问题。引入可选链就是为了解决这个问题。一起来看看：

```JavaScript
const book = { id:1, title: 'Title', author: null };

// 通常情况下，你会这样做
console.log(book.author.age) // throws error
console.log(book.author && book.author.age); // null

// 使用可选链
console.log(book.author?.age); // undefined
// 或深度可选链
console.log(book.author?.address?.city); // undefined
```

还可以使用如下函数可选链：

```JavaScript
const person = {
firstName: '前端',
lastName: 'Robbin',
printName: function () {
return `${this.firstName} ${this.lastName}`;
},
};
console.log(person.printName()); // '前端 Robbin'
console.log(persone.doesNotExist?.()); // undefined
```

### 7. 使用 `!!` 操作符

!! 运算符可用于将表达式的结果快速转换为布尔值(`true` 或 `false`):

```JavaScript
const greeting = 'Hello there!';
console.log(!!greeting) // true

const noGreeting = '';
console.log(!!noGreeting); // false
```

### 8. 字符串和整数转换

使用 `+` 操作符将字符串快速转换为数字:

```JavaScript
const stringNumer = '123';

console.log(+stringNumer); //123
console.log(typeof +stringNumer); //'number'
```

要将数字快速转换为字符串，也可以使用 `+` 操作符，后面跟着一个空字符串:

```JavaScript
const myString = 25 + '';

console.log(myString); //'25'
console.log(typeof myString); //'string'
```

这些类型转换非常方便，但它们的清晰度和代码可读性较差。所以实际开发，需要慎重的选择使用。

### 9. 检查数组中的假值

大家应该都用过数组方法：`filter`、`some`、`every`，这些方法可以配合 `Boolean` 方法来测试真假值。

```JavaScript
const myArray = [null, false, 'Hello', undefined, 0];

// 过滤虚值
const filtered = myArray.filter(Boolean);
console.log(filtered); // ['Hello']

// 检查至少一个值是否为真
const anyTruthy = myArray.some(Boolean);
console.log(anyTruthy); // true

// 检查所有的值是否为真
const allTruthy = myArray.every(Boolean);
console.log(allTruthy); // false
```

下面是它的工作原理。我们知道这些数组方法接受一个回调函数，所以我们传递 `Boolean` 作为回调函数。`Boolean` 函数本身接受一个参数，并根据参数的真实性返回 `true` 或 `false`。所以：

```JavaScript
myArray.filter(val => Boolean(val));
```

等价于：

```JavaScript
myArray.filter(Boolean);
```

### 10. 扁平化数组

在原型 Array 上有一个方法 `flat`，可以从一个数组的数组中制作一个单一的数组。

```JavaScript
const myArray = [{ id: 1 }, [{ id: 2 }], [{ id: 3 }]];

const flattedArray = myArray.flat();
//[ { id: 1 }, { id: 2 }, { id: 3 } ]
```

你也可以定义一个深度级别，指定一个嵌套的数组结构应该被扁平化的深度。例如：

```JavaScript
const arr = [0, 1, 2, [[[3, 4]]]];
console.log(arr.flat(2)); // returns [0, 1, 2, [3,4]]
```

### 11.Object.entries

大多数开发人员使用 `Object.keys` 方法来迭代对象。 此方法仅返回对象键的数组，而不返回值。 我们可以使用 `Object.entries` 来获取键和值。

```JavaScript
const person = {
name: 'Robbin',
age: 20
};

Object.keys(person); // ['name', 'age']
Object.entries(data); // [['name', 'Robbin'], ['age', 20]]
```

为了迭代一个对象，我们可以执行以下操作：

```JavaScript
Object.keys(person).forEach((key) => {
console.log(`${key} is ${person[key]}`);
});

// 使用 entries 获取键和值
Object.entries(person).forEach(([key, value]) => {
console.log(`${key} is ${value}`);
});

// name is Robbin
// age is 20
```

上述两种方法都返回相同的结果，但 `Object.entries` 获取键值对更容易。

### 12.replaceAll 方法

在 JS 中，要将所有出现的字符串替换为另一个字符串，我们需要使用如下所示的正则表达式：

```JavaScript
const str = 'Red-Green-Blue';

// 只规制第一次出现的
str.replace('-', ' '); // Red Green-Blue

// 使用 RegEx 替换所有匹配项
str.replace(/\-/g, ' '); // Red Green Blue
```

但是在 ES12 中，一个名为 `replaceAll` 的新方法被添加到 `String.prototype` 中，它用另一个字符串值替换所有出现的字符串。

```JavaScript
str.replaceAll('-', ' '); // Red Green Blue
```

### 13.数字分隔符

可以使用下划线作为数字分隔符，这样可以方便地计算数字中 0 的个数。

```JavaScript
// 难以阅读
const billion = 1000000000;

// 易于阅读
const readableBillion = 1000_000_000;

console.log(readableBillion) //1000000000
```

下划线分隔符也可以用于 BigInt 数字，如下例所示

```JavaScript
const trillion = 1000_000_000_000n;

console.log(trillion); // 10000000000
```

### 14.document.designMode

与前端的 JavaScript 有关，设计模式让你可以编辑页面上的任何内容。只要打开浏览器控制台，输入以下内容即可。

```JavaScript
document.designMode = 'on';
```

### 15.逻辑赋值运算符

逻辑赋值运算符是由逻辑运算符`&&`、`||`、`??`和赋值运算符`=`组合而成。

```JavaScript
const a = 1;
const b = 2;

a &&= b;
console.log(a); // 2

// 上面等价于
a && (a = b);

// 或者
if (a) {
a = b
}

```

检查 `a` 的值是否为真，如果为真，那么更新 `a` 的值。使用逻辑或 `||`操作符也可以做同样的事情。

```JavaScript
const a = null;
const b = 3;

a ||= b;
console.log(a); // 3

// 上面等价于
a || (a = b);
```

使用空值合并操作符 ??:

```JavaScript
const a = null;
const b = 3;

a ??= b;
console.log(a); // 3

// 上面等价于
if (a === null || a === undefined) {
  a = b;
}
```

注意：`??`操作符只检查 `null` 或 `undefined` 的值。
