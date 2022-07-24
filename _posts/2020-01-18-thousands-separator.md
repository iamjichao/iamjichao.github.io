---
layout: post
title: web 开发中的千位分隔符
categories: [HTML, JavaScript]
description: web 开发中的千位分隔符
keywords: [HTML, JavaScript]
---

> 千位分隔符，其实就是数字中的逗号。依西方的习惯，在数字中，每隔三位数加进一个逗号，也就是千位分隔符，以便更加容易认出数值。

在 web 开发中，经常需要实现大数值显示千位分隔符的需求。

### 为什么要使用千位分隔符

- 更加符合国际通用规范

英语中的大数值是千进制的：

```
1,000 / one thousand / 一千
1,000,000 / one million / 一百万（一千千）
1,000,000,000 / one billion / 十亿（一千千千）
```

很多国际单位也是千进制的，使用千位分隔符可以方便单位之间的换算：

```
1 km = 1000 m = 1,000,000 mm
1 kg = 1000 g = 1,000,000 mg
1 s = 1000 ms = 1,000,000 μs
```

- 防止被浏览器识别为电话号码

```HTML
<!-- 没有千位分隔符会被认为是电话号码 -->
<span>123456789</span>

<!-- 有千位分隔符会被认为是数字 -->
<span>123,456,789</span>
```

如果不使用千位分隔符，为了避免将大数值识别为电话号码，通常会在 HTML 中指定 `meta` 标签：

```HTML
<meta name="format-detection" content="telephone=no">
```

需要识别为电话号码的地方使用：

```HTML
<a href="tel:021-88881234">88881234</a>
```

- 能让屏幕阅读器识别更准确

```HTML
<!-- 没有千位分隔符时阅读器读作：一二三四五六七八九 -->
<span>123456789</span>

<!-- 有千位分隔符时阅读器读作：一亿两千三百四十五万六千七百八十九 -->
<span>123,456,789</span>
```

### 如何实现

- `Number.toLocaleString()`

整数部分完美实现，小数部分最多会四舍五入保留三位小数：

```js
(123456789).toLocaleString();
// '123,456,789'

(1234.56789).toLocaleString();
// '1,234.568'
```

- `String(Number).replace(/(\d)(?=(\d{3})+$)/g, "$1,")`

整数可以完美实现，小数就有问题了：

```js
String(123456789).replace(/(\d)(?=(\d{3})+$)/g, "$1,");
// '123,456,789'

String(1234.56789).replace(/(\d)(?=(\d{3})+$)/g, "$1,");
// '1234.56,789'
```

以上两个方法对于整数来说都没问题，但是小数就不能完全满足需求了。对于多位小数，我们可以将小数点两侧分别处理，然后再拼接在一起：

```js
function handleFloat(num) {
   function stringReverse(str = '') {
      return str.split('').reverse().join('');
   }

   const [int, float] = String(num).split('.');
   const i = Number(int).toLocaleString();
   let f = '';

   if (float) {
      const taolf = stringReverse(float);
      f = `.${stringReverse(Number(taolf).toLocaleString())}`;
   }

   return `${i}${f}`;
}

handleFloat(1234.56789);
// '1,234.567,89'
```

以上。

### 参考资料

- [千位分隔符](https://baike.baidu.com/item/%E5%8D%83%E4%BD%8D%E5%88%86%E9%9A%94%E7%AC%A6/10998823?fr=aladdin)

- [Number.prototype.toLocaleString() - MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString)
