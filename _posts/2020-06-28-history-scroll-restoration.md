---
layout: post
title: history.scrollRestoration
categories: [JavaScript]
description: history.scrollRestoration
keywords: [页面滚动, scrollRestoration]
---

当我们刷新页面时，浏览器在默认情况下会记住当前的滚动位置，但在有些场景下，我们不需要记住之前的滚动位置，每次刷新都定位到最顶端，这时候应该怎样实现呢？

### 硬实现

```js
// 页面 load 完毕执行
document.scrollingElement.scrollTop = 0;
```

虽然功能实现了，但是页面会产生晃动，用户体验不太好。

### 优雅的实现方式

浏览器提供了原生的 API 用来设置滚动位置的恢复行为：[`history.scrollRestoration`](https://developer.mozilla.org/zh-CN/docs/Web/API/History/scrollRestoration)

这个 API 有两个值：

- `auto` 浏览器默认行为，恢复页面的滚动位置
- `manual` 不恢复页面的滚动位置

在任意位置添加以下代码，即可实现刷新定位到初始位置，防止自动恢复页面的滚动位置。

```js
if (history.scrollRestoration) {
  history.scrollRestoration = 'manual';
}
```

这个 API 的兼容性非常好，即使一些老版本浏览器不支持，也不影响用户的正常使用，所以不用担心兼容性问题。

![image](https://fehub.net/images/posts/2020/history-scroll-restoration-1.png)

以上。

### 参考资料

- [History.scrollRestoration - MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/History/scrollRestoration)
