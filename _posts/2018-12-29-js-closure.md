---
layout: post
title: 理解 JS 闭包
categories: [JavaScript]
description: 理解 JS 闭包
keywords: [JavaScript, 闭包]
---

JavaScript 中的函数运行在它们被定义的作用域里，而不是它们被执行的作用域里。

### 什么是闭包

有权访问另一个函数作用域内变量的函数叫闭包。

JS 的每个函数都是一个个小黑屋，它可以获取外界信息，但是外界却无法直接看到里面的内容。代码 1 中，将变量 n 放进小黑屋里，除了 inc 函数之外，没有其他办法能接触到变量 n，而且在函数 a 外定义同名的变量 n 也是互不影响的。而之所以要用 return 返回函数标识 inc，是因为在 a 函数外部无法直接调用 inc 函数，所以 return inc 与外部联系起来，代码 2 中的 this 也是将 inc 与外部联系起来而已。

```js
// 代码 1
function a() {
  var n = 0;
  function inc() {
    n++;
    console.log(n);
  }
  return inc;
}
var c = a();
c();  // 控制台输出 1
c();  // 控制台输出 2

// 代码 2
function a() {
  var n = 0;
  this.inc = function() {
    n++;
    console.log(n);
  };
}
var c = new a();
c.inc();  // 控制台输出 1
c.inc();  // 控制台输出 2
```

### 一个陷阱

```js
function createFunctions() {
  var result = new Array();
  for (var i = 0; i < 10; i++) {
    result[i] = function() {
      return i;
    };
  }
  return result;
}
var funcs = createFunctions();
for (var i = 0; i < funcs.length; i++) {
  console.log(funcs[i]());
}
```

乍一看，以为输出 0~9，万万没想到输出 10 个 10？这里的陷阱就是：函数带 () 才是执行函数！单纯的一句 var f = function() { alert('Hi'); } 是不会弹窗的，后面接一句 f() 才会执行函数内部的代码。上面的代码翻译一下就是：

```js
var result = new Array(), i;
result[0] = function(){ return i; };  // 没执行函数，函数内部不变，不能将函数内的 i 替换！
result[1] = function(){ return i; };  // 没执行函数，函数内部不变，不能将函数内的 i 替换！
...
result[9] = function(){ return i; };  // 没执行函数，函数内部不变，不能将函数内的 i 替换！
i = 10;
funcs = result;
result = null; // result 被垃圾回收，因为 i 还在被 function 引用着，所以没有回收

console.log(i);  // funcs[0]() 就是执行 return i 语句，就是返回 10
console.log(i);  // funcs[1]() 就是执行 return i 语句，就是返回 10
...
console.log(i);  // funcs[9]() 就是执行 return i 语句，就是返回 10
```

### 总结

闭包就是一个函数引用另外一个函数的变量，因为变量被引用着所以不会被回收，因此可以用来封装一个”私有变量“。这是优点也是缺点，不必要的闭包只会徒增内存消耗！

以上。
