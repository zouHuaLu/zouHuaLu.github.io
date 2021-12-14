---
title: JavaScript的同步与异步
date: 2021-12-14 15:10:10
tags: [JavaScript,面试]
categories: 技术类-前端
---

[更详细的可以点这里](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Asynchronous/Introducing)

## [异步JavaScript](https://developer.mozilla.org/zh-CN/docs/Glossary/Asynchronous)

**异步**指两个或两个以上的对象或事件**不**同时存在或发生（或多个相关事物的发生**无需**等待其前一事物的完成）

## [同步JavaScript](https://developer.mozilla.org/zh-CN/docs/Glossary/Synchronous)

各方都实时（或者尽可能实时）地收取（而且必要的话也处理或者回复）信息的即时沟通方式，即为**同步**。

电话即为一个日常的例子：人们都倾向于在使用电话时即时地作出回应。

许多程序指令也是实时的：例如当输入一个算式时，除非编程人员有意为止，否则环境都会立即将结果反馈回来。

## 事件队列

像`promise`这样的异步操作被放入事件队列中，事件队列在主线程完成处理后运行，这样它们就不会阻止后续JavaScript代码的运行。排队操作将尽快完成，然后将结果返回到JavaScript环境。

## Promises 对比 callbacks

promises与旧式callbacks有一些相似之处。它们本质上是一个返回的对象，您可以将回调函数附加到该对象上，而不必将回调作为参数传递给另一个函数。

然而，Promise是专门为异步操作而设计的，与旧式回调相比具有许多优点:

- 您可以使用多个then()操作将多个异步操作链接在一起，并将其中一个操作的结果作为输入传递给下一个操作。这种链接方式对回调来说要难得多，会使回调以混乱的“末日金字塔”告终。 (也称为回调地狱)。
- Promise总是严格按照它们放置在事件队列中的顺序调用。
- 错误处理要好得多——所有的错误都由块末尾的一个.catch()块处理，而不是在“金字塔”的每一层单独处理。

## 异步代码的本质

让我们研究一个示例，它进一步说明了异步代码的本质，展示了当我们不完全了解代码执行顺序以及将异步代码视为同步代码时可能发生的问题

```js
console.log ('Starting');
let image;

fetch('coffee.jpg').then((response) => {
  console.log('It worked :)')
  return response.blob();
}).then((myBlob) => {
  let objectURL = URL.createObjectURL(myBlob);
  image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}).catch((error) => {
  console.log('There has been a problem with your fetch operation: ' + error.message);
});

console.log ('All done!');
```

浏览器将会执行代码，看见第一个`console.log()` 输出`Starting` ，然后创建`image`变量。

然后，它将移动到下一行并开始执行`fetch()`块，但是，因为`fetch()`是异步执行的，没有阻塞，所以在`promise`相关代码之后程序继续执行，从而到达最后的`console.log()`语句`All done!`并将其输出到控制台。

只有当`fetch()` 块完成运行返回结果给`.then()`，我们才最后看到第二个`console.log()`消息 `It worked ;)`。所以 这些消息可能以和你预期不同的顺序出现：

- Starting
- All done!
- It worked :)

如果你感到疑惑，考虑下面这个小例子：

```js
console.log("registering click handler");

button.addEventListener('click', () => {
  console.log("get click");
});

console.log("all done");
```

这在行为上非常相似——第一个和第三个`console.log()`消息将立即显示，但是第二个消息将被阻塞，直到有人单击鼠标按钮。前面的示例以相同的方式工作，只是在这种情况下，第二个消息在`promise`链上被阻塞，直到获取资源后再显示在屏幕上，而不是单击。

要查看实际情况，并将第三个`console.log()`调用更改为以下命令：

```js
console.log ('All done! ' + image.src + 'displayed.');
```

此时控制台将会报错，而不会显示第三个 console.log 的信息：

```js
TypeError: image is undefined; can't access its "src" property
```

这是因为：浏览器运行第三个console.log()的时候，fetch() 语句块还没有完成，因此image还没有赋值。

## 小结

`JavaScript`是一种同步的、阻塞的、单线程的语言，在这种语言中，一次只能执行一个操作。

但web浏览器定义了函数和API，允许我们当某些事件发生时不是按照同步方式，而是异步地调用函数(比如，时间的推移，用户通过鼠标的交互，或者获取网络数据)。

这意味着您的代码可以同时做几件事情，而不需要停止或阻塞主线程。

### 异步还是同步执行代码，取决于我们要做什么

- 同步-如果我们希望事情能够立即加载并发生。例如，当将一些用户定义的样式应用到一个页面时，您希望这些样式能够尽快被应用。

- 异步-如果我们正在运行一个需要时间的操作，比如查询数据库并使用结果填充模板，那么最好将该操作从主线程中移开使用异步完成任务。

