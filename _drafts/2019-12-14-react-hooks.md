---
layout: post
title: React Hoooks
categories: [JavaScript, React]
description: React Hoooks
keywords: React, Hoooks
---

## React Hoooks

### useState

一个参数：initialState

`const [count, setCount] = useState(0)`

### useEffect

1. 有两个参数 callback 和 dependencies 数组
2. 如果 dependencies 不存在，那么 callback 每次 render 都会执行
3. 如果 dependencies 存在，只有当它发生了变化， callback 才会执行
4. 如果 dependencies 是一个空数组，callback 只在 componentDidMount 时执行一次

```jsx
useEffect(() => {
  console.log(count);
  return () => {
    console.log('componentWillUnmount);
  }
}, [count]);
```


Q：为什么只能在函数最外层调用 Hook？为什么不要在循环、条件判断或者子函数中调用。

A：memoizedState 数组是按 hook定义的顺序来放置数据的，如果 hook 顺序变化，memoizedState 并不会感知到。

Q：自定义的 Hook 是如何影响使用它的函数组件的？

A：共享同一个 memoizedState，共享同一个顺序。

Q：“Capture Value” 特性是如何产生的？

A：每一次 ReRender 的时候，都是重新去执行函数组件了，对于之前已经执行过的函数组件，并不会做任何操作。

Q: memoizedState，cursor 是存在哪里的？如何和每个函数组件一一对应的？

A: react 会生成一棵组件树（或Fiber 单链表），树中每个节点对应了一个组件，hooks 的数据就作为组件的一个信息，存储在这些节点上，伴随组件一起出生，一起死亡。
