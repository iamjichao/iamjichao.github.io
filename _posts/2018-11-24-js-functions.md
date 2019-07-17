---
layout: post
title: 常用的 JavaScript 方法
categories: JavaScript
description: 常用的 JavaScript 方法
keywords: JavaScript, 方法
---

### 获取url传递参数的方法
---

```js
function getUrlParam (name) {
  var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
  var r = window.location.search.substr(1).match(reg);
  if (r != null) return unescape(r[2]);
  return null;
}
```

### 获取元素的坐标
---

```js
// 获取元素的纵坐标（相对于窗口）
function getTop (obj) {
  var offset = obj.offsetTop;
  if (obj.offsetParent != null) offset += getTop(obj.offsetParent);
  return offset;
}
// 获取元素的横坐标（相对于窗口）
function getLeft (obj) {
  var offset = obj.offsetLeft;
  if (obj.offsetParent != null) offset += getLeft(obj.offsetParent);
  return offset;
}
```

### 手机号校验和替换中间4位
---

```js
// 手机号校验
var reg = /^[1][34587]\d{9}$/;
phonenum.match(reg);
reg.test(phonenum);
// 正则表达式中的括号即可用于分组，同时也用于定义子模式串，在replace()方法中，参数二中可以使用$n(n为数字)来依次引用模式串中用括号定义的字串
phonenum.replace(/(\d{3})\d{4}(\d{4})/, '$1****$2');
```

### 姓名显示最后一字
---

```js
var str = '王大锤';
// 方法一
return new Array(str.length).join('*') + str.substr(-1);
// 方法二
return str.replace(/.(?=.)/g, '*');
```

### 获取是否联网
---

```js
window.navigator.onLine
```

### 数组常用操作
---

```js
// 移除数组中一个元素
Array.prototype.remove = function (val) {
  var index = this.indexOf(val);
  if (index > -1) {
    this.splice(index, 1);
  }
};
// 判断两个数组相等（无视顺序）
Array.prototype.equals = function(array) {
  if (!array) return false;
  if (this.length != array.length) return false;
  for (var i = 0, l = this.length; i < l; i++) {
    // Check if we have nested arrays
    if (this[i] instanceof Array && array[i] instanceof Array) {
      // recurse into the nested arrays
      if (!this[i].equals(array[i])) return false;
    } else if (array.indexOf(this[i]) === -1) {
      return false;
    }
  }
  return true;
};
```

### js sort方法根据数组中对象的某一个属性值进行排序
---

sort方法接收一个函数作为参数，这里嵌套一层函数用来接收对象属性名，其他部分代码与正常使用sort方法相同。

```js
var arr = [
  {name: 'a', age: 10},
  {name: 'b', age: 20},
  {name: 'c', age: 30}
];

function compare(property){
  return function(a,b){
    var value1 = a[property];
    var value2 = b[property];
    return value1 - value2;
  }
}
console.log(arr.sort(compare('age')));
```

### 常用的字符串操作
---

#### `charAt()` 返回在指定位置的字符
<!--more-->
```js
var str = "abac_dfra_wa";
console.log(str.charAt(3)); // 输出 c
```
#### `charCodeAt()` 返回在指定的位置的字符的 Unicode 编码

```js
var str = "abac_dfra_wa";
console.log(str.charCodeAt(3)); // 输出 99
```

#### `fromCharCode()` 从字符编码创建一个字符串

```js
console.log(String.fromCharCode(72,69,76,76,79)); // 输出 HELLO
```

#### `concat()` 连接字符串

```js
var str = "abac_dfra_wa";
console.log(str.concat('_000')); // 输出 abac_dfra_wa_000
```

#### `indexOf()` 检索字符串

```js
var str = "abac_dfra_wa"; 
console.log(str.indexOf('ac')); // 输出 2
```

#### `lastIndexOf()` 从后向前搜索字符串

```js
var str = "abac_dfra_wa";
console.log(str.lastIndexOf('ac')); // 输出 2
```

#### `match()` 找到一个或多个正则表达式的匹配

```js
var str="1 plus 2 equal 3";
console.log(str.match('plus')); // plus
console.log(str.match('st')); // null
console.log(str.match(/\d+/g)); // ['1', '2', '3']
```

#### `replace()` 替换与正则表达式匹配的子串

```js
var str="Hello WoRlD!";
console.log(str.replace(/WoRlD/, "World")); // Hello World!

var str="Hello WoRlD! ";
str += str;
console.log(str.replace(/WoRlD/g, "World")); // 替换所有, 输出 Hello World! Hello World! 

var str = "javascript Tutorial ";
console.log(str.replace(/javascript/i, "JavaScript"));// 确保匹配字符串大写字符的正确

var name = "Doe, John";
console.log(name.replace(/(\w+)\s*, \s*(\w+)/, "$2 $1")); // 将把 "Doe, John" 转换为 "John Doe" 的形式
```

#### `search()`  检索与正则表达式相匹配的值(大小写敏感)，未找到输出 -1

```js
var str="Hello World!";
console.log(str.search(/World/)); // 输出 6

var str="Hello World!";
console.log(str.search(/world/i)); // 忽略大小写的检索，输出 6
```

#### `slice()` 提取字符串的片断，并在新的字符串中返回被提取的部分

```js
var str="Hello happy world!";
console.log(str.slice(6)); // 输出 happy world!
console.log(str.slice(6, 11)); // 输出 happy
```

#### `split()` 把字符串分割为字符串数组

```js
"|a|b|c".split("|") // 返回 ["", "a", "b", "c"]

"How are you doing today?".split(" ", 3) // 返回 How,are,you

"hello".split("")	// 返回 ["h", "e", "l", "l", "o"]
```
