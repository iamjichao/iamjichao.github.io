---
layout: post
title: 事件循环：宏任务和微任务
categories: [JavaScript]
description: 事件循环：宏任务和微任务
keywords: [JavaScript, Event Loop, 宏任务, 微任务]
---

当 JavaScript 引擎执行 JavaScript 代码时，会按照先后顺序将任务分为两类：宏任务（Macro Task）和微任务（Micro Task），并将它们放入不同的任务队列中。

宏任务包括以下几种：

- script（整体代码）
- setTimeout
- setInterval
- I/O 操作
- UI 渲染（浏览器的渲染进程会单独维护一个 UI 渲染线程）

微任务包括以下几种：

- Promise.then
- MutationObserver（监视 DOM 变动的接口）
- process.nextTick（Node.js 环境中）

当一个宏任务执行完毕后，JavaScript 引擎会检查微任务队列中是否有任务，如果有，就会依次执行它们，直到微任务队列为空。然后再执行下一个宏任务，再依次检查微任务队列，以此类推。

通常情况下，微任务的执行优先级要高于宏任务，也就是说，当一个宏任务执行完成之后，如果有微任务的话，会优先执行微任务，然后再执行下一个宏任务。这样做的好处是可以确保异步代码执行的及时性和准确性。

下面是一个例子：

```js
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

Promise.resolve().then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});

console.log('script end');

// 输出顺序为:
// script start
// script end
// promise1
// promise2
// setTimeout
```

在这个例子中，首先执行同步代码，输出 `script start` 和 `script end`。然后当执行栈为空时，执行两个微任务，输出 `promise1` 和 `promise2`。最后当再次遍历宏任务时，输出 `setTimeout`。

综上所述，JavaScript 的事件循环机制是基于宏任务和微任务，事件循环的顺序是：

1. 执行同步代码（宏任务）。
2. 执行栈为空后，执行微任务，直到微任务队列为空。
3. 再次执行同步代码（宏任务）。
4. 重复步骤 2 和 3，直到没有微任务或者宏任务。

以上。
