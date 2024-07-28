---
layout: post
title: React Hooks 依赖项比较
categories: [React]
description: React Hooks 依赖项比较
keywords: [React, Hooks, 依赖项]
---

在使用 React Hooks 中的 `useEffect`  `useCallback` `useMemo` 等钩子函数时，需要传递一个依赖项数组，用于指定钩子函数的执行时机和条件。React 会比较依赖项数组中的每个元素，判断它们是否发生了变化，如果有变化则会执行对应的钩子函数。

### 变量的比较

在 JavaScript 中，变量可以保存两种类型的值：基本数据类型和引用数据类型。

基本数据类型包括数字、字符串、布尔值、null、undefined 和 Symbol。基本数据类型的值在内存中是独立存储的，它们的值是不可变的，因此比较它们是否相等只需要比较它们的值即可。

引用数据类型包括对象、数组、函数等。引用数据类型的值是保存在内存中的对象，变量存储的实际上是对象的地址（或称为引用），而不是对象本身的值。因此，在比较两个对象时，浅比较（Shallow Comparison）需要比较它们的引用是否相等，如果两个对象引用的是同一个对象，则认为这两个对象相等。深比较（Deep Comparison）则会递归比较对象的所有属性，直到找到基本类型的属性或者属性数量不相等为止，如果两个对象的所有属性都相等，则认为这两个对象相等。

例如，下面的代码创建了两个对象，虽然它们的属性值相同，但它们是两个不同的对象，存储在不同的内存位置上：

```jsx
const obj1 = { name: 'Alice', age: 20 };
const obj2 = { name: 'Alice', age: 20 };

console.log(obj1 === obj2);// false
```

在上面的代码中，我们使用 `===` 运算符来比较两个对象是否相等，结果是 `false`，因为它们的引用不同。

### 依赖项的浅比较

在比较依赖项数组中的元素时，React 会使用浅比较。具体来说，React 会比较每个元素的值是否相等，如果是基本数据类型，则比较它们的值是否相等；如果是引用数据类型，则比较它们的引用是否相等。这意味着如果两个对象的属性值相同，但它们的引用不同，React 仍然会认为它们不相等，从而重新执行钩子函数。而深比较通常用在需要比较嵌套结构的对象时，例如在 React 中比较两个组件的 props 是否相等。

### 浅比较的优缺点

浅比较的优点是效率高，因为它只比较一层，不需要递归比较嵌套数据的每个元素。但也有缺点，就是无法检测到嵌套数据的深层次变化。如果依赖项中包含了嵌套的数据结构，则可能会导致钩子函数不会在预期的情况下执行。

为了避免这种情况，可以使用深比较，例如使用 `lodash` 提供的 `_.isEqual` 函数来比较嵌套数据的每个元素。但需要注意的是，深比较会降低效率，因为它需要递归比较每个元素。因此，只有在必要的情况下才应该使用深比较。

以上。