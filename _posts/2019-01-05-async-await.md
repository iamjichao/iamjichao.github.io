---
layout: post
title: async/await 学习
categories: [JavaScript]
description: async/await 学习
keywords: [JavaScript, async, await]
---

使用 async / await，搭配 Promise，可以通过编写形似同步的代码来处理异步流程，提高代码的简洁性和可读性。

### async

async 方法比较简单，只需要在普通的函数前加上 async 关键字即可。async function 通过 `Promise.resolved()` 将返回值封装成一个 Promise 对象返回。所以可以通过 then 方法获取返回值。

```js
async function name([param[, param[, ... param]]]) { statements }
```

async function 总是返回一个 Promise 对象，即使代码中有 return <非 Promise> 的语句，也会自动把返回的这个 value 值包装成 Promise 对象 的 resolved 值。

以下两种方法可以输出相同的结果：

```js
async function helloAsync() {
  return 1;
}

helloAsync().then(res => {
  console.log(res);
});
```

```js
async function helloAsync() {
  return Promise.resolve(1);
}

helloAsync().then(res => {
  console.log(res);
});
```

### await

await 操作符用于等待一个 Promise 对象，**它只能在异步函数 async function 内部使用**（不能在常规函数里使用，不能工作在顶级作用域）。await 在等待 Promise 对象时会导致 async function 暂停执行，一直到 Promise 对象决议之后才会 async function 继续执行。

await 后面可以是字符串，布尔值，数值以及普通函数，也可以是具有可调用的 then 方法的对象。await 针对所跟的表达式不同，有两种处理方式：

- 对于 Promise 对象，await 会阻塞主函数的执行，等待 Promise 对象 resolve，然后得到 resolve 的值，作为 await 表达式的运算结果，然后继续执行主函数接下来的代码。

- 对于非 Promise 对象，await 等待函数或者直接量的返回，而不是等待其执行结果。

以下是一个 await Promise 的例子：

```js
async function helloAsync() {
  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve('done!'), 1000);
  });
  let result = await promise; // 直到 promise 返回一个 resolve 值
  console.log(result); // 1s 后打印 'done!'
}

helloAsync();
```

可以看出，await 是一个更优雅的得到 Promise 值的语句，它比 Promise 更加容易阅读和书写。

### 异常处理

Promise 对象有两种状态，除了 resolved，还有 rejected。如果 promise 对象变为 rejected，await 会中断后面的执行。如果想不被中断继续执行，可以采用 try catch 在函数内部捕获异常。

以下是一个异常处理的例子：

```js
function testAwait() {
  return Promise.reject('error');
}

async function helloAsync(){
  try{
    await testAwait();
  } catch(e) {
    // 打印 error
    console.log(e);
  }
}

helloAsync().then(() => {
  // 打印 success，说明 helloAsync 方法没有被中断
  console.log('success');
}).catch(e => {
  // 没有打印
  console.log(e);
});
```

如果我们不使用 try catch，调用 async 函数产生的 Promise 变成 reject 状态的话，我们可以添加 .catch 去处理它。

```js
async function helloAsync() {
  let res = await Promise.reject('error');
}

helloAsync().catch(err => {
  // TypeError: failed to fetch
  console.log(e);
});
```

总结一下：

当我们使用 async / await 时，我们很少需要 .then，因为 await 总是等待着最终结果，而且我们在 async 函数的内部能够使用常规的 try catch 捕获异常。

但是在代码的顶层，当我们在 async 函数的外部时，我们在语法上是不能使用 await 的，所以通常添加 .then / catch 去处理最终结果或者错误。

### 应用

以下是微信小程序云开发文档中关于分批次获取所有数据的一段代码，使用了 async / await 方法，没有各种回调，代码简洁易读。

```js
const cloud = require('wx-server-sdk')
cloud.init()
const db = cloud.database()
const MAX_LIMIT = 100
exports.main = async (event, context) => {
  // 先取出集合记录总数
  const countResult = await db.collection('todos').count()
  const total = countResult.total
  // 计算需分几次取
  const batchTimes = Math.ceil(total / 100)
  // 承载所有读操作的 promise 的数组
  const tasks = []
  for (let i = 0; i < batchTimes; i++) {
    const promise = db.collection('todos').skip(i * MAX_LIMIT).limit(MAX_LIMIT).get()
    tasks.push(promise)
  }
  // 等待所有
  return (await Promise.all(tasks)).reduce((acc, cur) => ({
    data: acc.data.concat(cur.data),
    errMsg: acc.errMsg,
  }))
}
```
