---
title: 深入理解javascript之typeof和instanceof
date: 2021-10-13 16:41:28
tags: [JavaScript,面试]
categories: 技术类-前端
---

# typeof

typeof方法**返回一个字符串**，来表示**数据的类型**。

各个数据类型对应typeof的值：

| **数据类型**                       | **Type**                 |
| ---------------------------------- | ------------------------ |
| Undefined                          | “undefined”              |
| Null                               | "object"                 |
| Boolean                            | “boolean”                |
| Number                             | "number"                 |
| String                             | "string"                 |
| Symbol                             | "symbol"                 |
| 宿主对象(JS环境提供的，比如浏览器) | Implementation-dependent |
| 函数对象Function                   | "function"               |
| 任何其他对象Object                 | "object"                 |

下面是代码示例：

```js
// Numbers
typeof 37 === 'number';
typeof 3.14 === 'number';
typeof Math.LN2 === 'number';
typeof Infinity === 'number';
typeof NaN === 'number'; // 尽管NaN是"Not-A-Number"的缩写,意思是"不是一个数字"
typeof Number(1) === 'number'; // 不要这样使用!

// Strings
typeof "" === 'string';
typeof "bla" === 'string';
typeof (typeof 1) === 'string'; // typeof返回的肯定是一个字符串
typeof String("abc") === 'string'; // 不要这样使用!

// Booleans
typeof true === 'boolean';
typeof false === 'boolean';
typeof Boolean(true) === 'boolean'; // 不要这样使用!

// Symbols
typeof Symbol() === 'symbol';
typeof Symbol('foo') === 'symbol';
typeof Symbol.iterator === 'symbol';

// Undefined
typeof undefined === 'undefined';
typeof blabla === 'undefined'; // 一个未定义的变量,或者一个定义了却未赋初值的变量

// Objects
typeof {a:1} === 'object';

// 使用Array.isArray或者Object.prototype.toString.call方法可以从基本的对象中区分出数组类型
typeof [1, 2, 4] === 'object';

typeof new Date() === 'object';

// 下面的容易令人迷惑，不要这样使用！
typeof new Boolean(true) === 'object';
typeof new Number(1) ==== 'object';
typeof new String("abc") === 'object';

// 函数
typeof function(){} === 'function';
typeof Math.sin === 'function';
```

发现typeof来判断数据类型其实并不准确。比如数组、正则、日期、对象hj的typeof返回值都是object

# instanceof

instanceof运算符可以用来判断某个构造函数的prototype属性**是否存在于另外一个**要检测对象**的原型链上**。

```js
// 定义构造函数
function C(){} 
function D(){} 

var o = new C();

// true，因为 Object.getPrototypeOf(o) === C.prototype
o instanceof C; 

// false，因为 D.prototype不在o的原型链上
o instanceof D; 

o instanceof Object; // true,因为Object.prototype.isPrototypeOf(o)返回true
C.prototype instanceof Object // true,同上

C.prototype = {};
var o2 = new C();

o2 instanceof C; // true

o instanceof C; // false,C.prototype指向了一个空对象,这个空对象不在o的原型链上.

D.prototype = new C(); // 继承
var o3 = new D();
o3 instanceof D; // true
o3 instanceof C; // true
```


