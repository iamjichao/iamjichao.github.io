---
layout: post
title: 一次搞懂 JSON.stringify
categories: [一次搞懂系列, JavaScript]
description: 一次搞懂 JSON.stringify
keywords: [JavaScript]
---

## 前言

先看一个面试题：实现一个对象的 map 函数，类似 `Array.prototype.map`，先看答案，然后带着问题往下看。

```jsx
Object.prototype._map = function (fn, oThis = null) {
  if (typeof fn !== 'function') {
    throw new TypeError(`${fn} is not a function!`);
  }
  return JSON.parse(JSON.stringify(this, (key, val) => {
    if (key) {
      return fn.call(oThis, key, val, this);
    } else {
      return val;
    }
  }));
}

const obj = { a: 2, b: 3, c: 4, d: 5 };

const _obj = obj._map((key, val, o) => {
  return ++val;
});

console.log(_obj); // {a: 3, b: 4, c: 5, d: 6}
```

这里用到了 `JSON.stringify()` 方法。通常情况下，我们对它的使用往往局限在对 Object 的序列化，或者使用它对 Object 进行深拷贝。这次我们一次搞懂 `JSON.stringify()` 的所有用法。

## JSON.stringify()

`JSON.stringify()` 方法将一个 JavaScript 对象或值转换为 JSON 字符串，如果指定了一个 replacer 函数，则可以选择性地替换值，或者指定的 replacer 是数组，则可选择性地仅包含数组指定的属性。

```jsx
JSON.stringify(value[, replacer [, space]])
```

## 九个特性

先看第一个参数。如果只传一个参数，`JSON.stringify()` 将第一个参数的值转换为相应的 JSON 格式，有以下九个特性：

1. 转换值如果有 `toJSON()` 方法，序列化的结果就是这个方法的返回值，忽略其他属性的值。
2. `Date` 对象包含 `toJSON()` 方法（同 `Date.toISOString()`），因此会被当做字符串处理。

```jsx
/** 1 **/
JSON.stringify({
  name: "Li Lei",
  toJSON: function() {
    return "Han Meimei";
  }
})
// '"Han Meimei"'

/** 2 **/
JSON.stringify(new Date());
// '"2020-12-05T09:11:20.566Z"'
JSON.stringify({ now: new Date() });
// '{"now":"2020-12-05T09:11:20.566Z"}'
```

3. **函数**、**undefined** 以及 **symbol** 单独序列化时会转换为 `undefined`，作为数组元素序列化时会转换为 `null`，作为对象的属性值序列化时会被忽略。
4. 当 **symbol** 作为对象的**属性键**序列化时，该属性会被完全忽略掉，即使 `replacer` 参数中强制指定了包含它（`replacer` 参数我们稍后会说到），序列化时也会忽略。
5. 因为非数组对象序列化时会忽略一些特殊的值，所以对象的属性不能保证以特定的顺序出现在序列化后的字符串中，数组除外。

```jsx
/** 3 **/
JSON.stringify(function(){});
JSON.stringify(() => {});
JSON.stringify(undefined);
JSON.stringify(Symbol('stringify'));
// undefined

/** 3, 5 **/
JSON.stringify({
  a: "stringify",
  b: undefined,
  c: Symbol("stringify"),
  d: function() {}
});
// '{"a":"stringify"}'

JSON.stringify(["stringify", undefined, Symbol("stringify"), function(){}]);
// '["stringify",null,null,null]'

/** 4 **/
JSON.stringify({ [Symbol("json")]: "stringify" }, function(k, v) {
  if (typeof k === "symbol") {
    return v;
  }
});
// undefined
```

6. **NaN**、**Infinity** 格式的数值以及 **null** 都会被当做 `null`，单独序列化时会转换为字符串 `"null"`，作为数组元素以及对象的属性值序列化时都会转换为 `null`。对 `BigInt` 类型（大于 2^53-1 的任意大的整数）的值序列化时会抛出 `Uncaught TypeError: Do not know how to serialize a BigInt` 。

```jsx
/** 6 **/
JSON.stringify(NaN);
JSON.stringify(Infinity);
JSON.stringify(null);
// "null"

JSON.stringify({
  a: NaN,
  b: Infinity,
  c: null
});
// '{"a":null,"b":null,"c":null}'

JSON.stringify([NaN, Infinity, null]);
// '[null,null,null]'

JSON.stringify(1n);
// Uncaught TypeError: Do not know how to serialize a BigInt
```

7. **布尔值**、**数字**、**字符串**的包装对象作为数组元素以及对象的属性值序列化时会自动转换成对应的原始值，单独序列化时会转化为对应的字符串。

```jsx
/** 7 **/
JSON.stringify(new Number(1));
// '1'
JSON.stringify(new String("false"));
// '"false"'
JSON.stringify(new Boolean(false));
// 'false'

JSON.stringify({
	a: new Number(1),
	b: new String("false"),
	c: new Boolean(false)
});
// '{"a":1,"b":"false","c":false}'

JSON.stringify([new Number(1), new String("false"), new Boolean(false)]);
// '[1,"false",false]'
```

8. 其他类型的对象，包括 **Map**/**Set**/**WeakMap**/**WeakSet**，仅会序列化可枚举的属性。

```jsx
/** 8 **/
JSON.stringify(
  Object.create(null, {
    a: { value: 'json', enumerable: false },
    b: { value: 'stringify', enumerable: true }
  })
);
// '{"b":"stringify"}'
```

9. 对包含循环引用的对象（对象之间相互引用，形成无限循环）执行此方法，会抛出错误。当我们试图通过 `JSON.parse(JSON.stringify())` 方法实现深拷贝时，要注意对象不能包含循环引用，否则会报错。

```jsx
/** 9 **/
// Uncaught TypeError: Converting circular structure to JSON
```

## 第二个参数

接下来看第二个和第三个参数。两个参数都是可选的。

第二个参数 `replacer` 可以是一个函数，也可以是一个数组。

- 当它是一个函数时，它有两个参数，键（key）和值（value），被序列化的值的每个属性都会经过该函数的处理，类似于数组方法 `map`、`filter` 等方法的回调函数。
- 当它是一个数组时，只有数组中包含的属性名才会被序列化到 JSON 字符串中。
- 当它是 null 或者未传时，对象所有的属性都会被序列化。

通过 `replacer` 参数，我们可以打破上述九个特性中的一些特性。

```jsx
const data = {
  a: "stringify",
  b: undefined,
  c: Symbol("stringify"),
  d: function() {}
};

// 不用 replacer 参数时
JSON.stringify(data);
// '{"a":"stringify"}'

// 使用 replacer 参数时
JSON.stringify(data, (key, value) => {
  switch (true) {
    case typeof value === "undefined":
      return "undefined";
    case typeof value === "symbol":
      return value.toString();
    case typeof value === "function":
      return value.toString();
    default:
      break;
  }
  return value;
});
// '{"a":"stringify","b":"undefined","c":"Symbol(stringify)","d":"function() {}"}'
```

需要注意的是，`replacer` 传入函数时，第一个参数不是对象的第一个键值对，而是空字符串作为 key 值，value 值是整个对象的键值对：

```jsx
const data = { a: 1, b: 2, c: 3 };
JSON.stringify(data, (key, value) => {
  console.log(value);
  return value;
});
// 第一个传入 replacer 函数的是 {"":{a:1,b:2,c:3}}
// { a: 1, b: 2, c: 3 }   
// 1
// 2
// 3
```

这时候我们再返回开头看看实现 Object.map 的方法，是不是恍然大悟呢？

当 `replacer` 是一个数组时，很简单，只会序列化数组中包含的属性值。

```jsx
const data = { a: "JSON", b: "stringify" };

// 只保留 b 属性的值
JSON.stringify(data, ["b"]);
// '{"b":"stringify"}'
```

## 第三个参数

第三个参数 `space` 指定缩进用的空白字符串，用于美化输出，它可以是数字，也可以是字符串。这个参数用处不大。

- 如果参数是数字，代表缩进有多少个空格，最大是 10，小于 1 则没有空格。
- 如果参数是字符串，那么这个字符串将作为缩进的空格，当字符串超过 10 个字母时，取它的前 10 个字母。
- 如果参数没有或者为 null，将没有空格。

```jsx
const data = { a: "JSON", b: "stringify" };

// 缩进有 12 个空格
JSON.stringify(data, null, 12);
// '{\n          "a": "JSON",\n          "b": "stringify"\n}'

// 用'a'代替缩进的空格
JSON.stringify(data, null, 'a');
// '{\na"a": "JSON",\na"b": "stringify"\n}'
```

## 举一反三

### JSON 对象

`JSON` 是一种语法，用来序列化对象、数组、数值、字符串、布尔值和 `null` 。`JSON` 对象包含两个方法：`parse` 和 `stringify`。除了这两个方法，`JSON` 这个对象本身并没有其他作用，也不能被调用或者作为构造函数调用。

`JSON` 基于 JavaScript 语法，但 JSON 不是 JavaScript，JavaScript 也不是 JSON。

- 对象和数组：属性名称必须是双引号括起来的字符串；最后一个属性后不能有逗号。
- 数值：不能出现前导零；如果有小数点，后面至少跟着一位数字。
- 字符串：字符串必须用双引号括起来。

### JSON.parse()

`JSON.parse()` 方法用来解析 JSON 字符串，构造由字符串描述的 JavaScript 值或对象。

#### 语法

```jsx
JSON.parse(text[, reviver])
```

#### 参数

- **`text`**

要被解析成 JavaScript 值的字符串，必须符合 JSON 的语法格式。

- **`reviver` 可选**

转换器函数，如果传入该函数，可以用来修改解析生成的原始值，在 parse 返回之前调用。这个函数的遍历顺序从最内层开始，按照层级顺序，依次向外遍历，当遍历到最顶层的解析值时，传入 `reviver` 函数的参数是空字符串和当前的解析值（有可能已经被修改过了）。

```jsx
JSON.parse('{"p": 5}', function (k, v) {
  if(k === '') return v;
  return v * 2;
});
// { p: 10 }
```

#### 返回值

- Object 类型，对应给定 JSON 文本的对象/值。

#### 异常

- 若传入的字符串不符合 JSON 规范，则会抛出 SyntaxError 异常。

```jsx
// 不允许用逗号作为结尾
JSON.parse("[1, 2, 3, 4, ]");
JSON.parse('{"foo" : 1, }');
```

以上。

## 参考资料

- [JSON.stringify() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)
- [JSON - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [JSON.parse() - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)
- [你不知道的 JSON.stringify() 的威力](https://github.com/NieZhuZhu/Blog/issues/1)
