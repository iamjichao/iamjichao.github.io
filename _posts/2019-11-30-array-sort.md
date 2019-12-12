---
layout: post
title: 数组排序
categories: JavaScript
description: 数组排序
keywords: 数组, 排序
---

### 使用 sort 对数组进行排序

```
arrayObject.sort(sortby)
```

- sortby：可选，规定排序顺序，必须是函数。

- 数组在原数组上进行排序，不生成新数组。

- 如果调用该方法时没有使用参数，将按字母顺序（字符编码的顺序）对数组中的元素进行排序。

- 如果想按照其他标准进行排序，就需要提供比较函数，比较函数应该具有两个参数 a 和 b。若 a 小于 b，在排序后的数组中 a 应该在 b 之前，则返回一个小于 0 的值。若 a 等于 b，则返回 0。若 a 大于 b，则返回一个大于 0 的值。

### 对简单数组排序

```js
const testArray = [1, 5, 9, 7, 3];

function sortNumber(value1, value2) {
  return value1 - value2 // 升序
  // return value2 - value1 // 降序
}

testArray.sort(sortNumber);
```

### 根据 Object 中的指定字段对 Object 数组进行排序

```js
const testArray = [
  {name: 'Jack', age: 26},
  {name: 'Amy', age: 30},
  {name: 'Paul', age: 25},
  {name: 'Lucy', age: 36}
];

function sortByItem(itemName) {
  return function (object1, object2) {
    const value1 = object1[itemName];
    const value2 = object2[itemName];
    return value1 - value2 // 升序
    // return value2 - value1 // 降序
  }
}

testArray.sort(sortByItem('age'));
```

以上。
