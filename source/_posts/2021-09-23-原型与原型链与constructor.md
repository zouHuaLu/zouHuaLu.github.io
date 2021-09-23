---
title: 原型与原型链与constructor
date: 2021-06-18 15:16:53
tags: 
    - [JavaScript]
    - [原型]
    - [原型链]
categories: 技术类
---
# 原型与原型链与constructor

本文参考自：[https://blog.csdn.net/cc18868876837/article/details/81211729](https://blog.csdn.net/cc18868876837/article/details/81211729)

## 先来个总结：

1.  我们需要牢记两点：①`__proto__`和`constructor`属性是对象所独有的；② `prototype`属性是函数所独有的，因为函数也是一种对象，所以函数也拥有`__proto__`和`constructor`属性。

2.  __`proto__`属性的作用就是当访问一个对象的属性时，如果该对象内部不存在这个属性，那么就会去它的`__proto__`属性所指向的那个对象（父对象）里找，一直找，直到`__proto__`属性的终点null，再往上找就相当于在null上取值，会报错。通过`__proto__`属性将对象连接起来的这条链路即我们所谓的原型链。

3.  `prototype`属性的作用就是让该函数所实例化的对象们都可以找到公用的属性和方法，即`f1.__proto__ === Foo.prototype`。

4.  `constructor`属性的含义就是指向该对象的构造函数，所有函数（此时看成对象了）最终的构造函数都指向`Function`。

![整体的联系](https://upload-images.jianshu.io/upload_images/13931286-5467dbdb42fd91ef?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

## 基础知识

`__proto__`翻译为**原型**，多个`__proto__`串连起来的叫做**原型链**。

`prototype`翻译为**原型对象**。

## Object.getPrototypeOf()

`Object.getPrototypeOf()` 方法返回指定对象的原型（内部`[[Prototype]]`属性的值）。

### 示例
```
const prototype1 = {};
const object1 = Object.create(prototype1);

console.log(Object.getPrototypeOf(object1) === prototype1);
// expected output: true
Object.getPrototypeOf(object1) === object1.__proto__
```

## 正式开始

```
function Foo() {...};
let f1 = new Foo();
```

以上代码表示创建一个构造函数Foo()，并用new关键字实例化该构造函数得到一个实例化对象f1。这里稍微补充一下new操作符将函数作为构造器进行调用时的过程：**函数被调用，然后新创建一个对象，并且成了函数的上下文（也就是此时函数内部的this是指向该新创建的对象，这意味着我们可以在构造器函数内部通过this参数初始化值），最后返回该新对象的引用。**

## `__proto__`属性

首先要记住两点：

1.  `__proto__`和`constructor`是对象才有的属性，在JavaScript中函数也是一种对象。

2.  `prototype`是函数才有的属性。

![__proto__](https://upload-images.jianshu.io/upload_images/13931286-9355e730b53d1879?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

这里我们仅留下 `__proto__` 属性，它是对象所独有的，可以看到`__proto__`属性都是由**一个对象指向一个对象**，即指向它们构造函数的**原型对象**（也可以理解为父对象），那么这个属性的作用是什么呢？它的**作用就是当访问一个对象的属性时，如果该对象内部不存在这个属性，那么就会去它的`__proto__`属性所指向的那个对象（可以理解为父对象）里找，如果父对象也不存在这个属性，则继续往父对象的`__proto__`属性所指向的那个对象（可以理解为爷爷对象）里找，如果还没找到，则继续往上找…直到原型链顶端null（可以理解为原始人。。。），再往上找就相当于在null上取值，会报错（可以理解为，再往上就已经不是“人”的范畴了，找不到了，到此结束，null为原型链的终点）**，由以上这种通过`__proto__`属性来连接对象直到null的一条链即为我们所谓的原型链。    其实我们平时调用的字符串方法、数组方法、对象方法、函数方法等都是靠`__proto__`继承而来的。

## `prototype`属性

![prototype属性](https://upload-images.jianshu.io/upload_images/13931286-a68d7e7851adf580?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

`prototype`属性是函数独有的。他是从一个函数指向一个对象，他的含义是函数的原型对象

`f1.__proto__ === Foo.prototype`，它们两个完全一样。那prototype属性的作用又是什么呢？**它的作用就是包含可以由特定类型的所有实例共享的属性和方法，也就是让该函数所实例化的对象们都可以找到公用的属性和方法。**任何函数在创建的时候，其实会默认同时创建该函数的prototype对象。

## constructor属性

![constructor属性](https://upload-images.jianshu.io/upload_images/13931286-8de481f4fbc04fee?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

`constructor`属性也是对象才拥有的，它是从一个对象指向一个函数，含义就是指向该对象的构造函数，每个对象都有构造函数（本身拥有或继承而来，继承而来的要结合`__proto__`属性查看会更清楚点，如下图所示），从上图中可以看出`Function`这个对象比较特殊，它的构造函数就是它自己（因为`Function`可以看成是一个函数，也可以是一个对象），所有函数和对象最终都是由`Function`构造函数得来，所以`constructor`属性的终点就是`Function`这个函数。

`函数创建的对象.__proto__ === 该函数.prototype，该函数.prototype.constructor===该函数本身`

## 完~

