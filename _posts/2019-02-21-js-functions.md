---
layout: post
title: 一些JS方法
categories: [JS]
description: 一些JS方法
keywords: JS, 方法
date: 2019-02-21
---

##### 获取url传递参数的方法

```js
function getUrlParam (name) {
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
    var r = window.location.search.substr(1).match(reg);
    if (r != null) return unescape(r[2]);
    return null;
}
```

##### 获取元素的坐标

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

##### 手机号校验和替换中间4位

```js
// 正则表达式中的括号即可用于分组，同时也用于定义子模式串，在replace()方法中，参数二中可以使用$n(n为数字)来依次引用模式串中用括号定义的字串
phonenum.replace(/(\d{3})\d{4}(\d{4})/, '$1****$2');
// 手机号校验
phonenum.match(/^[1][34587]\d{9}$/);
```

##### 姓名显示最后一字

```js
var str = '王大锤';
// 方法一
return new Array(str.length).join('*') + str.substr(-1);
// 方法二
return str.replace(/.(?=.)/g, '*');
```

##### 获取是否联网
`window.navigator.onLine`

#### js sort方法根据数组中对象的某一个属性值进行排序

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
console.log(arr.sort(compare('age')))
```
