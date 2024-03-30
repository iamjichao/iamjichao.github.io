---
layout: post
title: 一次搞懂原型和原型链
categories: [一次搞懂系列, JavaScript]
description: 一次搞懂原型和原型链
keywords: [JavaScript, 原型, 原型链]
topmost: true
---

## 有图有真相

![image](https://fehub.net/images/posts/2021/prototype.jpg)

## 真想只有一个

- 构造函数 `Person`，通过 `new` 实例化它的一个实例 `person`，实例 `person` 的原型就是构造函数 `Person` 的 `prototype` 属性，即 `Person.prototype`。

```js
function Person() {} // 构造函数
var person = new Person() // 实例
// person的原型 === Person.prototype
```

- 实例的原型 `Person.prototype` 通过 `constructor` 属性可以访问到构造函数 `Person`，也就是实例原型的 `constructor` 属性指向构造函数。

```js
Person.prototype.constructor === Person
```

- 实例 `person` 通过 `__proto__` 属性可以访问到实例 `person` 的原型，也就是说实例的 `__proto__` 属性指向实例的原型。`Object.getPrototypeOf()` 方法也可以返回指定对象的原型。

```js
person.__proto__ === Person.prototype
Object.getPrototypeOf(person) === Person.prototype
```

- 实例的原型 `Person.prototype` 本身是一个对象，也就是说它是内置构造函数 `Object` 的一个实例，那么 `Person.prototype` 也可以通过 `__proto__` 属性或者 `Object.getPrototypeOf()` 方法访问到它的原型。

```js
Person.prototype.__proto__ === Object.prototype
Object.getPrototypeOf(Person.prototype) === Object.prototype
```

- `Object.prototype` 是原型链的终点，所以通过 `__proto__` 属性或者 `Object.getPrototypeOf()` 方法访问它的原型时是 `null`。试想，如果 `Object.prototype` 也是一个对象，那么它就可以通过 `__proto__` 属性或者 `Object.getPrototypeOf()` 方法访问到它的原型 `Object.prototype`，这样就变成了自己指向自己，陷入死循环了。`Object.prototype` 是由 JS 引擎创建的，而不是由构造函数 `Object` 创建的，没有继承任何属性。

```js
Object.prototype.__proto__ === null
Object.getPrototypeOf(Object.prototype) === null
```

以上。
