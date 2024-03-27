---
layout: post
title: 一次搞懂作用域和闭包
categories: [一次搞懂系列, JavaScript]
description: 一次搞懂作用域和闭包
keywords: [JavaScript, 作用域, 闭包]
topmost: true
---

> 对于那些有一点 JavaScript 使用经验但从未真正理解闭包概念的人来说，理解闭包可以看作是某种意义上的重生，但需要付出非常多的努力和牺牲才能理解这个概念。

> 闭包并不是一个需要学习新的语法或模式才能使用的工具，它也不是一件必须接受像 Luke 一样的原力训练才能使用和掌握的武器。理解闭包就好像 Neo 第一次见到矩阵一样。

希望这篇文章可以像 Morpheus 一样，引导你去发现闭包这个神秘的矩阵。那么你选择蓝色药丸还是红色药丸？

## 开胃小菜

### JS 的编译

1. 传统编译语言的编译步骤：词法分析、语法分析、代码生成。
2. JS 虽然是一种解释型语言，是一门编译语言，代码片段在执行前（几微秒）进行编译。
3. 参与 JS 编译的角色：
- 引擎：负责整个 JS 的编译和执行过程
- 编译器：负责语法分析和代码生成
- 作用域：相当于一个容器，负责收集并维护所有标识符（变量、函数），确定代码对标识符的访问权限

### 词法作用域

1. 作用域的模型有词法作用域和动态作用域，词法作用域就是定义在词法阶段的作用域。JS 采用的是词法作用域模型，编译的词法分析阶段会确定代码中全部的标识符在哪个作用域以及是如何声明的，从而预测执行时应该如何查找。
2. JS中，除全局作用域外，函数声明以及代码块也会创建一个新的作用域，对应的词法作用域是由书写代码时函数声明或代码块的位置来决定的。作用域内声明的所有变量都会附属于这个作用域。
3. 作用域发生嵌套时，查找会从运行时所处的作用域开始，逐级向上进行，直到查到第一个匹配的标识符为止。

```jsx
function Matrix() {
  const name = 'Neo';
  {
    const name = 'Morpheus';
    console.log(name);
  }
  console.log(name);
}

Matrix();
// 'Morpheus'
// 'Neo'
```

### 函数作用域

1. 函数声明把变量和函数包裹起来，变成属于自己作用域的私有变量或函数，这遵循了软件设计中的最小授权（最小暴露）原则，可以规避因命名冲突导致的变量值被覆盖。
2. 函数作用域的含义是，**属于这个函数的全部变量都可以在整个函数的范围内（包括内部嵌套的作用域）使用**。

### 块作用域

1. ES6 中新增的块作用域可以将代码在块中隐藏，是对最小授权原则的扩展。
2. 如果使用 var 在块作用域中声明变量，会被提升到外部作用域，并不能把变量隐藏在块作用域中，使用 es6 的 const/let 进行声明可以将变量绑定到所在的作用域中，不会被提升。

![image](https://fehub.net/images/posts/2021/scope-closure-1.png)

1. 常见的块作用域：
- for 循环
- if 语句
- with
- try/catch 中的 catch
- {…}

### LHS 和 RHS 查询

1. LHS 查询是找到变量的容器，进而赋值（赋值操作的目标）

```jsx
var a = 1;
```

- 编译器判断变量 a 是否声明过，没有则在当前作用域下声明，有则忽略。这一步是在代码执行前进行。
- 运行阶段，引擎通过 LHS 查询在作用域中找到变量 a。
- 将 1 赋给该变量。

```jsx
// 相当于两个语句
var a;
a = 1;
```

1. RHS 查询是找到变量的值（赋值操作的源头）

```jsx
console.log(a);
```

- 对 a 进行 RHS 查询，取得 a 的值。
- 将值传递给 console.log(a)。
1. LHS 和 RHS 查询都会从当前作用域开始查找，一级一级往上层作用域查找，找到则停止，一直到全局作用域。如果到全局作用域还没有找到，RHS 查询会抛出 `ReferenceError` 异常，LHS 查询在非严格模式下会自动创建一个全局变量，而在严格模式下也会抛出 `ReferenceError` 异常。

![image](https://fehub.net/images/posts/2021/scope-closure-2.png)

### 变量提升

1. 所有的声明（变量和函数）都会被移动到各自作用域的最上面，这个过程被称为提升。
2. 变量声明提升：
- var 会提升
- const/let 不会提升
1. 函数声明提升：
- 函数声明会被提升到变量之前
- 函数表达式不会被提升

```jsx
console.log(a); // undefined
var a = 1;

console.log(b); // Uncaught ReferenceError: b is not defined
let b = 2;

c(); // 1
var c;

function c() {
  console.log(1);
}
c = function () {
  console.log(2);
}
```

## 鸡汤来喽

### 闭包到底是什么

> 闭包是基于词法作用域书写代码时所产生的自然结果。当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行。
> 

MDN 对 [闭包](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures) 的定义是：

> 闭包（closure）是一个函数以及其捆绑的周边环境状态（lexical environment，词法环境）的引用的组合。换而言之，闭包让开发者可以从内部函数访问外部函数的作用域。在 JavaScript 中，闭包会随着函数的创建而被同时创建。
> 

```jsx
function f1() {
  var a = 1;

  function f2() {
    console.log(a);
  }

  return f2;
}

var f = f1();
f(); // 1
```

上述代码中，`f2` 可以访问 `f1` 的内部作用域，`f1()` 返回内部的 `f2` 函数，然后赋值给变量 `f` 并调用，实际上是 `f2` 在定义时的词法作用域外被调用。

`f1` 执行后，通常情况下它的整个内部作用域会被引擎的垃圾回收机制销毁并释放内存。而在这段代码中，`f2` 作为 `f1()` 的返回值，`f1` 的内部作用域一直被 `f2` 使用，所以 `f1` 的作用域不会被回收，以供 `f2` 在之后的任意时间进行引用。

`f2` 始终保持对 `f1` 作用域的引用，这个引用和 `f2` 本身就组成了闭包。

总结一下：

- 无论通过何种方式将内部函数传递到所在的词法作用域以外，这个函数都会保持对原始作用域的引用，这样就形成了闭包。
- 无论在何处执行这个函数都会使用闭包，闭包使得函数可以继续访问定义时的词法作用域。

所以，**闭包 = 函数 + 外部作用域**。

我们的代码中其实到处都是闭包，只是我们没有发现。

### 函数柯里化

函数柯里化（curry）是函数式编程里面的概念。函数柯里化后，每次调用时只接受一部分参数，并返回一个函数，直到传递所有参数为止。

```jsx
// 柯里化之前
function add(a, b) {
  return a + b;
}
add(1, 2); // 3

// 柯里化之后
function Add (a) {
  return function (b) {
    return a + b;
  }
}
Add(1)(2); // 3
```

### 回调函数 Callback

- 定时器

```jsx
function wait(msg) {
  setTimeout(function () {
    console.log(msg);
  }, 1000);
}
wait('May the force be with you.');
```

- 事件监听器
- 数组常用方法 forEach、map…
- promise
- Ajax 请求

### 模块

模块是一个公共函数调用后返回内部私有函数和变量引用的一种代码模式。

```jsx
function Matrix() {
  var name1 = 'Neo';
  var name2 = 'Trinity';

  function Neo() {
    console.log(name1);
  }

  function Trinity() {
    console.log(name2);
  }

  return {
    Neo,
    Trinity,
  };
}

var m = Matrix();

m.Neo(); // 'Neo'
m.Trinity(); // 'Trinity'
```

模块模式需要具备两个必要条件：

- 必须有外部的包装函数，该函数必须至少被调用一次，每次调用都会创建一个新的模块实例。
- 包装函数必须返回至少一个内部函数，这样就会创建涵盖整个包装函数内部作用域的闭包。返回的内部函数就是模块的 API。

ES6 之前的模块使用，以 jQuery 为例，我们使用 script 标签引入 jQuery 模块后，就可以直接使用模块中暴露的 `jQuery` `$` 等标识符。

除此之外，还可以依赖于模块加载器，比如基于 AMD （异步模块定义）实现的 [RequireJS](https://requirejs.org/)，提供了 `require` `define` 方法用于引入模块和定义模块，定义模块的核心概念是这样的：

```jsx
var Modules = (function () {
  var modules = {};

  function define(name, deps, impl) {
    for (var i = 0; i < deps.length; i++) {
      deps[i] = modules[deps[i]];
    }
    modules[name] = impl.apply(impl, deps);
  }

  function get(name) {
    return modules[name];
  }

  return {
    define,
    get,
  };
})();
```

这里的包装函数是一个立即执行函数，执行后返回了包装函数内部定义的 `define` `get` 函数，这两个函数一直引用着包装函数的内部作用域，这样就产生了闭包。

ES6 为模块增加了语法支持，模块必须在独立的文件中定义，即一个文件一个模块，ES6 会将文件当作独立的模块来处理。每个模块可以导入其他模块的 API，也可以导出自己的 API。

```jsx
import Call from 'call.js';

const name = 'Mr.Anderson';

function Smith() {
  Call(name);
}

export Smith;
```

可以把整个文件看作是一个包装函数，导出的 `Smith` 函数一直引用着包装函数的内部作用域，形成闭包。

总结一下：

- ES6 之前的模块是基于函数的模块，通过 RequireJS 这类模块加载器可以实现模块的异步加载。
- ES6 模块是基于文件的模块，因为是语法层面的支持，浏览器默认的模块加载器就可以异步加载模块文件。

关于 JS 模块化，可以参考 MDN [JavaScript 模块](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Modules)。

![image](https://fehub.net/images/posts/2021/scope-closure-3.png)

## 饭后甜点

- 下面这段代码的执行结果是什么？

```jsx
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

- 回调函数的本质是闭包吗？
- 创建一个函数就会形成闭包吗？

# 参考资料

- [JavaScript 到底是解释型语言还是编译型语言](https://blog.csdn.net/qq_38836118/article/details/98878286/)
- [闭包 - MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)